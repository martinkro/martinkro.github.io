---
layout: post
title: "How to install OpenGrok on Windows"
date: 2016-09-23 17:49:19 +0800
description: "source code online browser"
category: tools
tags: []
---

## 1 Software Requirements

- jdk 8
- apache-tomcat-8.5.5.exe
- ctags58.zip
- opengrok-0.12.1.6.7z

## 2 Generate Tags & Indexs

directory structure

G:\opengrok

|--bin\ctags

|--opengrok-0.12.1.6

|--example

|--|--datroot

|--|--index.bat

G:\sourcecode\example

**index.bat content:**
<pre>
	java -jar ..\opengrok-0.12.1.6\lib\opengrok.jar -w example -W configuration.xml -c ..\bin\ctags.exe -P -S -v  -s ..\..\sourcecode\example -d datroot
</pre>

## 3 Config webapps

- Create directory *G:\Tomcat8.5\webapps\example*
- Extract source.war and copy content to example directory
- Modify example\WEB-INF\web.xml



```xml
	<content-param>
		<param-name>CONFIGURATION</param-name>
		<param-value>G:\opengrok\example\configuration.xml</param-value>
	</content-param>
```xml




