---
title: Typora精通
date: 2024-03-09 11:54:44
description: 关于typora使用教程，非常详细，包括一些拓展应用
tags:
  - 教程
  - typora
categories:
cover: https://typoraio.cn/img/favicon-128.png
top_img: https://img.znxs.vip/study/typora_bg_logo.jpg
---

# Typora（/'taɪpəura/）

![typora](https://img.znxs.vip/study/typora_bg_logo.jpg)

### 一、Typora基本语法指南

[TOC]

### Typora简介

Typora是一款轻便简洁的Markdown编辑器，支持即时渲染技术，这也是与其他Markdown编辑器最显著的区别。即时渲染使得你写Markdown就想是写Word文档一样流畅自如，不像其他编辑器的有编辑栏和显示栏。

- Typora删除了预览窗口，以及所有其他不必要的干扰。取而代之的是实时预览。
- Markdown的语法因不同的解析器或编辑器而异，Typora使用的是GitHub Flavored Markdown。

### Markdown介绍

- Markdown 是一种轻量级标记语言，它允许人们使用易读易写的纯文本格式编写文档。
- Markdown 语言在 2004 由约翰·格鲁伯（英语：John Gruber）创建。
- Markdown 编写的文档可以导出 HTML 、Word、图像、PDF、Epub 等多种格式的文档。
- Markdown 编写的文档后缀为 `.md`, `.markdown`。

## Typora基本技巧

### 常用快捷键

- 加粗： `Ctrl + B`
- 撤销： `Ctrl + Z`
- 字体倾斜 ：`Ctrl+I`
- 下划线：`Ctrl+U`
- 多级标题： `Ctrl + 1~6`
- 有序列表：`Ctrl + Shift + [`
- 无序列表：`Ctrl + Shift + ]`
- 降级快捷键 ：`Tab`
- 升级快捷键：`Shift + Tab`
- 插入链接： `Ctrl + K`
- 插入公式： `Ctrl + Shift + M`
- 行内代码： `Ctrl + Shift + K`
- 插入图片： `Ctrl + Shift + I`
- 返回Typora顶部：`Ctrl+Home`
- 返回Typora底部 ：`Ctrl+End`
- 创建表格 ：`Ctrl+T`
- 选中某句话 ：`Ctrl+L`
- 选中某个单词 ：`Ctrl+D`
- 选中相同格式的文字 ：`Ctrl+E`
- 搜索: `Ctrl+F`
- 搜索并替换 ：`Ctrl+H`
- 删除线 ：`Alt+Shift+5`
- 引用 ：`Ctrl+Shift+Q`
- 生成目录：`[TOC]+Enter`

注：一些实体符号需要在实体符号之前加” \ ”才能够显示

### 菜单

输入[TOC]即可产生菜单，菜单会自动更新

### 区域元素

```text
YAML FONT Matters
```

在文章的最上方输入---，按换行键产生，然后在里面输入内容即可。

### 段落

按换行键`Enter`建立新的一行,按`Shift`+`Enter`可以创建一个比段落间距更小的行间距。可在行尾插入打断线，禁止向后插入

```text
打断线<br/>后面的内容将自动换行
```

### 标题

开头#的个数表示，空格+文字。标题有1~6个级别，#表示开始，按换行键结束

```text
# 一级标题 快捷键为 Ctrl + 1
## 二级标题 快捷键为 Ctrl + 2
......
###### 六级标题 快捷键为 Ctrl + 6
```

### 字体

斜体以**或__括住

```text
*这是斜体字体1*_这是斜体字体2_
```

*这是斜体字体1*
*这是斜体字体2*

加粗
开头`**`，结尾`**`。
或者开头`__`,结尾`__`(两个短横线)。

```text
**这是加粗字体1** __这是加粗字体2__
```

**这是加粗字体1**
__这是加粗字体2__

删除线
开头`~~`，结尾`~~`。

```text
~~这是错误文字~~
```

~~这是错误文字~~

下划线

使用HTML标签<u>下划线</u>

```text
<u>下划线</u>
```

<u>下划线</u>

高亮

==需要自己在偏好设置里面打开这项功能==

```text
==高亮==
不高亮
```

### 代码

行内输入代码块快捷键： `Ctrl + Shift + K`

1. 开头```+语言名，开启代码块，换行键换行，光标下移键跳出
   示例：

```text
print("hello,python!"")
```

1. 用两个\'在正常段落中表示代码  快捷键 **[ctrl+shift+`]**
   例如：

```text
Use the `printf()`function.
```

Use the `printf()`function.

### 数学式

打开Typora选择数学模块

- 点击“段落”—>“公式块”
- 快捷键Ctrl+Shift+m
- `“$$”+回车`

以上三种方式都能打开数学公式的编辑栏。

示例：

```text
输入$，然后按ESC键，之后输入Tex命令，可预览
例如：
$\lim_{x\to\infty}\exp(-x)=0$
```

![img](https://pic2.zhimg.com/80/v2-c0139796d5848b706c9f2f4d79c7a749_720w.webp)

下标使用_

```text
H-2O
```

$$
H_2O
$$

上标使用^

```text
y^2=4
```

$$
y^2=4
$$



### 表情

Typora语法支持添加emoji表情，输入不同的符号码（两个冒号包围的字符）可以显示出不同的表情。

```text
以:开始，然后输入表情的英文单词,以：结尾，将直接输入该表情.例如：
:smile:
:cry:
:happy:
```

:smile::cry::happy:

### 表格

快捷键  `ctrl+T`

开头|+列名+|+列名+|+换行键，创建一个2*2表格，`Ctrl+Enter`可建立新行。

示例：

```text
|第一列|第二列|
```

### 分割线 

输入 `***` 或者 `---`,按换行键换行，即可绘制一条水平线。

```text
***---
```

------

上下是水平线

### 引用

开头>表示，空格+文字，按换行键换行，双按换行跳出

```text
> 引注1
> ···
> 引注2
>还有一行，双按换行键跳出引注模式
```

示例：

> 引注1
> ···
> 引注2还有一行，双按换行键跳出引注模式

普通引用

> 空格 + 引用文字：在引用的文字前加>+空格即可，引用可以嵌套。

```text
> 引用文本前使用 [大于号+空格]
> 这行可以不加，新起一行都要加上哦
>这是引用的内容
>>这是引用的内容
```

示例：

> 引用文本前使用 [大于号+空格]这行可以不加，新起一行都要加上哦
> 这是引用的内容
> 这是引用的内容

列表中使用

```text
* 第一项   
> 引用1    
> 引用2
* 第二项
```

示例：

- 第一项

> 引用1引用2

- 第二项

引用里嵌套引用

```text
> 最外层引用
> > 多一个 
> 嵌套一层引用
> > > 可以嵌套很多层
```

引用里嵌套列表

```text
> - 这是引用里嵌套的一个列表
> - 还可以有子列表
>     * 子列表需要从 - 之后延后四个空格开始
```

引用里嵌套代码块

```text
>     同样的，在前面加四个空格形成代码块
> ```
> 或者使用 ``` 形成代码块
> ```
```

### 脚注

在需要添加脚注的文字后面+[+^+序列+]，注释的产生可以鼠标放置其上单击自动产生，添加信息

或人工添加+[+^+序列+]+:

```text
脚注产生的方法[^footnote].
[^footnote]:这个就是"脚注"
```

脚注的产生方法[1]

### 链接

链接
输入网址，单击链接，展开后可编辑
ctr+单击，打开链接
例如：[https://www.baidu.com](https://link.zhihu.com/?target=https%3A//www.baidu.com)

常用链接方法

```text
文字链接 [链接名称](http://链接网址)网址链接 <http://链接网址>
```

示例效果：百度

超链接

格式1：用[ ]括住要超链接的内容，紧接着用( )括住超链接源+名字，超链接源后面+超链接命名
同样ctrl+单击，打开链接例如：

```text
这是[百度](https://www.baidu.com)官网
```

这是 百度官网格式2：超链接 title可加可不加

```text
This is [an example](http://example.com/ "Title") inline link.
[This link](http://example.net/) has no title attribute.
```

This is an example inline link.This link has no title attribute.

高级链接技巧

使用[+超链接文字+]+[+标签+]，创建可定义链接
ctrl+单击，打开链接。示例1：

```text
这是[百度][id][id]:https://www.baidu.com
```

这是百度示例2：

```text
这个链接用 1 作为网址变量 [Google][1].
这个链接用 yahoo 作为网址变量 [Yahoo!][yahoo].
然后在文档的结尾为变量赋值（网址）  
[1]: http://www.google.com/  
[yahoo]: http://www.yahoo.com/
```

### URLs

用<>括住url，可手动设置url对于标准URLs，可自动识别

```text
<www.baidu.com>
```

<www.baidu.com>

### 序列

开头*/+/- 加空格+文字，可以创建无序序列，换行键换行，删除键+shift+tab跳出
开头1.加空格+后接文字，可以创建有序序列例：

- 第一个无序序列
- 第二个无序序列
- ······

1. 第一个有序序列
2. 第二个有序序列
3. ······

可选序列

开头序列+空格+[ ]+空格+文字，换行键换行，删除键+shift+tab跳出例如：

```text
- [ ] 第一个可选序列
- [ ] 第二个可选序列
- [ ] 第三个可选序列
- [x] 第四个可选序列
```

- 第一个可选序列
- 第二个可选序列
- 第三个可选序列
- 第四个可选序列
  总结：先输入减号，然后输入空格，之后就变成了黑色圆点，在输入[]，在中间加个空格，回车就可以注：任务列表无快捷键，可以点击菜单栏段落，任务列表。

### 图片

> Typora文本文档中有使用图片内容，如果需要发布在各个兼容Markdown的软件平台，需要预先上传文档中的图片至图床，再通过对图床的图片链接调用，才能正常显示，否则各个平台将无法看到该文档图片。
> 免费图床网址：[https://sm.ms/](https://link.zhihu.com/?target=https%3A//sm.ms/)图床设置：[Typora图床自动上传图片设置篇]

1. 手动添加：跟链接的方法区别在于前面加了个感叹号 `!`，这样是不是觉得好记多了呢？

```text
![图片名称](http://图片网址)
```

1. 当然，你也可以像网址那样对图片网址使用变量

```text
这个链接用 1 作为网址变量 [Google][1].
然后在文档的结尾位变量赋值（网址） 

[1]: http://www.google.com/logo.png
```

1. 除了以上2种方式之外，还可以直接将图片拖拽进来，自动生成链接。

```text
![显示的文字](C:\Users\Hider\Desktop\echart.png "图片标题")
![显示的文字](C:\Users\Hider\Desktop\echart.png)
```

## Markdown其他效果测试



```markdown
<details>
    <summary>隐藏内容的标题</summary>
    ## 显示隐藏内容  这个不能使用makedown语法
</details>
```

<details>
    <summary>隐藏内容的标题</summary>
    ## 显示隐藏内容
</details>
<details>   
    <summary>点击时的区域标题：点击查看详细内容</summary>  
    **加粗**
</details>
<span style='color:red'>This is red</span>   //字体颜色
 漢  ㄏㄢˋ  <ruby> 漢 <rt> ㄏㄢˋ </rt> </ruby> // 特殊字
<kbd>Ctrl</kbd>+<kbd>F9</kbd>  // 按键标识
<span style="font-size:2rem; background:yellow;">**Bigger**</span> //字体大小和背景

<font face="微软雅黑" color="red" size="6">字体及字体颜色和大小</font>
<font color="#0000ff">字体颜色</font>

<p align="left">居左文本</p>
<p align="center">居中文本</p>
<p align="right">居右文本</p>





### 导出:post_office:

当你导出时发现乱糟糟的😥，明明是开头，缺是页尾，明明是结尾确实页头，这时候你就需要分页符咯！

```html
<div style="page-break-after:always"></div>
```

<span style="color: #fdbcc0;">直接就这样使用</span> 👇（看不见吧~）

<div style="page-break-after:always"></div>

### 2023年16大引人注目颜色

<div style="display: flex; flex-wrap: wrap; justify-content: space-around;">
        <!-- 第一行 -->
        <div style="width: 20%; margin-bottom: 1em; text-align: center;">
            <div style="height: 50px; background-color: #f46b00; margin-bottom: 5px;"></div>
            <div style="font-size: 0.9em; color: #f46b00;">柚子色 - Tangelo (#f46b00)</div>
        </div>
        <div style="width: 20%; margin-bottom: 1em; text-align: center;">
            <div style="height: 50px; background-color: #fdbcc0; margin-bottom: 5px;"></div>
            <div style="font-size: 0.9em; color: #fdbcc0;">水晶玫瑰粉 - Crystal Rose (#fdbcc0)</div>
        </div>
    <!-- 第二行 -->
    <div style="width: 20%; margin-bottom: 1em; text-align: center;">
        <div style="height: 50px; background-color: #00a92a; margin-bottom: 5px;"></div>
        <div style="font-size: 0.9em; color: #00a92a;">经典绿 - Green (#00a92a)</div>
    </div>
    <div style="width: 20%; margin-bottom: 1em; text-align: center;">
        <div style="height: 50px; background-color: #8ea0ac; margin-bottom: 5px;"></div>
        <div style="font-size: 0.9em; color: #8ea0ac;">夏日之歌蓝 - Summer Song (#8ea0ac)</div>
    </div>
 </div>

<div style="display: flex; flex-wrap: wrap; justify-content: space-around;">
        <!-- 第一行 -->
        <div style="width: 20%; margin-bottom: 1em; text-align: center;">
            <div style="height: 50px; background-color: #f07c62; margin-bottom: 5px;"></div>
            <div style="font-size: 0.9em; color: #f07c62;">桃粉色 - Peach Pink (#f07c62)</div>
        </div>
        <div style="width: 20%; margin-bottom: 1em; text-align: center;">
            <div style="height: 50px; background-color: #00FF00; margin-bottom: 5px;"></div>
            <div style="font-size: 0.9em; color: #00FF00;">绿色 - Green (#00FF00)</div>
        </div>
        <!-- 其他三个颜色元素... -->
 <!-- 第二行 -->
    <div style="width: 20%; margin-bottom: 1em; text-align: center;">
        <div style="height: 50px; background-color: #f6d100; margin-bottom: 5px;"></div>
        <div style="font-size: 0.9em; color: #f6d100;">帝国黄 - Empire Yellow (#f6d100)</div>
    </div>
    <div style="width: 20%; margin-bottom: 1em; text-align: center;">
        <div style="height: 50px; background-color: #c2dc39; margin-bottom: 5px;"></div>
        <div style="font-size: 0.9em; color: #c2dc39;">爱情鸟绿 - Love Bird (#c2dc39)</div>
    </div>
    <!-- 其他两个颜色元素... -->
</div>

<div style="display: flex; flex-wrap: wrap; justify-content: space-around;">
        <!-- 第一行 -->
        <div style="width: 20%; margin-bottom: 1em; text-align: center;">
            <div style="height: 50px; background-color: #587a97; margin-bottom: 5px;"></div>
            <div style="font-size: 0.9em; color: #587a97;">长存的蓝色 - Blue Perennial (#587a97)</div>
        </div>
        <div style="width: 20%; margin-bottom: 1em; text-align: center;">
            <div style="height: 50px; background-color: #cf2972; margin-bottom: 5px;"></div>
            <div style="font-size: 0.9em; color: #cf2972;">红头菜紫 - BeetrootPurple (#cf2972)</div>
        </div>
        <!-- 其他三个颜色元素... -->
 <!-- 第二行 -->
    <div style="width: 20%; margin-bottom: 1em; text-align: center;">
        <div style="height: 50px; background-color: #d11619; margin-bottom: 5px;"></div>
        <div style="font-size: 0.9em; color: #d11619;">烈艳红 - FieryRed (#d11619)</div>
    </div>
    <div style="width: 20%; margin-bottom: 1em; text-align: center;">
        <div style="height: 50px; background-color: #c8e2e3; margin-bottom: 5px;"></div>
        <div style="font-size: 0.9em; color: #c8e2e3;">天光蓝 -ShyLight (#c8e2e3)</div>
    </div>
    <!-- 其他两个颜色元素... -->
</div>

<div style="display: flex; flex-wrap: wrap; justify-content: space-around;">
        <!-- 第一行 -->
        <div style="width: 20%; margin-bottom: 1em; text-align: center;">
            <div style="height: 50px; background-color: #f6d6c3; margin-bottom: 5px;"></div>
            <div style="font-size: 0.9em; color: #f6d6c3;">香草奶油粉 - VanillaCream (#f6d6c3)</div>
        </div>
        <div style="width: 20%; margin-bottom: 1em; text-align: center;">
            <div style="height: 50px; background-color: #d6cace; margin-bottom: 5px;"></div>
            <div style="font-size: 0.9em; color: #d6cace;">丁香灰 - GrayLilac (#d6cace)</div>
        </div>
        <!-- 其他三个颜色元素... -->
 <!-- 第二行 -->
    <div style="width: 20%; margin-bottom: 1em; text-align: center;">
        <div style="height: 50px; background-color: #928f70; margin-bottom: 5px;"></div>
        <div style="font-size: 0.9em; color: #928f70;">韭菜绿 - LeekGreen (#928f70)</div>
    </div>
    <div style="width: 20%; margin-bottom: 1em; text-align: center;">
        <div style="height: 50px; background-color: #ad8467; margin-bottom: 5px;"></div>
        <div style="font-size: 0.9em; color: #ad8467;">玛奇朵棕 - Macchiato (#ad8467)</div>
    </div>
    <!-- 其他两个颜色元素... -->
</div>

<div style="display: flex; flex-wrap: wrap; justify-content: space-around;">
        <!-- 第一行 -->
        <div style="width: 20%; margin-bottom: 1em; text-align: center;">
            <div style="height: 50px; background-color: #4d000a; margin-bottom: 5px;"></div>
            <div style="font-size: 0.9em; color: #4d000a;">波多尔红 - PodoerRed (#4d000a)</div>
        </div>
        <div style="width: 20%; margin-bottom: 1em; text-align: center;">
            <div style="height: 50px; background-color: #002FA7; margin-bottom: 5px;"></div>
            <div style="font-size: 0.9em; color: #002FA7;">克莱因蓝  -     KleinBlue (#002FA7)</div>
        </div>
        <!-- 其他三个颜色元素... -->
 <!-- 第二行 -->
    <div style="width: 20%; margin-bottom: 1em; text-align: center;">
        <div style="height: 50px; background-color: #029f9e; margin-bottom: 5px;"></div>
        <div style="font-size: 0.9em; color: #029f9e;">马尔斯绿 - MarrsGreen (#029f9e)</div>
    </div>
    <div style="width: 20%; margin-bottom: 1em; text-align: center;">
        <div style="height: 50px; background-color: #482d1b; margin-bottom: 5px;"></div>
        <div style="font-size: 0.9em; color: #482d1b;">范戴克棕 - FandiequeBrown (#482d1b)</div>
    </div>
    <!-- 其他两个颜色元素... -->
</div>
