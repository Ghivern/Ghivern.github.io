---
title: 编译原理 - 求上下文无关文法的first集合和follow集合
tags: [compile principle]
---

```
//define.h
#define NUM 30   //the room for table 

#define LISTNUM 30  //the length of aggregate

#define N 30  //proNum for each VN

```
################################################################
```
//VN.h
#include <stdio.h>
#include <string.h>
#include "define.h"
#include "aggregate.h"


typedef struct{
    char name;
    int state,NumOfPro,position[N],done;
    struct List first,Prof[N],Follow;
}vn;

int Check_states(vn VN_struct_arr[],int NumOfVN);

int Kind(char VN_name[],int NumOfVN,char x);

int forsure(char VN_name[],int NumOfVN,char Pro[]);

void Reset(vn VN_struct_arr[],int NumOfVN);

int Changed_num(vn VN_struct_arr[],int NumOfVN);

void printAllfirstList(vn VN_struct_arr[],int NumOfVN);

void printAllfollowList(vn VN_struct_arr[],int NumOfVN);

void printAllProList(vn VN_struct_arr[],int NumOfVN,char table[][NUM]);

```
#############################################################
```
//VN.c
#include <stdio.h>
#include <string.h>
#include "VN.h"

int Check_states(vn VN_struct_arr[],int NumOfVN){  //1表示完成 ，0 表示未完成
	int i;
    for(i=1;i<=NumOfVN;i++)
      if(VN_struct_arr[i].state=='2')
         return 0;
    return 1;
}

int Kind(char VN_name[],int NumOfVN,char x){//0表示终结符  ,1表示` ,   2是VN
	int i,flag=0;
    for(i=1;i<=NumOfVN;i++)
       if(VN_name[i]==x)   flag++;
    if(flag>=1)   return 2;
	if(x=='`')    return 1;
	return 0;
}

int forsure(char VN_name[],int NumOfVN,char Pro[]){
	int i=1,k,flag0=0,flag1=0,flag2=0;
	while(Pro[i] != '\n'){
		k=Kind(VN_name,NumOfVN,Pro[i]);
		if(k==0)  flag0++;
		else if(k==1)  flag1++;
		else   flag2++;
		i++;
	}
     if(flag0>0)   return 0;
	 else if(flag2==0)   return 1;
	 else   return 2;
}

void Reset(vn VN_struct_arr[],int NumOfVN){
    int i;
    for(i=1;i<=NumOfVN;i++)
        VN_struct_arr[i].done=0;
}

int Changed_num(vn VN_struct_arr[],int NumOfVN){
    int i,flag=0;
    for(i=1;i<=NumOfVN;i++)
        if(VN_struct_arr[i].done==1)
           flag++;
   return flag; 
}

void printAllfirstList(vn VN_struct_arr[],int NumOfVN)
{
     int i;
	 for(i=1;i<=NumOfVN;i++)
	 {
          printf("FIRST(%c)= ",VN_struct_arr[i].name);
		  printList(VN_struct_arr[i].first);
		  printf("\n");
	 }
}




void printAllfollowList(vn VN_struct_arr[],int NumOfVN)
{
     int i;
	 for(i=1;i<=NumOfVN;i++)
	 {
          printf("FOLLOW(%c)= ",VN_struct_arr[i].name);
		  printList(VN_struct_arr[i].Follow);
		  printf("\n");
	 }
}

void printAllProList(vn VN_struct_arr[],int NumOfVN,char table[][NUM])
{
     int i,j,z;
	 for(i=1;i<=NumOfVN;i++)
	 {
          for(j=1;j<=VN_struct_arr[i].NumOfPro;j++)
          {
           z=1;
           printf("FIRST(");
           while(table[VN_struct_arr[i].position[j]][z]!='\n')
           {
              printf("%c",table[VN_struct_arr[i].position[j]][z]);
              z++;
           }
           printf(")=");
		  printList(VN_struct_arr[i].Prof[j]);
		  printf("\n");
          }
	 }
}

