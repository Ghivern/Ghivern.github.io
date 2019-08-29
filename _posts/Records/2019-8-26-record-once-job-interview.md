---
title: 记录自己第一次面试笔试题
layout: post
tags: [Records]
---

此次笔试共三道题，虽然不是很难，但是做的情况不太好，一方面由于机试电脑不习惯，二是紧张，没掌控好时间。在此记录下来。

1、输入：234
				10
	 输出：0000000234
	 

```
#include <stdio.h>
#include <string.h>
int main(){
	char str[100];
	char ch;
	int num;
	int i;
	memset(str,'\0',100);
	scanf("%s \n %c \n %d",str,&ch,&num);
	for(i=0;i<num-strlen(str);i++)
		printf("%c",ch);
	printf("%s",str);
	return 0;
}
```

2、输入两个int(32位)数，输入两个数位不同的个数

```
#include <stdio.h>

int main(){
	int m,n;
	int res;
	int count = 0;
	int i;
	scanf("%d %d",&m,&n);
	res = m ^ n;
	for(i=0; i<32; i++){
		if(res % 2 == 1){
			res = res>>1;
		}else{
			res = res>>1;
			count++;
		}
	}
	printf("%d",count);
	return 0;
}
```

3、将一个字符串中出现次数最少的字符去掉，输出字符串

```
#include <stdio.h>
#include <string.h>

int main(){
	char str[100];
	char newStr[100];
	int chs[26];
	int min;
	int last = 0;
	int i;
	int len;
	memset(str,'\0',100);
	memset(newStr,'\0',100);
	scanf("%s",str);
	for(i=0;i<26;i++)
		chs[i] = 0;

	for(i=0;i<strlen(str);i++)
		chs[str[i] - 'a']++;

	for(i=0;i<26;i++)
		if(chs[i] != 0){
			min = chs[i];
			break;
		}

	for(i=0;i<26;i++)
		if(chs[i]<min && chs[i]!=0)
			min = chs[i];

	len =strlen(str);
	for(i=0;i<len;i++)
		if(chs[str[i]-'a'] == min)
			str[i] = '\0';

	for(i=0;i<len+1;i++)
		if(str[i] == '\0'){
			strcat(newStr,str+last);
			last = i + 1;
		}
	printf("%s",newStr);
	return 0;
}
```

