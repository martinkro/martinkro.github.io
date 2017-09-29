---
layout: post
title: Generate RSA Key with OpenSSL
date: 2017-01-01 17:49:19 +0800
---

- 生成RSA私钥

```shell
openssl genrsa -out private.pem 2048
```

- 导出公钥

```shell
openssl rsa -in private.pem -outform der -pubout -out public.der
```

- 生成X509证书

```shell
openssl req -new -x509 -key private.pem -out cert.x509.pem -days 10000
```

- 将证书PEM转换为DER

```shell
openssl x509 -in cert.x509.pem  -inform PEM -out cert.x509.der -outform DER
```

- 将私钥转换为pk8格式的私钥

```shell
openssl pkcs8 -in private.pem -topk8 -outform DER -out private.pk8 -nocrypt
openssl pkcs8 -in private.pem -topk8 -outform DER -out private.pk8
```