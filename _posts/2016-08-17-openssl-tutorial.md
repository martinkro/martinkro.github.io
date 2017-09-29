---
layout: post
title: "OpenSSL Tutorial"
date: 2016-08-17 17:49:19 +0800
description: ""
category: 
tags: []
---

### 1 Introduction

几种典型的密码交换信息文件格式:

- DER - encoded certificate: .cer/.crt（.cer/.crt是用于存放证书，以二进制形式存放，不含私钥）
- PEM - encoded message: .pem（.pem跟crt/cer的区别是它以ASCII来表示）
- PKCS#12 - Personal Information Exchange: .pfx/.p12（pfx/p12用于存放个人证书/私钥，通常包含保护密码，二进制方式）
- PKCS#10 - Certification Request: .p10（p10是证书请求）
- PKCS#7 - cert request response: .p7r（p7r是CA对证书请求的回复，只用于导入）
- PKCS#7 - binary message: .p7b（p7b以树状展示证书链(certificate chain)，同时也支持单个证书，不含私钥）

#### 1.1 OpenSSL RSA部分命令：
生成rsa密钥
<pre>openssl genrsa -des3 -out prikey.pem</pre>
去除掉密钥文件保护密码
<pre>openssl rsa -in prikey.pem -out prikey.pem</pre>
分离出公钥
<pre>openssl rsa -in prikey.pem -pubout -out pubkey.pem</pre>
对文件进行签名
<pre>openssl rsautl -sign -inkey prikey.pem -in a.txt -out sig.dat</pre>
验证签名
<pre>openssl rsautl -verify -inkey prikey.pem -pubin -in sig.dat -out unsig.dat</pre>
用公钥对文件加密
<pre>openssl rsautl -encrypt -pubin -inkey pubkey.pem -in a.text -out b.text</pre>
用私钥解密
<pre>openssl rsautl -decrypt -inkey prikey.pem -in b.text</pre>
用证书中的公钥加密（未验证）
<pre>opensll rsautl -encrypt -certin -inkey cert1.pem -in a.txt</pre>


#### 1.2 OpenSSL X509部分命令：
打印出证书的内容
<pre>openssl x509 -in cert.pem -noout -text </pre>
打印出证书的系列号 
<pre>openssl x509 -in cert.pem -noout -serial</pre>
打印出证书的拥有者名字
<pre>openssl x509 -in cert.pem -noout -subject </pre>
以RFC2253规定的格式打印出证书的拥有者名字
<pre>openssl x509 -in cert.pem -noout -subject -nameopt RFC2253 </pre>
打印出证书的MD5特征参数
<pre>openssl x509 -in cert.pem -noout -fingerprint </pre>
打印出证书的SHA特征参数
<pre>openssl x509 -sha1 -in cert.pem -noout -fingerprint </pre>
把PEM格式的证书转化成DER格式
<pre>openssl x509 -in cert.pem -inform PEM -out cert.der -outform DER </pre>
把一个证书转化成CSR
<pre>openssl x509 -x509toreq -in cert.pem -out req.pem -signkey key.pem </pre>
给一个CSR进行处理，颁发字签名证书，增加CA扩展项
<pre>openssl x509 -req -in careq.pem -extfile openssl.cnf -extensions v3_ca -signkey key.pem -out cacert.pem </pre>
给一个CSR签名，增加用户证书扩展项 
<pre>openssl x509 -req -in req.pem -extfile openssl.cnf -extensions v3_usr -CA cacert.pem -CAkey key.pem -CAcreateserial </pre>

#### 1.3 RSA
<pre>
openssl genrsa -des3 -out private.pem 2048
openssl rsa -in private.pem -outform PEM -pubout -out public.pem
openssl rsa -in private.pem -out private_unencrypted.pem -outform PEM
</pre>

### 制作Android签名公钥／私钥

- 生成长度为2048位的RSA私钥
<pre>
openssl genrsa -3 -out test.pem 2048
</pre>
- 生成X509格式的公钥证书
<pre>
openssl req -new -x509 -key test.pem -out test.x509.pem -days 10000
</pre>
- 生成符合PKCS8标准的私钥文件
<pre>
openssl pkcs8 -in test.pem -topk8 -outform DER -out test.pk8 -nocrypt
</pre>

