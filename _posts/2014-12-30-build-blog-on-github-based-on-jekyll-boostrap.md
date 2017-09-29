---
layout: post
title: "Build blog on github based on jekyll boostrap"
date: 2014-12-30 17:49:19 +0800
description: ""
category: misc
tags: []
---

### 1 Create a Post

	rake post title="name"

### 2 Create a Page

	rake page name="about.md"

### 3 Intsall Themes

	rake them:install git="https://github.com/jekyllbootstrap/theme-the-program.git"

### 4 Switch Themes
	
	rake theme:switch name="the-program"
