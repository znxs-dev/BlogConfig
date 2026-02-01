---
title: 基于python beautifulsoup的爬虫教程
tags:
  - python
  - 爬虫
  - beautiful soup
date: 2025-01-19 10:33:50
description: 针对与request请求式获取网页内容，采用beautiful soup技术进行爬虫
categories:
cover: https://img.znxs.vip/study/202501271030755.png
top_img: https://img.znxs.vip/study/202501271030755.png
---

## 关于python🐍爬虫

首先，爬虫是一项技术类型，任何语言都能做爬虫，只要有相关实现的依赖或者包能提供相关功能，其中python 🐍 爬虫是基于一系列的依赖（包）来实现的，比较基础的有发送请求的request包（python自带）、requests包（第三方）、beautifulsoup4包（用于解析html网页）等等



## beautifulsoup4 基础

> 安装相关包

```bash
pip install beautifulsoup4  # beautifulsoup4 基础包
pip install lxml  # 推荐使用 lxml 作为解析器（速度更快）
pip install requests  # 用于发送网页请求，获取html网页
```

![beautifulsoup](https://img.znxs.vip/study/202501271030755.png)



### 基础用法

BeautifulSoup 用于解析 **HTML** 或 **XML** 数据，并提供了一些方法来导航、搜索和修改解析树。

BeautifulSoup 常见的操作包括**查找标签**、**获取标签属性**、**提取文本**等。

要使用 BeautifulSoup，需要先导入 BeautifulSoup，并将 HTML 页面加载到 BeautifulSoup 对象中。

于是，首先需要获取网页内容，这时候需要使用到requests 库，来获取第三方网页信息

```python
from bs4 import BeautifulSoup
import requests

# 使用 requests 获取网页内容
url = 'https://cn.bing.com/' # 抓取bing搜索引擎的网页内容
response = requests.get(url)

# 使用 BeautifulSoup 解析网页
soup = BeautifulSoup(response.text, 'lxml')  # 使用 lxml 解析器
# 解析网页内容 html.parser 解析器
# soup = BeautifulSoup(response.text, 'html.parser')
```





### 查找标签

BeautifulSoup 提供了多种方法来查找网页中的标签，最常用的包括 **find()** 和 **find_all()**。

- `find()` 返回第一个匹配的标签
- `find_all()` 返回所有匹配的标签

```python
from bs4 import BeautifulSoup
import requests

# 使用 requests 获取网页内容
url = 'https://www.znxs.vip/'  # 抓取bing搜索引擎的网页内容
response = requests.get(url)

# 中文乱码问题  设置编码格式
response.encoding = 'utf-8'

# 若请求成功 进行下一步
if response.status_code == 200:
    # 使用 BeautifulSoup 解析网页
    soup = BeautifulSoup(response.text, 'lxml')  # 使用 lxml 解析器
    # 查找<title>标签
    title_tag = soup.find('title')
    # 打印标题文本
    if title_tag:
        print(title_tag.get_text())
    else:
        print("未找到<title>标签")

    # 查找第一个<a>标签
    first_link = soup.find('a')
    print("第一个a标签",first_link)

    # 获取第一个 <a> 标签的 href 属性
    first_link_href = first_link.get('href')
    print("第一个a标签的 url",first_link_href)

    # 查找所有 <a> 标签
    all_links = soup.find_all('a')
    print(all_links)
```



### 获取标签的文本

通过 **get_text()** 方法，你可以提取标签中的文本内容：

```python
    # 获取标签文本
    # 获取第一个a标签的文本内容
    first_link_text = soup.find('a').get_text()
    print("第一个a标签的文本内容",first_link_text)

    # 获取页面中的所有文本
    all_text = soup.get_text()
    print("所有文本内容",all_text)
```



### 查找子标签和父标签

你可以通过 **parent** 和 **children** 属性访问标签的父标签和子标签：

```python
# 获取当前标签的父标签
parent_tag = first_link.parent

# 获取当前标签的所有子标签
children = first_link.children
```



### 查找具有特定属性的标签

你可以通过传递属性来查找具有特定属性的标签。

例如，查找类名为 example-class 的所有 div 标签：

```python
# 查找所有 class="example-class" 的 <div> 标签
divs_with_class = soup.find_all('div', class_='example-class')

# 查找具有 id="unique-id" 的 <p> 标签
unique_paragraph = soup.find('p', id='unique-id')
```

### CSS 选择器

BeautifulSoup 也支持通过 CSS 选择器来查找标签。

select() 方法允许使用类似 jQuery 的选择器语法来查找标签：

```python
# 使用 CSS 选择器查找所有 class 为 'example' 的 <div> 标签
example_divs = soup.select('div.example')

# 查找所有 <a> 标签中的 href 属性
links = soup.select('a[href]')
```



### BeautifulSoup 属性与方法

以下是 BeautifulSoup 中常用的属性和方法:

| **方法/属性**              | **描述**                                                 | **示例**                                         |
| :------------------------- | :------------------------------------------------------- | :----------------------------------------------- |
| `BeautifulSoup()`          | 用于解析 HTML 或 XML 文档并返回一个 BeautifulSoup 对象。 | `soup = BeautifulSoup(html_doc, 'html.parser')`  |
| `.prettify()`              | 格式化并美化文档内容，生成结构化的字符串。               | `print(soup.prettify())`                         |
| `.find()`                  | 查找第一个匹配的标签。                                   | `tag = soup.find('a')`                           |
| `.find_all()`              | 查找所有匹配的标签，返回一个列表。                       | `tags = soup.find_all('a')`                      |
| `.find_all_next()`         | 查找当前标签后所有符合条件的标签。                       | `tags = soup.find('div').find_all_next('p')`     |
| `.find_all_previous()`     | 查找当前标签前所有符合条件的标签。                       | `tags = soup.find('div').find_all_previous('p')` |
| `.find_parent()`           | 返回当前标签的父标签。                                   | `parent = tag.find_parent()`                     |
| `.find_all_parents()`      | 查找当前标签的所有父标签。                               | `parents = tag.find_all_parents()`               |
| `.find_next_sibling()`     | 查找当前标签的下一个兄弟标签。                           | `next_sibling = tag.find_next_sibling()`         |
| `.find_previous_sibling()` | 查找当前标签的前一个兄弟标签。                           | `prev_sibling = tag.find_previous_sibling()`     |
| `.parent`                  | 获取当前标签的父标签。                                   | `parent = tag.parent`                            |
| `.next_sibling`            | 获取当前标签的下一个兄弟标签。                           | `next_sibling = tag.next_sibling`                |
| `.previous_sibling`        | 获取当前标签的前一个兄弟标签。                           | `prev_sibling = tag.previous_sibling`            |
| `.get_text()`              | 提取标签内的文本内容，忽略所有HTML标签。                 | `text = tag.get_text()`                          |
| `.attrs`                   | 返回标签的所有属性，以字典形式表示。                     | `href = tag.attrs['href']`                       |
| `.string`                  | 获取标签内的字符串内容。                                 | `string_content = tag.string`                    |
| `.name`                    | 返回标签的名称。                                         | `tag_name = tag.name`                            |
| `.contents`                | 返回标签的所有子元素，以列表形式返回。                   | `children = tag.contents`                        |
| `.descendants`             | 返回标签的所有后代元素，生成器形式。                     | `for child in tag.descendants: print(child)`     |
| `.parent`                  | 获取当前标签的父标签。                                   | `parent = tag.parent`                            |
| `.previous_element`        | 获取当前标签的前一个元素（不包括文本）。                 | `prev_elem = tag.previous_element`               |
| `.next_element`            | 获取当前标签的下一个元素（不包括文本）。                 | `next_elem = tag.next_element`                   |
| `.decompose()`             | 从树中删除当前标签及其内容。                             | `tag.decompose()`                                |
| `.unwrap()`                | 移除标签本身，只保留其子内容。                           | `tag.unwrap()`                                   |
| `.insert()`                | 向标签内插入新标签或文本。                               | `tag.insert(0, new_tag)`                         |
| `.insert_before()`         | 在当前标签前插入新标签。                                 | `tag.insert_before(new_tag)`                     |
| `.insert_after()`          | 在当前标签后插入新标签。                                 | `tag.insert_after(new_tag)`                      |
| `.extract()`               | 删除标签并返回该标签。                                   | `extracted_tag = tag.extract()`                  |
| `.replace_with()`          | 替换当前标签及其内容。                                   | `tag.replace_with(new_tag)`                      |
| `.has_attr()`              | 检查标签是否有指定的属性。                               | `if tag.has_attr('href'):`                       |
| `.get()`                   | 获取指定属性的值。                                       | `href = tag.get('href')`                         |
| `.clear()`                 | 清空标签的所有内容。                                     | `tag.clear()`                                    |
| `.encode()`                | 编码标签内容为字节流。                                   | `encoded = tag.encode()`                         |
| `.is_empty_element`        | 检查标签是否是空元素（例如 `<br>`、`<img>` 等）。        | `if tag.is_empty_element:`                       |
| `.is_ancestor_of()`        | 检查当前标签是否是指定标签的祖先元素。                   | `if tag.is_ancestor_of(another_tag):`            |
| `.is_descendant_of()`      | 检查当前标签是否是指定标签的后代元素。                   | `if tag.is_descendant_of(another_tag):`          |

### 其他属性

| **方法/属性** | **描述**                                 | **示例**                    |
| :------------ | :--------------------------------------- | :-------------------------- |
| `.style`      | 获取标签的内联样式。                     | `style = tag['style']`      |
| `.id`         | 获取标签的 `id` 属性。                   | `id = tag['id']`            |
| `.class_`     | 获取标签的 `class` 属性。                | `class_name = tag['class']` |
| `.string`     | 获取标签内部的字符串内容，忽略其他标签。 | `content = tag.string`      |
| `.parent`     | 获取标签的父元素。                       | `parent = tag.parent`       |

### 其他

| **方法/属性**      | **描述**                   | **示例**                                                    |
| :----------------- | :------------------------- | :---------------------------------------------------------- |
| `find_all(string)` | 使用字符串查找匹配的标签。 | `tag = soup.find_all('div', class_='container')`            |
| `find_all(id)`     | 查找指定 `id` 的标签。     | `tag = soup.find_all(id='main')`                            |
| `find_all(attrs)`  | 查找具有指定属性的标签。   | `tag = soup.find_all(attrs={"href": "http://example.com"})` |





