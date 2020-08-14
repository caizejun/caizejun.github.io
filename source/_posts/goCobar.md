---
title: goCobar
date: 2020-08-06 18:18:53
tags: 
- go
- lib
categories: 
- golang
- lib
---

## 作用

Cobra 是一个提供了简单的接口，可以创建类似 git 和 go tools 的强大的现代 CLI 接口的库。
Cobra 也是一个生成应用脚手架的应用程序，快速开发基于 Cobra 的应用程序。

## 例子

生成一个对单词进行格式转换的指令

### 基本架构

1. cmd目录：里面用来存放命令
   * root.go: 自我理解相当于其他命令的容器，在（init方法）里面添加其他命令，对外提供Execute方法(里面执行root的Execute()方法)，在外面执行Execute,既可以完成注册
   * word.go: 在此文件里对word命令进行注册

2. internal目录：各种指令的具体实现
   * word目录：word.go word命令的具体实现
   * time目录：time.go time命令的具体实现
