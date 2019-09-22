---
title: Linux文件操作 --- lseek和fseek
tags: [Linux System Program]
---

1、系统调用lseek
  function ptorotype:

  off_t lseek(int fildes,off_t offset,int whence)

  explain:指定文件描述符的读写位置



  parameter:

  fildes: 与操作文件相关的文件描述符

  offset: (off_t)具体实现有关的整数类型，指定whence定义规则下的偏移位置

  whence:指定设置偏移值的方法（SEEK_SET、SEEK_CUR、SEEK_END）

  note: off_t , SEEK_XXX定义在sys/types.h中



  returned value:

  sucess: 返回从文件开始到文件读写指针被设置处的偏移量

  failed: 返回-1

2、标准I/O库函数fseel
  function ptorotype:

  int fseek(FILE *stream,long int offset,int whence)

  explain:指定文件流的读写操作位置



  parameters:

  stream: 与操作文件相关的指向FILE结构体的指针

  offset: 长整形类型，whence定义规则下的偏移位置

  whence :指定设置偏移值的方法(SEEK_SET、SEEK_CUR、SEEK_END)



  returned value:

  success: 返回0

  failed: 返回-1