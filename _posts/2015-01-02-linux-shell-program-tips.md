---
layout: post
title: "linux shell program tips"
date: 2015-01-02 17:49:19 +0800
description: ""
category: misc
tags: [linux shell script]
---

### 1 $?

$?:return value of last command

example:how to repeat download android source


	repo sync
	while [ $? -ne 0 ] ;
	do
		repo sync
	end

