---
layout: post
title: "How to creat blog on GitHub"
date: 2016-08-25
comments: true
categories:
---

<head>
	<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
	<style type="text/css">
	#customers
		{
		font-family:"Trebuchet MS", Arial, Helvetica, sans-serif;
		width:100%;
		text-align:left;
		font-size:0.9em;
		line-height:1.5;
		}
	ul
		{
		list-style-type: disc;
		list-style-position: inside;		
		}
	</style>
</head>

<div class="css-full-post-content js-full-post-content" id="customers">
本文讲述了笔者在GitHub上建立简单网页的过程，最终达到可以发送文章的效果。
参考的文章包括：
<li><a href="http://www.pchou.info/ssgithubPage/2013-01-03-build-github-blog-page-01.html"> 一步步在GitHub上创建博客主页</a></li>

<ol>
<li>首先需要在GitHub上建立一个拥有gh-pages分支的repository，大致步骤如下：</li>
	<ul>a.注册GitHub的账号</ul>
	<ul>b.在GitHub上新建一个repository目录</ul>
	<ul>c.在新建的repository目录下新建一个gh-pages分支</ul>
	<ul>d.将repository clone到本地</ul>
	<ul>e.按照指定的目录结构新建文件夹，并上传到GitHub的repository上</ul>
	<ul>f.访问指定页面将看到自己的网页</ul>
具体步骤参考：
<a href="http://www.pchou.info/ssgithubPage/2013-01-05-build-github-blog-page-02.html"> 一步步在GitHub上创建博客主页(2)</a>

</ol>

<li>构建自己的页面，直接复制现有的模板即可</li>
	<ul><a href="http://jekyllthemes.org/">模板链接</a></ul>
	<ul><a href="http://jekyllthemes.org/themes/white-paper/">WhitePaper</a></ul>
	<ul>直接覆盖当前gh-pages分支下的内容，上传即可</ul>	<ul>如果github的repository的名字不是whitepaper则可能出错，在header.html和index.html中修改下，将/whitepaper改为/yourRepoName</ul>
	<ul>给index.html添加翻页功能</ul>

</div>
	
	将如下内容加入index.html：
	
```
{% if paginator.previous_page %}
<a href="{{ paginator.previous_page_path | prepend: site.baseurl | replace: '//', '/' }}"Previous</a>
{% endif %}

{% for page in (1..paginator.total_pages) %}
{% if page == paginator.page %}
  <span class="active">{{ page }}</span>
{% elsif page == 1 %}
  <a href="{{ '/index.html' | prepend: site.baseurl | replace: '//', '/' }}">{{ page }}</a>
{% else %}
  <a href="{{ site.paginate_path | prepend: site.baseurl | replace: '//', '/' | replace: ':num', page }}">{{ page }}</a>
{% endif %}
{% endfor %}

{% if paginator.next_page %}
    <a href="{{ paginator.next_page_path | prepend: site.baseurl | replace: '//', '/' }}">Next</a>
{% endif %}
```