```
##############################################################

```
//arrdeal.h
#include <stdio.h>
#include <string.h>
#include "define.h"

int Init(char table[]);

int Inits(char table[][NUM]);

int Input(char table[],char pose[],int pN);

int Inputs(char table[][NUM],char pose[],int pN);

int Char_to_Position(char arr[],char x);

int count_right(char arr[]);

int Char_to_positions(char arr[],char x,int positions[]);

void Char_to_Lines(char arr[][NUM],char x,int tmp[]);

void printChan(char table[][NUM]);

```
###########################################################
```
//arrdeal.c
#include <stdio.h>
#include <string.h>
#include "arrdeal.h"

int Init(char table[])
{
    int i=0;
	for(i=0;i<NUM;i++)
		table[i]='\n';
	return 1;
}

int Inits(char table[][NUM])
{
	int i=0;
	for(i=0;i<NUM;i++)
		Init(table[i]);
	return 1;
}



int Input(char table[],char pose[],int pN){
	char ch;
	int i=0;
	int j=0;
	int flag;
	scanf("%c",&ch);
	  if(ch=='\n')
		  return 0;
    while(ch!='\n'){
		flag=0;
	  for(j=0;j<pN;j++)
	  {
		 if(ch==pose[j])
		 flag++;
	  }
	  if(flag==0)
         table[i++] = ch;
	  scanf("%c",&ch);
	 
	}
	return 1;
}

int Inputs(char table[][NUM],char pose[],int pN){
    int line=0;
    int flag=Input(table[line++],pose,pN);
	while(flag!=0)
	{	
		flag=Input(table[line++],pose,pN);
	}
	return 1;
}



int count_right(char arr[]){
	int i=1;
	while(arr[i]!='\n')
		i++;
	return --i;
}

int Char_to_positions(char arr[],char x,int positions[]){
    int i=1;
    int count=0;
	positions[1]=0;
    while(arr[i] != '\n'){
        if(arr[i]==x){
            count++;
            positions[count]=i;
        } 
        i++;
    }
    positions[0]=count;
	return positions[1];
}

void Char_to_Lines(char arr[][NUM],char x,int tmp[]) { //tmp记录非终结符所在行数，tmp[0]记录个数个数
    int line=0;
    int pointer=1;
	tmp[0]=0;
    while(arr[line][0] != '\n'){
	     if( arr[line][0] != x )
			 line++;
		 else{
		     tmp[pointer]=line;
			 line++;
			 pointer++;
			 tmp[0]++;
		 }
	}
}

void printChan(char table[][NUM]){   
	int k=0,i=0;
	while(table[k][0]!='\n'){
    i=1;
	printf("%c-->",table[k][0]);
	while(table[k][i] != '\n'){
        printf("%c",table[k][i]);
        i++;
    }
	printf("\n");
	k++;
	} 
}

```
#########################################################

```
//aggregate.h
#include <stdio.h>
#include <string.h>
#include "define.h"

struct List{
    char list[LISTNUM];
    int length;
};



void add(struct List *des,struct List *sou);

void add_element(struct List* des,char c);

void sub_element(struct List* des,char c);

void printList(struct List c);

```

########################################################

```
//aggregate.c
#include <stdio.h>
#include <string.h>
#include "aggregate.h"

void add(struct List *des,struct List *sou){
    int i,j;
    int flag;
    int pointer = des->length +1; 

	if((des->length==0 && sou->length==0) || sou->length==0)
	        return;
	else if(des->length==0){
		for(i=1;i<=sou->length;i++)
			  des->list[i] = sou->list[i];
		des->length = sou->length;
	}
	else{
     for(i=1;i<=sou->length;i++){
         flag = 0;
         for(j=1;j<=des->length;j++)
            if(sou->list[i] == des->list[j])
                 flag++;
         if(flag==0)
            des->list[pointer++] = sou->list[i];
     }
     des->length = pointer-1;   
	}
}

