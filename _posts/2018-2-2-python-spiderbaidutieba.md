---
layout:     post
title: 爬取百度贴吧实战
subtitle:  Python爬虫抓取帖子中的回复
date:       2018-2-2
author:     "tomylee"
header-img: "img/star1.jpg"
tags:
    - Python
    - Data Acquisition and Mining
---

#### 在学习python到爬虫时，看到了很多爬取数据的例子，今天自己也来做个小小的练习，主要是熟悉一下整个过程中的步骤。

#### 我的目标是抓取百度贴吧中有关于高中时期的我的一个帖子:laughing: [为何都喜欢李佳](http://tieba.baidu.com/p/3228287632?pn=1)，虽然当时班上同学告诉我时我很莫名其妙，但想一想还是不错的哈哈哈。今天就来把它的内容爬取下来并保存在本地文件。

### 1.分析URL格式
这是这篇帖子的地址：http://tieba.baidu.com/p/3228287632?pn=1，可以分为基础部分和参数部分。
```
http://             代表资源传输使用http协议
tieba.baidu.com     百度的二级域名，指向百度贴吧的服务器;
/p/3138733512       帖子的地址定位符;
?pn=1               该URL的参数，代表帖子页码;

```
### 2.页面抓取

用urllib.request库抓取页面内容。定义一个类名叫BDTB(百度贴吧)，一个初始化方法，一个获取页面的方法。
```python
import urllib.request
import re
import urllib.error

__author__ = "lj"


class BDTB:

	def __init__(self):

		self.base_url = "http://tieba.baidu.com/p/"
		self.default_title = "百度贴吧"
		self.page_index = 1
		self.content_num = 1
		self.file = None

	def get_page_html(self, question_num, pn=1):
		try:
			request = urllib.request.Request(self.base_url + str(question_num) + "?pn=" + str(pn))
			response = urllib.request.urlopen(request)
			return response.read().decode()
		except urllib.error.URLError as e:
			if hasattr(e, "reason"):
				print("出错辣！", e.reason)
			return None
```
可以使用
```
spider = BDTB()
spider.get_page_html(3228287632)
```
来测试提取到的网页内容。

### 3.提取标题、页码、正文内容
通过f12查看网页代码，找到标题、页码、正文内容等所在的代码段，然后进行提取并返回。
```python
        # 在html代码中找到帖子的标题
	def get_title(self, page_html):
		title_pattern = re.compile('''<h1.*?>(.*?)</h1>''', re.S)
		result = re.search(title_pattern, page_html)
		if result:
			return result.group(1)
		return None

	# 在html代码中提取帖子的总页数
	def get_page_num(self, page_html):
		page_num_pattern = re.compile('''回复贴，共.*?>(.*?)<''', re.S)
		result = re.search(page_num_pattern, page_html)
		if result:
			return int(result.group(1))
		return None

	# 从html代码中提取每层楼的内容
	def get_str_contents(self, page_html):
		contents_pattern = re.compile('''d_post_content j_d_post_content  clearfix">(.*?)</div>''', re.S)
		result = re.finditer(contents_pattern, page_html)
		if result:
			str_contents = self.replace(result)
			return str_contents
		return None
```

### 4.替换无关的内容（图片、音频、链接等）
由于只有一个音频内容，我直接把整个的html代码复制过来进行了替换而没有匹配。
```python
        # 替换掉无关的字符串
	def replace(self, result):
		str_contents = []
		for content in result:
			item = re.sub('''<img.*?>|<br>''', "\n", content.group(1))
			item = re.sub('''<a href.*?>|</a>| {4,7}''', "", item)
			item = re.sub('''<div class="voice_player.*?>''', "", item)
			item = re.sub('''<span class="before">&nbsp;</span><span class="middle"><span class="speaker speaker_animate">&nbsp;</span>\n<span class="time">\n   <span class="second">3</span>&quot;\n</span>\n  </span>\n  <span class="after">&nbsp;</span>''', "", item)
			str_contents.append(item.strip())
		return str_contents
```

### 5.文件操作，注意编码问题
```python
        # 打开txt文件
	def open_file(self, title):
		if title:
			self.file = open(title + ".txt", "w+", encoding="utf-8")
		else:
			self.file = open(self.default_title+ ".txt","w+", encoding="utf-8")

	# 写入文件
	def write_file(self, str_contents):
		for str_content in str_contents:
			self.file.write("楼数：" + str(self.page_index) + "\n")
			self.file.write(str_content + "\n\n\n")
			self.page_index += 1
```

