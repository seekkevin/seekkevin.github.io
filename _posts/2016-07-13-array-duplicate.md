---
title: 求一个数组中重复数字的个数，要求复杂度为O(n)
author: k
layout: post
permalink: /array-duplicate/
post_views_count:
  - 0
categories:
  - c/c++
  - 问题描述
---


给出代码

	#include <stdio.h>
	#include <unistd.h>
	#include <iostream>
	#include <memory.h>
	#include <string.h>
 
	int main()
	{
        typedef int data_type;
        data_type a[] = {1,4,6,7,7,8,4,9,1,7,6,4,1,2,0,0};
 
        const char* hint = "The original number is:\n ";
        write(STDOUT_FILENO, hint, strlen(hint));
        for(int i = 0; i < sizeof(a)/sizeof(a[0]); ++i)
        {
                printf("%d ", a[i]);
        }
        printf("\n");
 
        // get the max value
        int max = a[0];
        for(int i = 0;i < sizeof(a)/sizeof(data_type);++i)
        {
                if(a[i] > max)
                {
                        max = a[i];
                }
        }
 
        // allocate an space. you can use bitwise if the space is too big.
        data_type* p = new data_type[max + 1];
        memset(p, 0, max + 1);
 
        int cnt = 0;
        for(int i = 0;i < sizeof(a)/sizeof(data_type);++i)
        {
                if(*(p + a[i]) == -1)
                {
                        ++cnt;
                }
                else
                {
                        *(p + a[i]) = -1;
                }
        }
 
        std::cout << "The dumplicated number is " << cnt << std::endl;
 
        return 0;
	}
　　

编译执行


	[root@ba algr]# g++ array.cpp -g
	[root@ba algr]# ./a.out
	The original number is:
	 1 4 6 7 7 8 4 9 1 7 6 4 1 2 0 0
	The dumplicated number is 8
	[root@ba algr]#