void add_element(struct List* des,char c){
    int i;
    int flag=0;
    int length = des->length+1;
	if(des->length==0){
		des->list[1]=c;
		des->length=1;
	}
	else{
      for(i=1;i<=des->length;i++){
        if(des->list[i]==c)
          flag++;
      }
      if(flag==0){
      des->list[length] =c;
      des->length = length;
      }
	}
}

void sub_element(struct List* des,char c){
    int i;
    int pointer=0;
    if(des->length==0)
         return;
    else{
       for(i=1;i<=des->length;i++){
            if(des->list[i]==c)
               pointer = i;
       }
       if(pointer>0){
           for(i=pointer+1;i<=des->length;i++)
              des->list[i-1]=des->list[i];
           des->length--;
       }
    } 
}

void printList(struct List c)
{
    int i;
    printf("{");
    for(i=1;i<=c.length-1;i++)
    {
        printf("%c,",c.list[i]);
    }
    printf("%c",c.list[c.length]);
    printf("}");
}


```

##########################################################

```
//main.c
#include <stdio.h>
#include <string.h>
#include "VN.h"
#include "arrdeal.h"

    char table[NUM][NUM]; 
    char VN_name[NUM]; 
    int tmp[NUM];
    vn VN_struct_arr[NUM]; //存产生式
    int NumOfVN;

int f(char x){
	return Char_to_positions(VN_name,x,tmp);	
}

int States_Init(char VN_name[],char table[][NUM],vn VN_struct_arr[],int tmp[]){
    int pointer=1,line=0,position,i,j;
	char ch;
    for(i=0;i<NUM;i++)
         VN_name[i]='\n';
	ch=table[line][0];
    while(ch != '\n'){   
		position=f(ch);
        if(position==0){
                VN_name[pointer] = ch;
                pointer++;
        }
	line++;
	ch=table[line][0];
    }
    for(i=1;i<=pointer;i++){
        VN_struct_arr[i].name=VN_name[i];
        VN_struct_arr[i].state=2;
        VN_struct_arr[i].first.length=0;
        VN_struct_arr[i].Follow.length=0;
		VN_struct_arr[i].done=1;
        Char_to_Lines(table,VN_name[i],tmp);
        VN_struct_arr[i].NumOfPro=tmp[0];
        for(j=1;j<=tmp[0];j++)
            VN_struct_arr[i].position[j]=tmp[j];
        for(j=1;j<=tmp[0];j++)
          VN_struct_arr[i].Prof[j].length=0;       
    }    
    return pointer-1;
}

void States_Entry(vn VN_struct_arr[],int NumOfVN){
      int flag,i,j,z,f0=0,f1=0,F=0,E=0,co,X=0;
	  char R;
      for(i=1;i<=NumOfVN;i++){
          X=0;
          for(j=1;j<=VN_struct_arr[i].NumOfPro;j++){
              flag = forsure(VN_name,NumOfVN,table[VN_struct_arr[i].position[j]]);
			  if(flag==0 )
				  X++;
			  if(flag==1)
				  VN_struct_arr[i].state=1;	  
		  }
          if(X==VN_struct_arr[i].NumOfPro)
              VN_struct_arr[i].state=0;
      }
     while(Check_states(VN_struct_arr,NumOfVN)==0){
         for(i=1;i<=NumOfVN;i++){  
            if(VN_struct_arr[i].state==2){
			   F=0; E=0;
               for(j=1;j<=VN_struct_arr[i].NumOfPro;j++){
				      f0=0;f1=0;
					  co=count_right(table[VN_struct_arr[i].position[j]]);
                      for(z=1;z<=co;z++){ 
                          R=table[VN_struct_arr[i].position[j]][z];
						  if(VN_struct_arr[f(R)].state==0)
							  f0++;
						  if(VN_struct_arr[f(R)].state==1)
							  f1++;						 
					    } 
					    if(f1==co)
                         F++;
                        if(f0==co)
                         E++;
			    }
			    if(F>0)
                   VN_struct_arr[i].state=1;
			    if(E==VN_struct_arr[i].NumOfPro)
                   VN_struct_arr[i].state=0;
			}
        }
    }  
}



