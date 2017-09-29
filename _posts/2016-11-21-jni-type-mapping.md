---
layout: post
title: Java Native Type Mapping
date: 2016-11-21 17:49:19 +0800
---

### Type Mapping Signature

|Java Type|Native Type|JNI Name|Signature            |
|:--------|:----------|:-------|:--------------------|
|int      |long       |jint    |I                    |
|long     |int64_t    |jlong   |L                    |
|byte     |int8_t     |jbyte   |B                    |
|boolean  |int8_t     |jboolean|Z                    |
|char     |uint16_t   |jchar   |C                    |
|short    |short      |jshort  |S                    |
|float    |float      |jfloat  |F                    |
|double   |double     |jdouble |D                    |
|void     |void       |void    |V                    |
|Object   |_jobject*  |jobject |                     |
|String   |           |jstring |Ljava/lang/String;   |
|Array    |           |        |[Signature           |
|char[]   |           |        |[C                   |
|int[]    |           |        |[I                   |
|String[] |           |        |[Ljava/lang/String;  |