- keytool生成密钥 


<pre>keytool -genkey -alias test.keystore -keyalg RSA -validity 10000 -keystore test.keystore</pre>

- make_key生成的密钥对转换为keystore中的密钥 

1、把pkcs8格式的私钥转换为pkcs12格式： 
<pre>openssl pkcs8 -in test.pk8 -inform DER -outform PEM -out test.priv.pem -nocrypt</pre>

2、生成pkcs12格式的密钥文件： 
<pre>openssl pkcs12 -export -in test.x509.pem -inkey test.priv.pem -out test.pk12 -name testkey</pre>

3、生成keystore： 
<pre>keytool -importkeystore -deststorepass android -destkeypass android -destkeystore test.keystore -srckeystore shared.pk12 
srcstoretype PKCS12 -srcstorepass android -alias testkey</pre>

这样就生成了一个名为test.keystore的keystore文件，就可以用这个文件对apk签名。

- keystore中的密钥转换为密钥对 

1、keystore文件转换为pkcs12格式
<pre>keytool -importkeystore -srckeystore test.keystore -destkeystore test.p12 -srcstoretype JKS - deststoretype PKCS12</pre>

2、dump pkcs12 文件
<pre>openssl pkcs12 -in test.p12 -nodes -out test.rsa.pem</pre>

3、以文本形式打开test.rsa.pem，复制“BEGIN CERTIFICATE” “END CERTIFICATE”之间的内容到一个文件 test.x509.pem, 即公钥 

4、复制 “BEGIN RSA PRIVATE KEY”“END RSA PRIVATE KEY” 之间的内容到一个文件test.rsa.pem,然后运行如下命令
<pre>openssl pkcs8 -topk8 -outform DER -in test.rsa.pem -inform PEM -out test.pk8 -nocrypt</pre>

这样就test.x509.pem和test.pk8就生成了.

### PKCS8

pkcs8 - PKCS#8 format private key conversion tool

Convert a private key to PKCS#8 format using default parameters (AES with 256 bit key and hmacWithSHA256):
<pre>
openssl pkcs8 -in key.pem -topk8 -out enckey.pem
</pre>


Convert a private key to PKCS#8 unencrypted format:
<pre>
openssl pkcs8 -in key.pem -topk8 -nocrypt -out enckey.pem
</pre>

Convert a private key to PKCS#5 v2.0 format using triple DES:
<pre>
 openssl pkcs8 -in key.pem -topk8 -v2 des3 -out enckey.pem
</pre>

Convert a private key to PKCS#5 v2.0 format using AES with 256 bits in CBC mode and hmacWithSHA512 PRF:
<pre>
 openssl pkcs8 -in key.pem -topk8 -v2 aes-256-cbc -v2prf hmacWithSHA512 -out enckey.pem
</pre>


Convert a private key to PKCS#8 using a PKCS#5 1.5 compatible algorithm (DES):
<pre>
 openssl pkcs8 -in key.pem -topk8 -v1 PBE-MD5-DES -out enckey.pem
</pre>

Convert a private key to PKCS#8 using a PKCS#12 compatible algorithm (3DES):
<pre>
 openssl pkcs8 -in key.pem -topk8 -out enckey.pem -v1 PBE-SHA1-3DES
</pre>
 
Read a DER unencrypted PKCS#8 format private key:
<pre>
 openssl pkcs8 -inform DER -nocrypt -in key.der -out key.pem
</pre>

Convert a private key from any PKCS#8 encrypted format to traditional format:
<pre>
 openssl pkcs8 -in pk8.pem -traditional -out key.pem
</pre>

Convert a private key to PKCS#8 format, encrypting with AES-256 and with one million iterations of the password:
<pre>
 openssl pkcs8 -in key.pem -topk8 -v2 aes-256-cbc -iter 1000000 -out pk8.pem
</pre>