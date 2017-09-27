---
title: Apache 配置虚拟主机及开启SSL
id: .nan
categories:
  - web开发
  - 服务器
date: 2017-03-02 06:36:00
tags:
---

1.  在httpd.conf中加载SSL模块

```
LoadModule ssl_module modules/mod_ssl.so
```
    
2.  并在文件末尾添加：

```
# Load config files in the "/etc/httpd/conf.d" directory, if any.（指加载相对路径httpd下的conf.d文件夹内的所有后缀为conf的文件，即额外配置文件）
Include conf.d/*.conf
```
    
3.  在conf.d文件夹下新建一个vhost.conf文件，并添加以下内容：<!--more-->

```
    #http配置
    <VirtualHost *:80>

        DocumentRoot /var/www/www.domain.com

        ServerName www.domain.com

        ErrorLog logs/www.domain.com-error_log

        CustomLog logs/www.domain.com-access_log common

    </VirtualHost>

    #https配置
    <VirtualHost *:443>

        DocumentRoot /var/www/www.domain.com

        ServerName www.domain.com

        ErrorLog logs/www.domain.com-error_log

        CustomLog logs/www.domain.com-access_log common

        #开启SSL
        SSLEngine on

        #公钥
        SSLCertificateFile /var/www/www.domain.com/ssl/public.pem

        #私钥
        SSLCertificateKeyFile /var/www/www.domain.com/ssl/214032540680904.key

        #证书链
        SSLCertificateChainFile /var/www/www.domain.com/ssl/chain.pem

    </VirtualHost>
```
4.  最后一步，重启apache

```
    $ systemctl restart httpd
```

大功告成，请看效果。