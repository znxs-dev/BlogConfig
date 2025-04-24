---
title: 简单使用n8n小案例
date: 2025-04-24 18:00:00
description: 前几天看到了这个自动化程序项目，感觉很有意思，花了一上午部署学习了一下，于是有了下面这篇部署构建第一个程序的教程
tags: 
  - 教程
  - n8n
cover: https://i-blog.csdnimg.cn/direct/445a9e80b062436a9a6501339a6edccb.png
top_img: https://i-blog.csdnimg.cn/direct/445a9e80b062436a9a6501339a6edccb.png
---



# 简单使用n8n小案例

前几天看到了这个自动化程序项目，感觉很有意思，花了一上午部署学习了一下，于是有了下面这篇部署构建第一个程序的教程

> Tips：该教程使用了部分AI生成的代码



### 部署

直接使用docker 是最简单的 官网教程：[n8n github](https://github.com/n8n-io/n8n)

```bash
docker volume create n8n_data
docker run -it --rm --name n8n -p 5678:5678 -v n8n_data:/home/node/.n8n docker.n8n.io/n8nio/n8n
```

这里我使用的docker desktop 部署的，直接点点点也挺方便

部署之后点击对应的端口即可：[localhost:5678](http://localhost:5678)



### 注册

这里正常注册就行



### 创建自动化工作流

点击`Create Workflow`创建工作流

进入页面

![create workflow](https://img.znxs.vip/study/image-20250420114303495.png)



这里创建工作流必须要一个启动节点，也就是触发器，没有触发器，完成工作流创建之后是启动不了的

点击中间的+号创建触发器，这里直接选简单任务触发器，就是常见的定时任务

![Schedule Trigger](https://img.znxs.vip/study/image-20250420114505823.png)

这里几个参数就是正常的定时任务参数 定时周期什么的，很简单

上面的简单解释就是 以天为周期，每天为周期，在整点（midnight），在0分钟执行

![触发器](https://img.znxs.vip/study/image-20250420114756537.png)

创建完成之后，点击后面的+号，创建任务，也就是你想执行的任务

这里因教程需要，需要制作一个每日(9-18点)提醒喝水任务，所以就创建一个邮件发送节点

![选择邮件任务](https://img.znxs.vip/study/image-20250420114947229.png)

![邮件任务](https://img.znxs.vip/study/image-20250420115004719.png)

这里需要填入几个参数，发送方邮件地址，接受方邮件地址，以及认证消息（下图）

![image-20250420115415342](https://img.znxs.vip/study/image-20250420115415342.png)





然后保存即可

![image-20250420115533201](https://img.znxs.vip/study/image-20250420115533201.png)





现在可以简单的测试一下了，点击前面的红色按钮（Test workflow） 就能直接执行工作流

> 【注意】：有的人可能发现，我明明是执行的定时任务，为什么他不定时呢
> 这里就是n8n的设计了，直接点击test不是执行定时任务，而是测试任务，主要目的是测试任务可执行性，教程到这，如果你的测试任务没问题，就可以点击页面上方的inactive 按钮开启工作流，这个按钮才是真正的开启定时任务
>
> ![总启动按钮](https://img.znxs.vip/study/image-20250420115851342.png)



到这里本来应该就可以了，但是不符合我们的真是实际需求，每个小时都需要提醒吗？

显然不是，只有白天才需要提醒，所以这里需要修改，这里添加一个函数

![直接点击加号](https://img.znxs.vip/study/image-20250420160238795.png)



搜索 function 找到code 添加

 ![code界面](https://img.znxs.vip/study/image-20250420160349901.png)

这里需要编写函数，执行逻辑，这里直接问ai ，获取当前时间，并且判断是否为当天的9点到18点，是返回时间，否返回null

```js
// 初始化一个空数组，用于存储处理后的数据
const formattedItems = [];

// 遍历所有输入项
for (const item of $input.all()) {
    // 获取当前项中的日期字段（假设字段名为 currentDate）
    const originalDate = new Date(item.json.currentDate);

    if (!isNaN(originalDate.getTime())) { // 检查日期是否有效
        // 转换日期格式为 YYYY-MM-DD HH:mm:ss
        const formattedDate = `${originalDate.getFullYear()}-${String(originalDate.getMonth() + 1).padStart(2, '0')}-${String(originalDate.getDate()).padStart(2, '0')} ${String(originalDate.getHours()).padStart(2, '0')}:${String(originalDate.getMinutes()).padStart(2, '0')}:${String(originalDate.getSeconds()).padStart(2, '0')}`;

        // 判断是否在 9 点到 18 点之间
        const hours = originalDate.getHours();
        const isWithinWorkingHours = hours >= 9 && hours < 18;

        // 将转换后的日期和判断结果添加到新的对象中
        formattedItems.push({
            json: {
                ...item.json, // 保留原始数据
                formattedDate, // 添加新字段：格式化后的日期
                isWithinWorkingHours // 添加新字段：是否在工作时间内
            }
        });
    } else {
        // 如果日期无效，仍然保留原始数据，并标记错误
        formattedItems.push({
            json: {
                ...item.json,
                formattedDate: null, // 标记为无效
                isWithinWorkingHours: false // 无效日期视为不在工作时间内
            }
        });
    }
}

// 返回处理后的数据
return formattedItems;
```



然后再添加一个if判断，白天时间提醒，晚上不提醒  这里的if可以接受前面个节点返回的数据  判断是否存在就行了

![image-20250420160635977](https://img.znxs.vip/study/image-20250420160635977.png)



![总工作流](https://img.znxs.vip/study/image-20250420160736732.png)



### 运行

最后保存，返回主页面，点击inactive就行

![运行](https://i-blog.csdnimg.cn/direct/098b6518615948d9a48d462fd715506c.png)



### 写在最后

自动化工作流纵然比较方便，但是学习和调试时间成本比较大，就这个小小的功能，我从零到一都是花了三个小时，网上教程较少，大伙还得多加学习
