---
title: Flux 响应式流式处理
date: 2026-02-01 21:27:06
description: 给AI应用开发准备的打基础的Flux知识点
tags:
  - 学习
  - 技术
categories:
cover: https://img.znxs.vip/study/20260201213243302_20260201213243118.jpg
top_img: https://img.znxs.vip/study/20260201213243302_20260201213243118.jpg
---



简单学习参考：[Spring WebFlux：响应式编程的“未来战士”还是“花架子”？](https://juejin.cn/post/7515695400671805491)

Spring 官网：[Web on Reactive Stack](https://docs.spring.io/spring-framework/reference/web-reactive.html)

# Flux 响应式流式处理

响应式编程是一种面向数据流和变化传播的编程范式。这意味着可以在编程语言中很方便地表达静态或动态的数据流，而相关的计算模型会自动将变化的值通过数据流进行传播。

响应式编程基于reactor（Reactor 是一个运行在 Java8 之上的响应式框架）的思想，当你做一个带有一定延迟的才能够返回的io操作时，不会阻塞，而是立刻返回一个流，并且订阅这个流，当这个流上产生了返回数据，可以立刻得到通知并调用回调函数处理数据。

电子表格程序就是响应式编程的一个例子。单元格可以包含字面值或类似"=B1+C1"的公式，而包含公式的单元格的值会依据其他单元格的值的变化而变化。

响应式传播核心特点之一：变化传播：一个单元格变化之后，会像多米诺骨牌一样，导致直接和间接引用它的其他单元格均发生相应变化。

## 响应流的特点

要搞清楚这两个概念，必须说一下响应流规范。它是响应式编程的基石。他具有以下特点：

- 响应流必须是无阻塞的。
- 响应流必须是一个数据流。
- 它必须可以异步执行。
- 并且它也应该能够处理背压



## 函数编程

函数编程的分类

## 基础Lambda表达式 (1-20)

1. 编写一个Lambda表达式，接受两个整数并返回它们的和

2. 编写一个Lambda表达式，判断字符串是否为空

3. 使用Lambda实现Runnable接口

4. 使用Lambda实现Comparator接口排序字符串

5. 使用Lambda实现Predicate接口过滤数字

6. 使用Lambda实现Function接口进行字符串转换

7. 使用Lambda实现Consumer接口打印元素

8. 使用Lambda实现Supplier接口生成随机数

9. 使用方法引用调用静态方法

10. 使用方法引用调用实例方法

11. 使用方法引用调用构造函数

12. 使用Lambda实现BiFunction接口

13. 使用Lambda实现UnaryOperator接口

14. 使用Lambda实现BinaryOperator接口

15. 组合多个Predicate进行复杂条件判断

16. 组合多个Function进行链式转换

17. 使用andThen组合Function

18. 使用compose组合Function

19. 使用Optional处理可能为null的值

20. 使用Optional的map和flatMap方法

## Stream基础操作 (21-40)

1. 从集合创建Stream并遍历
2. 使用filter过滤偶数
3. 使用map将字符串转换为大写
4. 使用flatMap展开嵌套集合
5. 使用distinct去重
6. 使用sorted排序
7. 使用limit限制元素数量
8. 使用skip跳过元素
9. 使用peek查看中间结果
10. 使用forEach遍历Stream
11. 使用toArray将Stream转换为数组
12. 使用reduce计算总和
13. 使用collect收集到List
14. 使用collect收集到Set
15. 使用collect收集到Map
16. 使用collect连接字符串
17. 使用count统计元素数量
18. 使用min查找最小值
19. 使用max查找最大值
20. 使用findFirst获取第一个元素

## Stream高级操作 (41-60)

1. 使用anyMatch判断是否存在满足条件的元素
2. 使用allMatch判断所有元素是否满足条件
3. 使用noneMatch判断没有元素满足条件
4. 使用findAny获取任意元素
5. 使用groupingBy分组
6. 使用partitioningBy分区
7. 使用mapping在collect中转换
8. 使用filtering在collect中过滤
9. 使用flatMapping在collect中展开
10. 使用summingInt计算总和
11. 使用averagingDouble计算平均值
12. 使用summarizingInt获取统计信息
13. 使用joining连接字符串
14. 使用reducing进行归约操作
15. 使用collectingAndThen在收集后转换
16. 使用toMap收集到Map并处理键冲突
17. 使用toConcurrentMap收集到并发Map
18. 使用toCollection收集到特定集合
19. 使用toUnmodifiableList收集到不可变列表
20. 使用toUnmodifiableSet收集到不可变集合

## 数值流和并行流 (61-75)

1. 使用IntStream处理基本类型
2. 使用LongStream处理long类型
3. 使用DoubleStream处理double类型
4. 使用range创建数值范围
5. 使用rangeClosed创建包含边界的范围
6. 使用mapToInt转换为数值流
7. 使用boxed将数值流转换为对象流
8. 使用sum计算数值流总和
9. 使用average计算数值流平均值
10. 使用summaryStatistics获取数值统计
11. 使用parallel创建并行流
12. 使用sequential切换为顺序流
13. 使用unordered优化并行流性能
14. 在并行流中使用线程安全的收集器
15. 处理并行流中的竞态条件

## 高级Stream操作 (76-100)

1. 使用Stream.iterate创建无限流
2. 使用Stream.generate创建无限流
3. 使用Stream.concat连接多个流
4. 使用takeWhile在条件满足时获取元素
5. 使用dropWhile在条件满足时丢弃元素
6. 使用ofNullable创建可能为空的流
7. 使用empty创建空流
8. 使用builder构建流
9. 使用Stream.of创建流
10. 处理流的异常
11. 使用自定义收集器
12. 使用Spliterator控制流的分割
13. 使用流的短路操作优化性能
14. 在流操作中使用final变量
15. 使用流处理文件I/O
16. 使用流处理数据库结果
17. 使用流进行递归操作
18. 使用流实现分页查询
19. 使用流进行数据转换和清洗
20. 使用流进行聚合计算
21. 使用流实现复杂的数据分组
22. 使用流进行性能测试和比较
23. 使用流处理JSON数据
24. 使用流与Optional组合处理复杂数据
25. 使用流实现函数式设计模式













## Java Flux和Mono 100道编程题目

### 基础创建与转换 (1-20)

创建一个包含"Hello", "World"的Flux

创建一个空的Flux

从数组创建Flux  

从Stream创建Flux 

创建包含单个元素的Mono 

创建空的Mono

从Optional创建Mono 

将Flux转换为List

将Mono转换为Flux

将Flux转换为Mono（取第一个元素）

使用justOrEmpty处理null值

使用defer延迟创建Mono

使用fromCallable创建Mono

使用fromRunnable创建Mono

使用fromSupplier创建Mono

创建范围数字的Flux

使用generate创建无限序列

使用create创建自定义Flux

使用push创建单线程Flux

使用interval创建定时序列

### 过滤与映射 (21-40)

过滤Flux中的偶数

使用distinct去重

使用distinctUntilChanged去重连续重复

使用filterWhen进行异步过滤

使用map进行元素转换

使用flatMap展开嵌套结构

使用flatMapSequential保持顺序展开

使用concatMap顺序执行flatMap

使用switchOnFirst根据第一个元素处理

使用handle自定义元素处理

使用index添加索引

使用timestamp添加时间戳

使用elapsed计算时间间隔

使用cast进行类型转换

使用ofType过滤类型

使用scan进行累积计算

使用reduce进行归约

使用collectList收集到List

使用collectMap收集到Map

使用collectMultimap收集到Multimap

### 组合操作 (41-60)

使用concat连接两个Flux

使用merge合并两个Flux

使用zip组合多个Flux

使用combineLatest组合最新值

使用firstWithSignal选择第一个响应的Flux

使用firstWithValue选择第一个有值的Flux

使用then切换为另一个Publisher

使用thenMany切换为另一个Flux

使用thenEmpty切换为空Mono

使用thenReturn返回固定值

使用repeat重复序列

使用repeatWhen条件重复

使用retry重试操作

使用retryWhen条件重试

使用timeout设置超时

使用delayElements延迟发射

使用delaySubscription延迟订阅

使用buffer缓冲元素

使用window窗口化处理

使用groupBy分组处理

### 错误处理 (61-75)

使用onErrorReturn提供默认值

使用onErrorResume回退到另一个Publisher

使用onErrorContinue继续处理

使用onErrorMap转换错误类型

使用doOnError执行副作用

使用doFinally执行最终操作

使用using管理资源

使用onErrorStop停止错误传播

使用retryBackoff指数退避重试

使用timeout回退到备用Flux

使用transformDeferred延迟转换

使用checkpoint添加调试点

使用log记录流事件

使用doOnEach处理每个信号

使用metrics收集指标

### 高级特性 (76-100)

实现自定义Publisher

使用Schedulers控制执行线程

使用publishOn切换下游线程

使用subscribeOn指定订阅线程

使用parallel进行并行处理

使用runOn指定并行线程

实现背压控制

使用limitRate限制请求速率

使用onBackpressureBuffer处理背压

使用onBackpressureDrop丢弃超量元素

使用onBackpressureLatest保留最新

使用context传递上下文

使用transform操作符组合

使用compose操作符组合

实现热序列（Hot Sequence）

使用cache缓存结果

使用replay重放历史数据

使用connect手动连接ConnectableFlux

使用autoConnect自动连接

使用refCount引用计数

实现自定义Processor

使用blockFirst阻塞获取第一个元素

使用blockLast阻塞获取最后一个元素

使用toIterable转换为Iterable

使用toStream转换为Stream
