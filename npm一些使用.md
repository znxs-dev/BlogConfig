---
title: npm命令解释
date: 2024-06-30 18:16:37
description: 关于npm命令的一些使用，忘了查
tags:
  - npm
  - memory
categories:
cover: https://ts4.cn.mm.bing.net/th?id=ODLS.11812f16-ec58-43ff-958e-73c00d7321b8&w=32&h=32&qlt=90&pcl=fffffa&o=6&pid=1.2
top_img: https://ts4.cn.mm.bing.net/th?id=ODLS.11812f16-ec58-43ff-958e-73c00d7321b8&w=32&h=32&qlt=90&pcl=fffffa&o=6&pid=1.2
---

### npm的一些使用

`npm install` 和 `npm i` 是一样的,都是安装package.json文件中的依赖包。

安装单独的依赖包时，`npm install \<packageName>`

- `--save` 等同于 `-S` （常用，**可保存在package.json文件中**），

  -S, --save 安装包信息将加入到dependencies（生产阶段的依赖,也就是项目运行时的依赖，就是程序上线后仍然需要依赖）

- `--save-dev` 等同于 `-D`

  -D, --save-dev 安装包信息将加入到devDependencies（开发阶段的依赖，就是我们在开发过程中需要的依赖，只在开发阶段起作用。）

区别:

在用npm install 单独安装 npm 包时，有两种命令参数可以把它们的信息写入 package.json 文件，一个是npm install–save,另一个是 npm install –save-dev，他们表面上的区别是：

**–save 会把依赖包名称添加到 package.json 文件 dependencies 下，**

**–save-dev 则添加到 package.json 文件 devDependencies下** ，譬如：

```json
  "dependencies": {
    "axios": "^1.7.2",
    "mitt": "^3.0.1",
    "pinia": "^2.1.7",
    "vue": "^3.4.21",
    "vue-router": "^4.3.2"
  },
  "devDependencies": {
    "@vitejs/plugin-vue": "^5.0.4",
    "typescript": "^5.2.2",
    "vite": "^5.2.0",
    "vue-tsc": "^2.0.6"
  }
```

不过这只是它们的表面区别。它们真正的区别是：
`dependencies`是**运行时的依赖**，
`devDependencies`是**开发时的依赖**。
即devDependencies 下列出的模块，是我们开发时用的，比如 我们安装 js的压缩包gulp-uglify 时，我们采用的是 “npm install –save-dev gulp-uglify ”命令安装， 因为我们在发布后用不到它，而只是在我们开发才用到它。
