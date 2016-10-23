---
layout: post
title: "Kramdown 语法学习"
date: 2016-10-23
---

# MD一级标题
## MD二级标题
### MD三级标题

# Markdown基本元素
## 分级标题

# Markdown一级标题  {#fir_id}
## Markdown二级标题 {#sec_id}
### Markdown三级标题 {#thr_id}

## 列表元素

### 基本列表
* kram
+ down

1. kram
2. down
3. now

### 嵌套列表

*   First item

    A second paragraph 另起一段

    * nested list 嵌套列表

    > blockquote 引用块

*   Second item

### 定义列表

术语
: 定义1

: 定义2

## 代码区域

* 前置空格

    * 前有4个空格。下一行#前也有空格。**这个强调不会被编译**

    ## 这一行文本不会被编译成二级标题.
    
* 波浪符号

~~~~~~~
~~~~
波浪符号中的内容不会被转换
~~~~
~~~~~~~~

## 代码高亮

~~~
def what?
  42
end
~~~
{: .language-ruby}

AAAAAAAAAAAAAAAAAA

{% highlight scss %}
.container {
    background: $background-color;
    padding: 0;

    @include media-query($on-palm) {
        background: rgba($background-color, .95);
        width: 100%;
    }
}
{% endhighlight %}


## 图像和链接

引用部分

And here is a referenced ![smile] [smile2]
This is a [reference style link][linkid] to a page.

定义部分

[smile]: /images/cat.jpg

[smile2]: /images/cat.jpg
{: height="36px" width="36px"}

[linkid]: http://gastonsanchez.com/

## 数学公式

$$E = mc^2$$

$$
\begin{align*}
  & \phi(x,y) = \phi \left(\sum_{i=1}^n x_ie_i, \sum_{j=1}^n y_je_j \right)
  = \sum_{i=1}^n \sum_{j=1}^n x_i y_j \phi(e_i, e_j) = \\
  & (x_1, \ldots, x_n) \left( \begin{array}{ccc}
      \phi(e_1, e_1) & \cdots & \phi(e_1, e_n) \\
      \vdots & \ddots & \vdots \\
      \phi(e_n, e_1) & \cdots & \phi(e_n, e_n)
    \end{array} \right)
  \left( \begin{array}{c}
      y_1 \\
      \vdots \\
      y_n
    \end{array} \right)
\end{align*}
$$