int lookForNot(vn VN_struct_arr[],char VN_name[],int NumOfVN,char Chan[])
{
    int flag,i,count=count_right(Chan);
    for(i=1;i<=count;i++){
           flag = Kind(VN_name,NumOfVN,Chan[i]);
           if(flag==0)			  
              return i;
		   if(flag==2 && VN_struct_arr[f(Chan[i])].state==0)
			     return i;

    }
    return 0; 
}

void First_tmp(vn VN_struct_arr[],char VN_name[],int NumOfVN)
{
    int i,j,z,flag,l,col,count;
    while(Changed_num(VN_struct_arr,NumOfVN)>0)
    {
        Reset(VN_struct_arr,NumOfVN);
        for(i=1;i<=NumOfVN;i++)
        {
            l=VN_struct_arr[i].first.length;
            for(j=1;j<=VN_struct_arr[i].NumOfPro;j++)
            {
               col=lookForNot(VN_struct_arr,VN_name,NumOfVN,table[VN_struct_arr[i].position[j]]);
               count = count_right(table[VN_struct_arr[i].position[j]]);
               if(col==0)  //全是可以取空
               {
                   for(z=1;z<=count;z++)
                   {
                       flag = Kind(VN_name,NumOfVN,table[VN_struct_arr[i].position[j]][z]);
                       if(flag==2){
                        add(&VN_struct_arr[i].first,&VN_struct_arr[f(table[VN_struct_arr[i].position[j]][z])].first);
                        sub_element(&VN_struct_arr[i].first,'`');
                       }
                   }
                   add_element(&VN_struct_arr[i].first,'`');
               }
               else
               {
                   for(z=1;z<=col;z++)
                   {
                       flag = Kind(VN_name,NumOfVN,table[VN_struct_arr[i].position[j]][z]);
                       if(flag==0)
					   {
                           add_element(&VN_struct_arr[i].first,table[VN_struct_arr[i].position[j]][z]);
					      
					   }
                       if(flag==2)
                       {
                        add(&VN_struct_arr[i].first,&VN_struct_arr[f(table[VN_struct_arr[i].position[j]][z])].first);
                        sub_element(&VN_struct_arr[i].first,'`');
                       }
                   }
               }   
            }
            if(l < VN_struct_arr[i].first.length)
                VN_struct_arr[i].done=1;
        }
    }
}

int First(char table[][NUM],vn VN_struct_arr[],int tmp[]){
    int NumOfVN;
    NumOfVN = States_Init(VN_name,table,VN_struct_arr,tmp);
    States_Entry(VN_struct_arr,NumOfVN);
	First_tmp(VN_struct_arr,VN_name,NumOfVN);
    return NumOfVN;
}




void FirstOfPro(vn VN_struct_arr[],char table[][NUM],int NumOfVN)
{
    int i,j,z,flag,col,count;
        for(i=1;i<=NumOfVN;i++){
            for(j=1;j<=VN_struct_arr[i].NumOfPro;j++){
               col=lookForNot(VN_struct_arr,VN_name,NumOfVN,table[VN_struct_arr[i].position[j]]);
               count = count_right(table[VN_struct_arr[i].position[j]]);
               if(col==0)  {
                   for(z=1;z<=count;z++){
                       flag = Kind(VN_name,NumOfVN,table[VN_struct_arr[i].position[j]][z]);
                       if(flag==2){
                        add(&VN_struct_arr[i].Prof[j],&VN_struct_arr[f(table[VN_struct_arr[i].position[j]][z])].first);
                        sub_element(&VN_struct_arr[i].Prof[j],'`');
                       }
                   }
                   add_element(&VN_struct_arr[i].Prof[j],'`');
               }
               else{
                   for(z=1;z<=col;z++){
                       flag = Kind(VN_name,NumOfVN,table[VN_struct_arr[i].position[j]][z]);
                       if(flag==0)
                          add_element(&VN_struct_arr[i].Prof[j],table[VN_struct_arr[i].position[j]][z]);
                       if(flag==2){
                        add(&VN_struct_arr[i].Prof[j],&VN_struct_arr[f(table[VN_struct_arr[i].position[j]][z])].first);
                        sub_element(&VN_struct_arr[i].Prof[j],'`');
                       }
                   }
               }   
            }
        }
}

