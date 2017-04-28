---
layout:     post
title:      "yeoman简介与npm发包"
date:       2017-04-28 
author:     "Rabbit"
header-img: "img/post-bg-2015.jpg"
tags:
    - yeoman
    - npm
    - 学习
---



# YEOMAN
yeoman脚手架工具
Generators: 通过`yo`命令来生成文件
## 组织自己的generators
### 设置为一个node module
generator的核心就是node模块

1. 创建一个名为'generator-XX'的文件夹。
	- Yeoman会依旧依赖稳健系统查找可用的generators
2. 文件中创建`package.json`文件
	- 可以使用`npm init`创建
	- name必须要有`generator-`前缀
	- 关键字`keywords`必须是`yeoman-generator`
	- 必须确保使用最新版本的`yeoman-generator`
		- `npm install --save yeoman-generator`
3. 文件目录
	- 可以使用`git clone`[generator-generator](https://github.com/yeoman/generator-generator)该项目，参考其内容
4. 扩展generator
	- 扩展基础generator
		```
			var Generator = require('yeoman-generator');
			module.exports = class extends Geneator{};
		```
5. 覆盖constructor方法
	```
   constructor(args, opts) {
      super(args, opts);
      this.option('babel');
   }
	```
6. 添加自己的功能
	```
	module.exports = class extends Generator {
	   // overwrite constructor
	   constructor(args, opts) {
	      super(args, opts);
	      this.option('babel');
	   }
	   
	   method1() {
	      this.log('method 1 just ran');
	   }
	   
	   method2() {
	      this.log('method 2 just ran');
	   }
	};
	```
7. 运行generator
	- 本地开发generator，不能作为全局npm module使用
	- 使用npm创建全局module,并且和本地的建立链接
	- 文件根目录下，运行`npm link`
		- 运行此命令之前需要进行npm发包，发包过程如下
	- link成功之后使用`yo name` 
	- *踩坑*
		- `yo name`命令中一定要使用name,而创建自己的generator必须要以generator-为前缀，即文件名为`generator-name`。则使用`yo name`的时候，只使用后面的name
		- 执行`yo name`之前，要保证yo和generator-name安装在同一目录下，如yo是全局安装，则generator-name也要是全局安装。*yo XXX 的时候，在yo里要执行require('generator-XXX')，因此位置必须统一*



# npm发包
1. 注册账号
2. 添加package.json文件，发包
	- npm publish
	- 踩坑: 因为使用了公司npm镜像，然后莫名一直安装失败，只有使用`npm config set registry https://registry.npmjs.org/`之后，再进行`npm publish`才能成功
	- 心得: 一定要看log文件，比在网上查好得多！！
