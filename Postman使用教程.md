---
title: Postman使用教程
date: 2023-09-05 22:22:02
description: 关于自己讲述的postman的教程，主要是保姆级别的，通俗易懂
tags:
  - postman
  - 软件测试
  - 使用教程
categories:
cover: https://tse4-mm.cn.bing.net/th/id/OIP-C.XS51hMJsQBuFKSKV3-xj_QHaD4?w=303&h=180&c=7&r=0&o=5&dpr=1.3&pid=1.7
top_img: https://tse4-mm.cn.bing.net/th/id/OIP-C.XS51hMJsQBuFKSKV3-xj_QHaD4?w=303&h=180&c=7&r=0&o=5&dpr=1.3&pid=1.7
---

### 参考文献

[全网最全的 postman 工具使用教程 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/401385193)

[Postman 的使用教程（详细）_postman使用教程_一个橙子呀的博客-CSDN博客](https://blog.csdn.net/hong521520/article/details/106614593)

# 接口测试工具-postman 实战

### 什么是接口（Api）

下面就是一个标准的接口

```HTTP
http://localhost:8080/api/user/list?name=John
```

![http请求](https://img.znxs.vip/znxs/znxsznxsimage-20230911103852935.png)



接口也叫数据接口，就是指系统或组件之间的交互点，通过这些交互点可以实现数据的交互。(数据交互的通道)

例如：开发一个班级管理系统，有新增学生接口，删除学生接口，修改学生接口，查询学生接口等等。

班级管理学生的功能（增删改查）+传入参数 这部分就是`接口`的组成

**接口测试**:是对系统或组件之间的接口进行测试，主要是校验数据的交换、传递和控制管理过程，以及相互逻辑依赖关系。

就是在前后端交互之前，为了方便开发的顺利进行，先进行一次接口测试，模拟客户端向服务器发送数据，进行相应的业务，并向客户端返回数据，检查响应数据是否符合预期

### http请求

以下是一个HTTP GET请求的示例：

```JSON
GET /api/user/list HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36
Accept: application/json, text/plain, /
```

以下是一个HTTP POST请求的示例：

```JSON
POST /api/user/list HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36
Accept: application/json
Content-Type: application/json
Content-Length: 29
{"name": "John"}
```

http请求由三部分组成，分别是:请求行、请求头、请求体

> 请求行：请求方法、请求url、http版本协议

常见的请求方法有五种：GET、POST、PUT、DELETE、PATCH

> 请求头：请求的一些详细信息

| 请求头          | 说明                                               |
| --------------- | -------------------------------------------------- |
| Host            | 接受请求的服务器地址，可以是ip端口号，也可以是域名 |
| User-Agent      | 发送请求的应用                                     |
| Connection      | 指定与连接相关的属性，如Connection Keep-Ative      |
| Accept-Charset  | 通知服务端可以发送的编码格式                       |
| Accept-Encoding | 通知服务端可以发送的数据压缩                       |
| Accept-Language | 通知服务端可以发送的语言                           |

> 请求体：请求的携带数据

### 发送请求的流程

简单演示：

![get请求过程](https://img.znxs.vip/znxs/znxsznxs630eca05d7a64e3bbaefebcaf20b3a6e.png)

- 用户可能点击了网页的一个按钮(url)
- 然后网页发起了http数据请求
- 服务器解析数据请求，找到对应的功能处理请求（对数据库数据进行增删改查等一系列操作）
- 服务器获取到数据进行响应请求返回数据给网页
- 网页处理数据返回给用户展示

### 使用测试工具postman

这里就使用常用的接口测试软件postman进行演示

##### postman安装

在官网上下载安装包之后，直接进行安装

Postman下载：https://www.postman.com/downloads/

安装完成之后需要进行登录，点击create account

![登录注册](https://img.znxs.vip/znxs/znxsimage-20230907080849726.png)



然后就会跳转到注册页面，这里网络原因可能有时候跳不过去，可以直接访问https://identity.getpostman.com/signup 这个连接就是注册页面

然后关闭postman输入登录即可

进入postman页面，就是postman的首页，可以点击Workspaces进入工作区，一般都是在这里进行测试接口

![工作区](https://img.znxs.vip/znxs/znxsimage-20230907092458557.png)



### 第一个请求接口

> 可以直接点击+创建接口，但是不建议，因为接口测试工作一般都是整个项目或者整个模块的接口，如果没有一个好的管理，测试工作会非常杂乱无章，影响测试效率。所以要创建集合目录来管理接口

创建集合文件夹，用于保存项目接口，可以编写项目描述

![项目文件夹](https://img.znxs.vip/znxs/znxsimage-20230907093241412.png)



创建模块文件夹，用于保存项目具体模块接口，可以编写模块描述

![模块文件夹](https://img.znxs.vip/znxs/znxsimage-20230907093411131.png)



创建第一个接口

![接口](https://img.znxs.vip/znxs/znxsimage-20230907093447603.png)



这里可以使用我创建的共享接口

```HTTP
http://47.92.126.51:8080/api/list
```

点击send发送请求得到数据

![常用功能](https://img.znxs.vip/znxs/znxsimage-20230907095543646.png)



其中auth用于权限认证，

### 常见的请求接口

> 有四种类型：查询参数的接口，表单类型的接口，json类型的接口以及含有上传文件的接口

##### 查询参数请求接口

就是在url后面加上查询参数

```HTTP
http://localhost:8080/api/user?name=John
```

**postman操作**

1. 新建请求。在请求方法中选择请求方法：GET（默认为GET）

2. 添加请求参数

   1. 方式一，直接在url后面添加?拼接键值对，例如：?name=John

   2. 方式二，点击Params选项卡，在表格中输入键值对自动补充至url拼接

      ![查询参数请求接口](https://img.znxs.vip/znxs/znxsimage-20230907101224304.png)

      

3. 接口URL中输入地址，点击Send按钮就可以发送请求了 。

##### 表单类型请求接口

需要向服务器说明连接的数据类型为表单类型 也就是需要修改请求头`Content-Type`参数为`application/x-www-form-urlencoded`

**postman操作**

1. 新建请求，选择请求方法为POST，输入url

2. 点击Headers 添加一行请求头信息(postman会自动添加，可忽略这步)

   | Key          | Value                             |
   | ------------ | --------------------------------- |
   | Content-type | application/x-www-form-urlencoded |

3. 点击body,**选中**`x-www-form-urlencoded` 填写表单键值对 可以参考请求参数方式填写

4. 点击send发送请求

![表单类型请求接口](https://img.znxs.vip/znxs/znxsimage-20230907113919110.png)



##### 上传文件表单请求接口

> 在实际开发中文件上传需求非常常见，这就需要使用到文件上传`uploadFolder`的请求方式了

**postman操作**

1. 新建请求，选择请求方法为POST，输入url

2. 点击body **选中**`from-data` 填写键值对 因为这里是文件上传 所以需要把key中的text选项改为file，后面的value输入框中就变成了select的按钮，点击按钮就可以选择文件

3. 选择完文件后点击send发送请求

   ![上传文件请求接口](https://img.znxs.vip/znxs/znxsimage-20230907120057806.png)

   

##### json类型请求接口

json是开发过程中最常见的一种情况，即请求的请求体为json数据格式，json有着良好的可读性，易解析，结构化数据等优点 所以常用于数据的**增加**和**修改**操作

```JSON
// json 数据格式
{
	"key":"value",
    "name":"John"
}
```

**postman操作**

1. 新建请求，选择请求方式为POST，输入url

2. 选择body ，选中row 并选中为JSON格式

3. 输入json数据

   ```JSON
   {
   	"name": "李四",
       "age": 18, // 这里age在数据库中为int类型 所以直接输入数字,
       "bj": "软件21304"
   }
   ```

4. 输入完成后，点击send发送请求

   ![json数据请求接口](https://img.znxs.vip/znxs/znxsimage-20230907121456622.png)

   

以上是基本的发送请求的方法

根据不同的业务需求和场景，需要发送对应的请求，这个时候就需要一个文档来说明，就是接口文档

常见的接口文档

------

**1. 查询指定项目属性**

###### 接口功能

> 获取指定学生信息

###### URL

> [http://localhost:8080](http://localhost:8080/)

###### 支持格式

> JSON

###### HTTP请求方式

> GET

###### 请求参数

> | 参数 | 必选 | 类型   | 说明           |
> | ---- | ---- | ------ | -------------- |
> | name | ture | string | 请求的学生名字 |
> | age  | true | int    | 请求的学生年龄 |

###### 返回字段

> | 返回字段 | 字段类型 | 说明                             |
> | -------- | -------- | -------------------------------- |
> | code     | int      | 返回结果状态。0：正常；1：错误。 |
> | data     | obj      | 返回的数据                       |
> | msg      | string   | 返回描述信息                     |

###### 接口示例

> 地址：http://localhost:8080/api/list?name=张三

```JSON
{
    "code": 200,
    "data": [
        {
            "id": 1,
            "name": "张三",
            "age": 18,
            "bj": "软件21304"
        }
    ],
    "message": "查询学生成功"
}
```

还有自动生成类的接口文档工具Swagger 开发中经常用到

可以访问[swagger本项目接口文档](http://47.92.126.51:8080//swagger-ui.html)

`http://47.92.126.51:8080//swagger-ui.html`查看

------

### postman自动化测试

到这里，已经掌握了基本的postman的使用，现在来看看自动化接口测试

##### 接口结果判断

请求返回的结果如果自己自动判断那就很慢，而且会影响效率，需要自动化判断接口结果

在tests里面编写js脚本 用于查看请求接口后的正确性，一般常用的就是断言

- 判断请求返回的响应状态码是否正确
- 查看请求返回的内容是否符合预期
- 查看请求时间是否超时或者过慢(可省略)

因为有全局变量，所以不用写其他的，可以直接获取请求的相关对象

##### 相关对象

- responseCode：返回请求的状态信息（例如：code）
- responseBody：返回请求的数据内容（响应体）
- tests ：键值对，在TestResults显示 查看返回测试结果是否成功
  - key：字符串类型，处理结果的描述（如：code = 200）
  - value：布尔类型，ture 表示测试通过，false反之
- responseTime：请求响应时间
- pm：postman工具的对象，功能比较全面，可以查看postman官方文档

##### 使用Tests

```JS
复制//查看httpCode码
console.log(responseCode.code)
// 判断接口状态码是否为200
tests["接口状态码200"] = responseCode.code === 200;
//判断请求时间
tests["返回时间小于1000毫秒"] = responseTime < 1000;
//返回body转json
console.log(responseBody)  // 请求体数据为json字符串，需要转换成json对象
var data = JSON.parse(responseBody);
//检查json数据
tests['code码必须为200']= data.code==200
复制// 其他常用命令
// 添加全局环境变量
pm.environment.set("variable_key", "variable_value");
// 设置
```

这些功能只能满足一些单一的测试

### 环境变量

全局变量（Global）：全局变量在任意位置都可以使用。

集合变量（Collection）：在集合中的整个请求中都可用，并且与环境无关，因此不根据所选环境进行更改。

环境变量（Environment）：允许针对不同环境定制处理，例如本地开发与测试或生产。但一次只能激活一个环境。

数据变量（Data）：来自外部的CSV 和 JSON 文件，用于定义在通过 Newman 或 Collection Runner 运行集合时可以使用的数据集。

局部变量（Local）：该变量是临时的，只能在请求脚本中访问。局部变量值仅限于单个请求或集合运行，并且在运行完成后不再可用。

### 批量自动化测试

一般做一个完整的项目，不会一个一个的去测试，非常耗费时间和人力，通过项目模块化，采取集成测试的方式,进行单元、单模块的集中测试

1. 进入项目集合首页，点击Run进行集合测试

2. 会出来一个配置页面

   - Iteration：用于设置接口一共要运行的次数
   - Delay：设置每次运行的接口之间的时间间隔（毫秒）
   - Data：上传测试数据文件
   - Save responses：保存响应
   - Keep variable values：保留变量值
   - Run conllection without using stored cooikes：运行集合并不存储cookie
   - Save cookies after collection run：收集运行后的cookies

3. 先运行一次看看,所有配置都默认选项

   可以看到项目的所有断言以及运行情况

   ![项目运行情况](https://img.znxs.vip/znxs/znxsimage-20230911152606813.png)

   

   可以看到通过的有三个，失败的有一个，并且也明确的标注了失败的断言

4. 数据文件

   这样确实很快，但还是不方便，如果想要修改错误，修改测试数据，修改其他的参数等等，还要去接口去一个一个添加，非常麻烦，这里就可以用到`全局变量`

   可以在`Pre-request`或者项目集合文件中设置全局变量

   ```js
   // 设置全局变量
   postman.setGlobalVariable("name","张三")
   postman.setGlobalVariable("age","18")
   ```

   就可以在请求参数中设置`key`，然后在`value`写上data数据的key

   | key  | value      |
   | ---- | ---------- |
   | name | `{{name}}` |
   | age  | `{{age}}`  |

   然后点击刚刚的批量运行键，运行，在配置项选择数据

   可以是csv类型文件或者json文件

   | name | age  |
   | ---- | ---- |
   | 张三 | 18   |
   | 李四 | 19   |

   这个时候，就会根据全局变量来自动执行有几个数据就执行几次

### 请求依赖问题

以上的接口都是比较简单的接口测试，都是并行接口，如果一个接口数据需要另一个接口数据，这个时候就需要线性的接口传递方法来解决

举个栗子：有个老师需要登录，来查看学生进行管理

老师登录之后保存了token(登录认证标识) ，下次登录的时候就不用重新登录了

新建一个模块，因为父接口需要是第一个接口，而postman集合接口执行顺序就是从前往后，所以放在第一个 第二个放放其他接口，后面放子接口

> 如果父接口登录正常，则会携带token，并且跳转至子接口1顺序执行父接口->子接口1->子接口2

![请求依赖](https://img.znxs.vip/znxs/znxsimage-20230912080711574.png)



> 如果父接口登录错误，则不会带token，并且跳转至子接口2，跳过其他接口和子接口1，顺序执行父接口->子接口2

![请求依赖](https://img.znxs.vip/znxs/znxsimage-20230912080510121.png)



从着可以看出，可以通过逻辑判断跳过接口的执行，不过只有在集合Run的时候才会调用，单个执行并不会跳转请求接口

**等价类**

| 输入条件     | 有效等价类    | 无效等价类    |
| ------------ | ------------- | ------------- |
|              | ①用户名不为空 | ②用户名为空   |
| 用户名和密码 | ③密码不为空   | ④密码为空     |
|              | ⑤用户名存在   | ⑥用户名不存在 |
|              | ⑦密码正确     | ⑧密码不正确   |

**测试用例**

| 编号 | 输入数据                            | 覆盖等价类 | 预期     |
| ---- | ----------------------------------- | ---------- | -------- |
| 1    | username:`罗老师`,password:`123456` | ①,③,⑤,⑦    | 登录成功 |
| 2    | username:` `,password:` `           | ②          | 登录失败 |
| 3    | username:`罗老师`,password:` `      | ①,④        | 登录失败 |
| 4    | username:`陈老师`,password:`123456` | ①,③,⑥      | 登录失败 |
| 5    | username:`罗老师`,password:`123`    | ①,③,⑤,⑧    | 登录失败 |
