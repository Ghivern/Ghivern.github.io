---
title: 压缩和解压文件
tags: [Linux]
---

> 记录Linux的压缩和解压命令和用法

## 一、gzip和gnuzip(压缩工具)

1. gzip:压缩单个文件命令，压缩文件后缀gz
	
2. gunzip:解压缩由gzip压缩的文件

## 二、tar(打包工具)

1. tar是一个归档命令，可以将多个文件或文件夹归档到一个归档文件;并且可以通过参数决定压缩与否

2. 常用参数 
	* -c(create) : 创建归档文件,参数指定参与归档的文件和文件夹
	* -x(extract) ：从归档文件中提取文件，参数可选；若指定参数，将提取指定文件或文件夹
	* -v(verbose) :显示参与归档的文件
	* -f ：指定操作的归档文件
	* -z ：使用gzip和gunzip压缩和解压缩归档文件
	* -C ：指定目的路径
	* -r :将文件附加到归档文件的尾部，参数指定文件(压缩文档不可执行此操作)  tar -r test.c -f archieve.tar
	* -t :显示归档文件中的文件列表   tar -tf archieve.tgz
	* -u(update) :更新归档文件中的内容(压缩文档不可执行此操作)  tar -u test.c -f archieve.tar
	* -d(different) :比较归档文件和指定文件系统中文件的不同(非压缩文档不可，只能指定单个文件)  tar -df test.tgz ./
	* --delete :无短参数，从归档文件中删除某(些)项,只能从后往前删除，同名项目将全部被删除(压缩文档不能执行此操作)  tar -f archieve.tar --delete test.c

3. tar -czvf test.tgz ./*.txt  

4. tar -xzvf test.tgz -C ./archieve

5. 归档文件一般后缀为tar;若进行压缩，则tar.gz或tgz

## 三、bzip2(暂时不用，日后补充）

## 四、cpio