### 6.集成一个接口,在外部直接调用这个入口接口即可，并且把每页抓取的内容显示在终端
```python

	def start(self, question_num):
		page_html = self.get_page_html(question_num, self.page_index)
		title = self.get_title(page_html)
		page_num = self.get_page_num(page_html)
		self.open_file(title)
		for i in range(page_num):
			str_contents = self.get_str_contents(page_html)
			print(str_contents)
			self.write_file(str_contents)
			self.page_index += 1
			page_html = self.get_page_html(question_num, self.page_index)
		self.file.close()
```

### 7.完善优化代码
```python
import urllib.request
import re
import urllib.error

__author__ = "lj"


class BDTB:

	def __init__(self):

		self.base_url = "http://tieba.baidu.com/p/"
		self.default_title = "百度贴吧"
		self.page_index = 1
		self.content_num = 1
		self.file = None

	def get_page_html(self, question_num, pn=1):
		try:
			request = urllib.request.Request(self.base_url + str(question_num) + "?pn=" + str(pn))
			response = urllib.request.urlopen(request)
			return response.read().decode()
		except urllib.error.URLError as e:
			if hasattr(e, "reason"):
				print("出错辣！", e.reason)
			return None

	# 在html代码中找到帖子的标题
	def get_title(self, page_html):
		title_pattern = re.compile('''<h1.*?>(.*?)</h1>''', re.S)
		result = re.search(title_pattern, page_html)
		if result:
			return result.group(1)
		return None

	# 在html代码中提取帖子的总页数
	def get_page_num(self, page_html):
		page_num_pattern = re.compile('''回复贴，共.*?>(.*?)<''', re.S)
		result = re.search(page_num_pattern, page_html)
		if result:
			return int(result.group(1))
		return None

	# 从html代码中提取每层楼的内容
	def get_str_contents(self, page_html):
		contents_pattern = re.compile('''d_post_content j_d_post_content  clearfix">(.*?)</div>''', re.S)
		result = re.finditer(contents_pattern, page_html)
		if result:
			str_contents = self.replace(result)
			return str_contents
		return None

	# 替换掉无关的字符串
	def replace(self, result):
		str_contents = []
		for content in result:
			item = re.sub('''<img.*?>|<br>''', "\n", content.group(1))
			item = re.sub('''<a href.*?>|</a>| {4,7}''', "", item)
			item = re.sub('''<div class="voice_player.*?>''', "", item)
			item = re.sub('''<span class="before">&nbsp;</span><span class="middle"><span class="speaker speaker_animate">&nbsp;</span>\n<span class="time">\n   <span class="second">3</span>&quot;\n</span>\n  </span>\n  <span class="after">&nbsp;</span>''', "", item)
			str_contents.append(item.strip())
		return str_contents

	# 打开txt文件
	def open_file(self, title):
		if title:
			self.file = open(title + ".txt", "w+", encoding="utf-8")
		else:
			self.file = open(self.default_title+ ".txt","w+", encoding="utf-8")

	# 写入文件
	def write_file(self, str_contents):
		for str_content in str_contents:
			self.file.write("楼数：" + str(self.page_index) + "\n")
			self.file.write(str_content + "\n\n\n")
			self.page_index += 1

	# 开始入口
	def start(self, question_num):
		page_html = self.get_page_html(question_num, self.page_index)
		title = self.get_title(page_html)
		page_num = self.get_page_num(page_html)
		self.open_file(title)
		for i in range(page_num):
			str_contents = self.get_str_contents(page_html)
			print(str_contents)
			self.write_file(str_contents)
			self.page_index += 1
			page_html = self.get_page_html(question_num, self.page_index)
		self.file.close()

if __name__ == "__main__":
	spider = BDTB()
	spider.start(3228287632)

```

可以参考一下[Python爬虫实战二之爬取百度贴吧帖子](https://cuiqingcai.com/993.html)

### 8.抓取结果
![1](/img/tieba1.jpg)
![1](/img/tieba2.jpg)
虽然基本都不认识我，哈哈哈，毕竟学校太大了。至于现在的我呢，泯然众人。（好像一直也没怎么才华横溢）溜了溜了。
