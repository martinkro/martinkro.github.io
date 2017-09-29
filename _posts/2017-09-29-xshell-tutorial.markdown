---
layout: post
title: XShell tutorial
date:   2017-09-29 15:35:19 +0800
categories: "blog"
---

## Xshell login without  password

1. 使用xshell 输出密钥
    ```shell
    ssh-keygen -t rsa -f test -C "test-key"
    ```
2. 放置密钥
    ```shell
    mkdir -p /root/.ssh
    chmod 700 /root/.ssh
    cat id_rsa.pub >> authorized_keys
    chmod 600 authorized_keys
    ```
3. restart sshd
    ```shell
    service ssh restart
    /etc/ssh/sshd_config
    PubkeyAuthentication yes
    PasswordAuthentication no
    ChallengeResponseAuthentication no
    ```
    
## LAMP
1. Install Apache
    ```shell
    apt-get install apache2
    ```
2. Install MySQL
    ```shell
    apt-get install mysql-server libapache2-mod-auth-mysql php5-mysql
    mysql_install_db
    /usr/bin/mysql_secure_installation
    ```
3. Install PHP
    ```shell
    apt-get install php5 libapache2-mod-php5 php5-mcrypt
    apt-get install phpmyadmin
    /etc/init.d/apache2 restart
    ```
4. PHPMyAdmin
    ```shell
    /etc/apache2/apache2.conf
    Include /etc/phpmyadmin/apache.conf
    service apache2 restart
    ```
## Apache Virtual Host
- a2ensite
- a2dissite
- a2enmod
- a2dismod

## fdisk
```shell
fdisk /dev/sdc
mkfs.ext4 /dev/sda5
```
    
## Apache https

1. 确认是否安装ssl模块
    是否有mod_ssl.so文件
2. 生成证书和密钥
3. 配置apache
    - 修改httpd-ssl.conf文件
    注意在此文件中配置证书和密钥
    SSLCertificateFile /apache/conf/server.crt
    SSLCertificateKeyFile /apache/conf/server.key
    - 虚拟机设置NameVirtualHost *:443
        l. 修改httpd.conf文件
                LoadModule ssl_module /opt/taobao/install/httpd/modules/mod_ssl.so
        2. 引入ssl配置文件
                Include “/apache/conf/httpd-ssl.conf”
        3. 如果你配置的虚拟机，注意一下端口的访问接受情况
                NameVirtualHost *:80
4. 重新启动apache

用https方式访问，查看是否生效

## QA
1. Trouble downloading packages list due to a “Hash sum mismatch” error
    ```shell
    sudo rm -rf /var/lib/apt/lists/*
    sudo apt-get update
    ```
2. Enable debug apt-get
    ```shell
    apt-get update -o Debug::Acquire::http=true
    ```
3. Enable ssh server
    ```shell
    apt-get install openssh-server
    ```
    
4. xshell transfer file
    ```shell
    sudo apt-get install lrzsz
    rz
    sz
    ```
5. chown 
    ```shell
    chown user:group file
    ```
    
6. Enable root login Ubuntu 12.04
```shell
sudo passwd root
```

7. Shutdown command
```shell
shutdown -h now
```


