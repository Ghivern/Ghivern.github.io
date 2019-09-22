---
title: Linux文件操作 --- 标准库函数
tags: [Linux-System-Program]
---

1、standard I/O library function fopen


   function prototype:

   FILE *fopen(const char *filename,const char *mode)

   explain:open a file to connect to one file stream



   parameters:

   filename:the file path you want to open

   mode:the way to operate file

             r     rb    r+    rb+     r+b  

             w   wb   w+   wb+   w+b

             a    ab    a+   ab+    a+b

   note:

     1、b stand for binary file

     2、+ stand for update way

returned value:

     sucess: return a not empty pointer point to FILE structure

     failed:return NULL which was defined in stdio.h 

note:

    the number of pointer point to FILE is limited by FOPEN_MAX defining in stdio.h 

#include <stdio.h>
#include <stdlib.h>
int main(){
   FILE *f;
   f = fopen("./test.txt","w");
   if(f==NULL){
     printf("fopen error\n");
     exit(0);
   }
   return 0;
}
2、standard I/O library function fclose
   function prototype:

   int fclose(FILE *stream)

  note:

    this function can guaranty that all the data in buffer can be outputed before process ending

#include <stdio.h>
#include <stdlib.h>

int main(int argc,cahr **argv){
   FILE *f;
   f = fopen("./test.txt","r+");
   if(f==NULL){
     exit(0);
   }
   fclose(f);
}
3、standard I/O library function fwrite
   function prototype:

  size_t fwrite(void *buf,size_t size,size_t nitems,FILE * stream)

  explain:write data to file stream

  

   parameters:

   buf: the pointer pointing to buffer

   size : the size of each one item

   nitems: the number of item that you want to write to file

   stream:the pointer pointing to structural body FILE



  returned value:

   success: return the number of item that write to file actually

  

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define NUM 30
int main(int argc,char **argv){
    FILE *f;
    int res;
    char buffer[NUM];
    f = fopen("./test.txt","a");
    if(f==NULL){
       exit(0);
    }
    memset(buffer,'\0',NUM);
    strcpy(buffer,"hello world");
    res = fwrite(buffer,sizeof(char),strlen(buffer),f);
    fclose(f);
    return 0;
}
4、standard I/O library function fread
   function prototype:

   size_t fread(void *buf,size_t size,size_t nitems,FILE *stream)

   explain:read data from file stream



   parameter:

   buf: pointer that point to buffer 

   size:the size of one item

   nitems:the number of item that you want to read

   stream:the pointer that point to structural body FILE 



   returned value:

  success: return the number of items read from file to buffer actually.

                 'actually' means that the returned value may less than the parameter we set,the situations where this problem occurs as following statement:

                (1)has read to the end of file

  failed:



#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define NUM 30

int main(){
  FILE *f;
  int res;
  char buffer[NUM];
  f = fopen("./test.txt","r");
  if(f==NULL){
     exit(0); 
  }
  memset(buffer,'\0',NUM);
  res = fread(buffer,sizeof(char),NUM-1,f);
  printf("the data read from file is %s\n",buffer);
  return 0;
}
