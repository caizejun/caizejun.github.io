---
title: linuxCommand
date: 2020-08-27 17:14:31
tags:
categories:
- linux
- fileOperation
---


## 解压缩相关

### tar工具，用于处理后缀名tar, tar.gz, tar.Z, tar.bz2的文件</br>
  
   参数说明:</br>

>-c 建立新的压缩文件
    -r 添加文件到已经压缩的文件</br>
    -u 添加改变了和现有的文件到已经存在的压缩文件</br>
    -x 从压缩的文件中提取文件</br>
    -t 显示压缩文件的内容</br>
    -z 支持gzip解压文件</br>
    -j 支持bzip2解压文件</br>
    -v 显示操作过程</br>
    -k 保留源有文件不覆盖</br>
    -C 切换到指定目录</br>
    -f 指定压缩文件</br>

***压缩文件***</br>

>tar -zcvf test.tar.gz file1 file2 #打包，并以gzip压缩</br>
tar -jcvf test.tar.bz2 file1 file2 #打包，并以bzip2压缩

***解压tar.gz和tar包到当前目录***

>tar -xvf test.tar.gz</br>
tar -xvf test.ta

***解压到指定目录（文件格式同上）***</br>

>tar -xvf test.tar.gz -C dir</br>
tar -xvf test.tar -C dir

### zip工具</br>

***压缩***

涉及的参数</br>

>-d 从压缩文件内删除指定的文件</br>
-f 此参数的效果和指定"-u"参数类似，但不仅更新既有文件，如果某些文件原本不存在于压缩文件内，使用本参数会一并将其加入压缩文件中</br>
 -j 只保存文件名称及其内容，而不存放任何目录名称。 -r 递归处理，将指定目录下的所有文件和子目录一并处理</br>
-u 更换较新的文件到压缩文件内</br>
-v 显示指令执行过程或显示版本信息</br>
-y 直接保存符号连接，而非该连接所指向的文件，本参数仅在UNIX之类的系统下有效</br>

示例
>zip -r test.zip test/ #打包test目录下的文件</br>
zip -rj test.zip test/ #打包test目录下文件，且压缩包不带test目录

***解压***

涉及的参数</br>
>-l 显示压缩文件内所包含的文件</br>
-j 只保存文件名称及其内容，而不存放任何目录名称</br>
-o 以压缩文件内拥有最新更改时间的文件为准，将压缩文件的更改时间设成和该</br>
-v 显示指令执行过程或显示版本信息</br>
-d 指定解压目录，目录不存在会创建</br>

示例</br>
>unzip -o test.zip -d dir #讲test.zip解压到dir目录</br>

## 移动复制等相关

1. mv 移动或者重命名

两个参数</br>
>i 若指定目录也有同名文件，则先询问是否要覆盖旧文件</br>
f 若有重名文件，强制覆盖

示例
>mv aaa bbb 更改aaa文件为bbb</br>
mv info/ logs 将info目录放入logs目录中。注意，如果logs目录不存在，则该命令将info改名为logs</br>
mv /usr/student/*  .  &emsp;将/usr/student下的所有文件和目录移到当前目录下</br>
