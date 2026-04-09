---
title: 基于PHP的DGUT疫情自动打卡平台
date: 2022-04-16
sidebar: auto
tags:
 - PHP
 - DGUT
categories: 
 - 项目
---

# 基于PHP的DGUT疫情自动打卡平台

> 采用PHP进行开发，主要业务为验证和记录用户注册信息，用于使用Python实现自动化打卡。
>
> 项目地址:[https://yqfk.shenghao.xyz/](https://yqfk.shenghao.xyz/)

## 前端

### 注册页

该页面为简单的`HTML`页面，通过`JS`的`Ajax`发送注册请求。

此处利用JS对数据进行有效性校验，实现过滤无效请求

| 功能点     | 需求描述                |
| ---------- | ----------------------- |
| 账号       | 非0开头的7-18为有效数字 |
| 密码       | 无验证                  |
| 邮箱       | 以字母和数字+@+合法域名 |
| 验证码     | 6位有效数字             |
| 获取验证码 | 点击按钮后进入60s等待   |

![image-20230317155619563](http://cdn.derrick51.cn/img/blog/image-20230317155619563.png)

### 注销页

此页面作为系统注销定时打卡的页面，与上页类似。

![image-20230317160005890](http://cdn.derrick51.cn/img/blog/image-20230317160005890.png)

## 后端

### 数据库设计

表users

| 数据名      | 意义     |
| ----------- | -------- |
| id          | 用户id   |
| username    | 用户名   |
| password    | 密码     |
| email       | 邮箱     |
| create_time | 注册时间 |

表verify

| 数据名      | 意义     |
| ----------- | -------- |
| id          | 用户id   |
| email       | 邮箱     |
| authcode    | 验证码   |
| create_time | 发送时间 |

### PHP开发

邮件是用的私人邮箱，因为考虑这个平台也仅仅在100人以下规模，只有一次注册业务。

我用的是`PHPMailer-6.5.4`，配置好`smtp`后可直接发送邮件。

![image-20230317162924029](http://cdn.derrick51.cn/img/blog/image-20230317162924029.png)

### 难点

[中央认证系统](https://auth.dgut.edu.cn)模拟登录，使用了AES-128-CBC加密，这里需要对中央认证系统的JS文件进行查看研究，发现有JS未加密，可直接查看密码加解密流程，由此可解决加密问题。

由于POST表单中含有cookie项，故需对模拟请求的cookie进行保存，同时要提取页面内的`execution，captcha，_eventId，cllt，dllt，lt`等参数，再通过curl模拟请求登录接口。

根据登录接口特性测试，若状态码为302代表重定向到服务页面，说明登录成功。

由此破解中央认证登录接口，可得到中央认证`ticket`及相关登录信息，可使用`OAuth2.0`系统认证的所有校内应用。

> 莞工中央认证系统使用curl请求会返回java错误回显信息，暴露系统路径，严重影响系统安全性。

dgut_login.php 源码：

```php
<?php

class dgutuser{
    public $username;
    public $password;
    public $aes_key;
    public $aes_password;
    // 构造函数获取登录用户
    function __construct($username,$password){
        $this->username = $username;
        $this->password = $password;
    }
    function login_session(){
        $login_url = "https://auth.dgut.edu.cn/authserver/login";  //登录页面地址
        $cookie_file = dirname(__FILE__)."/pic.cookie";  //cookie文件存放位置（自定义）
        $ch = curl_init();
        curl_setopt($ch, CURLOPT_URL, $login_url);
        curl_setopt($ch, CURLOPT_HEADER, 0);
        curl_setopt($ch, CURLOPT_RETURNTRANSFER,1);
        curl_setopt($ch, CURLOPT_COOKIEJAR, $cookie_file);
        curl_setopt($ch,CURLOPT_CONNECTTIMEOUT,60);
        curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, false);
        curl_setopt($ch, CURLOPT_SSL_VERIFYHOST, false);
        $UserAgent = 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/107.0.0.0 Safari/537.36 Edg/107.0.1418.35';
        curl_setopt($ch, CURLOPT_USERAGENT, $UserAgent);
        $data = curl_exec($ch);
        curl_close($ch);
        // 1. 获取pwdEncryptSalt(AES key)和execution
        
        preg_match_all('~id="_eventId".*?value="(.*?)".*?~',$data,$_eventId);
        preg_match_all('~id="cllt".*?value="(.*?)".*?~',$data,$cllt);
        preg_match_all('~id="dllt".*?value="(.*?)".*?~',$data,$dllt);
        preg_match_all('~id="lt".*?value="(.*?)".*?~',$data,$lt);
        preg_match_all('~id="pwdEncryptSalt".*?value="(.*?)".*?~',$data,$pwdEncryptSalt);
        preg_match_all('~id="execution".*?value="(.*?)"~',$data,$execution);

        $_eventId = $_eventId[1][0];
        $cllt = $cllt[1][2];
        $dllt = $dllt[1][0];
        $lt = $lt[1][0];
        $execution = $execution[1][0];

        $this->aes_key = utf8_encode($pwdEncryptSalt[1][0]);

        # 2. 生成加密密码
        // var_dump(openssl_get_cipher_methods());
        
        $pwdstr = utf8_encode($this->randStr(64)).$this->password;
        $encrypted = openssl_encrypt($pwdstr,"aes-128-cbc",$this->aes_key, 1, utf8_encode($this->randStr(16)));
        $this->aes_password = base64_encode($encrypted);


        # 3、发送登录请求
        //post表单
        $post = array(
            "username"=>$this->username,
            "password"=>$this->aes_password,  # base64密码
            "execution"=>$execution,
            "captcha"=>'',
            "_eventId"=>$_eventId,
            "cllt"=>$cllt,
            "dllt"=>$dllt,
            "lt"=>$lt,
        );
        //cookie
        $cookies = $this->readcookie();
        $header = array(
            "Host:auth.dgut.edu.cn",
            "Origin:https://auth.dgut.edu.cn",
            "Referer:https://auth.dgut.edu.cn/authserver/login",
            "User-Agent:Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/107.0.0.0 Safari/537.36 Edg/107.0.1418.35",
            "Cookie:route=".$cookies['route']."; JSESSIONID=".$cookies['jsID']."; "."org.springframework.web.servlet.i18n.CookieLocaleResolver.LOCALE=zh_CN",
        );

        $conn = curl_init();
        curl_setopt($conn, CURLOPT_URL, "https://auth.dgut.edu.cn/authserver/login");
        // curl_setopt($conn,CURLOPT_FOLLOWLOCATION,1);
        curl_setopt($conn,CURLOPT_POST,1);
        curl_setopt($conn, CURLOPT_HTTPHEADER, $header);
        // curl_setopt($conn, CURLOPT_COOKIE, $cookie_file);
        curl_setopt($conn,CURLOPT_POSTFIELDS,http_build_query($post));
        curl_setopt($conn, CURLOPT_RETURNTRANSFER,1);
        curl_setopt($conn,CURLOPT_CONNECTTIMEOUT,60);
        curl_setopt($conn, CURLOPT_SSL_VERIFYPEER, false);
        curl_setopt($conn, CURLOPT_SSL_VERIFYHOST, false);
        
        curl_exec($conn);

        $rescode = curl_getinfo($conn)['http_code'];

        if($rescode == 302){
            return 1;
        }else{
            return 0;
        }

        curl_close($conn);
    }
    /**
     * 从文件中读出cookie
     */
    function readcookie(){
        $filename = "pic.cookie";
        $f = fopen($filename,'r');
        $content = fread($f,filesize($filename));
        $data = array(
            "jsID"=>'',
            "route"=>''
        );
        preg_match_all('~JSESSIONID	(.*?)\n~',$content,$jsID,2);
        preg_match_all('~route	(.*?)\n~',$content,$route,2);
        $data['jsID'] = trim($jsID[0][1]);
        $data['route'] = trim($route[0][1]);
        return $data;
    }
    /**
     * 随机字符串
     * @param $length
     * @return string
     */
    function randStr($length){
        $chars = "ABCDEFGHJKMNPQRSTWXYZabcdefhijkmnprstwxyz2345678";
        $string = '';
        for ($i = 0; $i < $length; $i++) {
            $string .= $chars[mt_rand(0, strlen($chars) - 1)];
        }
        return $string;
    }
}

// $obj = new dgutuser("xxxxx","xxxxx");
// echo $obj->login_session();

?>


```



## 收获

通过此次学习，也了解了前端加密的相关内容，同时对安全认证有一定了解，基于oAuth2.0的机制有初步认知。