---
title: 文件操作--系统调用
layout: post
tags: [Linux System Program]
---

1.1 open system call
     function prototype:
     int open(const char *path,int oflags)
     
     parameters:
     path:  file name
     
     oflags:  the way to open file.it can be made of not only one value which could be connected by '|'.
               necessary values of oflags:
                O_RDONLY 
                O_WRONLY  
                O_RDWR
               optional values of oflags:
               O_APPEND（附加）
               O_TRUNC（截尾巴）
               O_CREAT,O_EXCL(often use with O_CREAT to guaranty open's creating operation is 
                atomic operation which avoid more than one the same file being created)
               
     retuened value:
              success: return a small integer as file descripton(which could be used by other system calls to operate file)
              fail: return -1(and set glabol variable errno)
              
      note:

              1.the new file descriptor is always the smallest integer which is not used.
              2.the number of file descriptor is limited.

      //e.g.
        int fd;
        fd = open("./test.txt",O_WRONLY|O_APPEND);
        if(fd = -1){
        printf("open error\n");
        exit(0);
        }



1.2 open system call when oflags=O_CREAT
      note:in this condition the third parameter mode must be used
  
      function prototype:
      int open(const char *path,int ofalgs,,mode_t mode)
      
      parameters:
      the  first and second parameter is the same with the above expound.
      mode:defining the file's authority which was created.
                S_IRUSR     S_IWUSR      S_IXUSR
                S_IRGRP    S_IWGRP      S_IXGRP
                S_IROTH    S_IWOTH      S_IXOTH  


      returned value:the same with above expound

//e.g.
int fd;
fd = open(“test.txt”,O_RDONLY|O_CREAT|O_EXCL,S_IRUSR|S_IWUSR|IXUSR|S_IRGRP);
if(fd == -1){
printf(""open error\n);
exit(0);
}


2.close system call

   function prototype:
   int close(int fildes)
   
   parameters:
   fildes: the file descriptor that you want to close   
   
   returned value:
      success:0
      failed:-1

int fd;
fd = open("./test.txt",O_WRONLY|O_CREAT|O_EXCL,S_IWUSR|S_IRUSR|S_IXUSR);
if(fd==-1){
  printf("open error\n");
  exit(0);
}
//write()
close(fd);
  note:

         If you don't close the file descriptor ,when the process ended,linux system while do that for you automatically.

        But it is not suit for the file operation function in C standard I/O library as result of buffer exiting in standard I/O library function.


  3. write system call
  function prototype:
  size_t write(int fildes,const void *buffer,size_t nbytes)
  explain: write nbytes byte to buffer
  
  parameters:
  fildes: the files descriptor 
  buffer:buffer's first address
  nbytes:the byte number that you want to send to buffer
  
  returned value:
      sucess:return the byte number that write to buffer actually
      failed: return -1 (and set global variable errno) 

#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <string.h>
#define NUM 30

int main(int argc,char **argv){
int fd;
int res
char buffer[NUM];
fd = open("./test.txt",O_WRONLY|O_APPEND|O_CREAT|O_EXCL,S_IWUSR|S_IRUSR|S_IXUSR);//open file
memset(buffer,'/0',NUM);  //clear buffer
strcpy(buffer,"hello world");  //assign buffer
res = write(fd,buffer,strlen(buffer)); //write buffer to file by appending way
if(res == -1){
   printf("write error\n");
   exit(0);
}
return 0;
}

4.read system call
function prototype:
size_t read(int fildes,void *buffer,size_t nbyte)
explain:read nbyte byte from buffer

paremeters:
fildes: the files descriptor 
buffer:buffer's first address
nbytes:the byte number that you want to read from buffer

returned value:
      sucess:return the byte number that read from buffer actually
      failed: return -1 (and set global variable errno) 

#include <unistd.h>
#include <string.h>
#include <stdio.h>
#include <stdlib.h>
#define NUM 30

int main(int argc,char **argv){
int fd;
char buffer[NUM];
int res;

fd = open("./test.txt",O_EDONLY);
if(fd==-1){
printf("read error\n");
exit(0);
}
memset(buffer,'\0',NUM);
res = read(fd,buffer,NUM-1);
if(res == -1){
printf("read error\n");
exit(0);
}
printf("the string reading from test.txt is %s\n");
return 0;
}