void FollowOneLine(char arr[],char x,vn VN_struct_arr[]){
    int positions[NUM],count,i,p,flag=0,c;
    Char_to_positions(arr,x,positions);
    count = count_right(arr);
    for(p=1;p<=positions[0];p++)
    {
        flag=0;
       for(i=positions[p];i<=count;i++)
       {
		 
           if(arr[i+1] != '\n')
           {
               c=Kind(VN_name,NumOfVN,arr[i+1]);
               if(c==0)
               {
                    add_element(&VN_struct_arr[f(x)].Follow,arr[i+1]);
                    flag++;
                    break;
               }
               else if(c==2 && VN_struct_arr[f(arr[i+1])].state==0)
               {
                    flag++;
                    add(&VN_struct_arr[f(x)].Follow,&VN_struct_arr[f(arr[i+1])].first);
                    break;
               }
			   else if(c==1)
			   {
                    ;
			   }
               else
               {
				
                    add(&VN_struct_arr[f(x)].Follow,&VN_struct_arr[f(arr[i+1])].first);
					sub_element(&VN_struct_arr[f(x)].Follow,'`');
               }
           }
       }
       if(flag==0)
       {
           add(&VN_struct_arr[f(x)].Follow,&VN_struct_arr[f(arr[0])].Follow);
       }
    }
}

void Follow(vn VN_struct_arr[],char table[][NUM],int NumOfVN,char VN_name[])
{
    int i,j,l,a;
    char ch;
    add_element(&VN_struct_arr[f(VN_name[1])].Follow,'#');
    VN_struct_arr[f(VN_name[1])].done=1;
    while(Changed_num(VN_struct_arr,NumOfVN)>0)
    {
    Reset(VN_struct_arr,NumOfVN);
    for(a=1;a<=NumOfVN;a++)
    {
        ch = VN_name[a];
        l=VN_struct_arr[f(ch)].Follow.length;
        for(i=1;i<=NumOfVN;i++)
        {
            for(j=1;j<=VN_struct_arr[i].NumOfPro;j++)
                FollowOneLine(table[VN_struct_arr[i].position[j]],ch,VN_struct_arr); 
        }
        if(l < VN_struct_arr[f(ch)].Follow.length)
          VN_struct_arr[f(ch)].done=1;
    }

    }
}




int main()
{
	int i=1;
	char pose[2];
 
	pose[0]='-';
	pose[1]='>';
    Inits(table);


	Inputs(table,pose,2);

    //printChan(table);
    //printf("\n\n");
    
    NumOfVN =First(table,VN_struct_arr,tmp);
    printAllfirstList(VN_struct_arr,NumOfVN);
    printf("\n\n");

    FirstOfPro(VN_struct_arr,table,NumOfVN);
    printAllProList(VN_struct_arr,NumOfVN,table);
    printf("\n\n");

    Follow(VN_struct_arr,table,NumOfVN,VN_name);
	printAllfollowList(VN_struct_arr,NumOfVN);
    printf("\n\n");
	

    return 0;
}

/*
S-->AB
S-->bC
A-->`
A-->b
B-->`
B-->aD
C-->AD
C-->b
D-->aS
D-->c
*/

```
