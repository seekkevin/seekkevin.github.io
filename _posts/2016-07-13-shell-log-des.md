---
title: shell实战之日志脱敏
author: k
layout: post
permalink: /shell-log-shell-des/
post_views_count:
  - 0
categories:
  - shell
tags:
  - shell
---

本次实战目标为日志脱敏，将日志目录内的所有文件进行处理，凡是涉及到卡号和密码的信息，一律以“*”号替代，要替代的内容都从对应的标签内获取，本脚本执行目录

	drwxr-xr-x 5 root root 4096 May 31 20:20 log
	-rw-r--r-- 1 root root   57 May 31 20:20 cfg.log
	-rw-r--r-- 1 root root    3 May 31 20:28 cfg.time
	-rwxr-xr-x 1 root root 1625 May 31 20:30 desi.sh
	-rw-r--r-- 1 root root  856 May 31 20:50 sh.log

使用说明

本脚本使用说明：
         1.文件夹log为待处理的日志目录，作为例子
         2.cfg.log用来配置待处理的日志目录
         3.cfg.time为定时任务的间隔时间，单位为分钟
         4.sh.log为脚本执行留下的日志
         5.desi.sh为处理日志脱敏的脚本
         6.在desi.sh前几行是各配置文件路径，需要手动配置，注意必须是绝对路径
 
    PS：
       经测试，处理80M的日志大概需要30s+时间，所以最好把时间间隔设置的尽量长一些，另外，本脚本并未对处理过的日志做任何记忆，也就是说，每次执行脚本的时候，都会对日志目录内的日志进行处理，所以随着时间的推移，日志越来越大，脚本执行的时间也会越长，这一点需要注意。如果必要，后期可优化。

desi.sh内容


	# This file is used to handle log desensitization
	# kay
	# 05/31/2016

	#!/bin/bash -

	# global path variable
	timecfg="/root/shell/cfg.time"
	path="/root/shell/desi.sh"
	shlogpath="/root/shell/sh.log"
	logcfg="/root/shell/cfg.log"

	# add crontab
	docrontab()
	{
		cronfile="/tmp/crontab.${USER}"
		crontab -l > ${cronfile}
        interval=`cat ${timecfg}`
		content="*/"${interval}" * * * *   "${path}
		grep -q "${path}" ${cronfile} && echo "" || 
		{
			echo "${content}" >> ${cronfile}
			crontab ${cronfile}
		}
		rm -rf ${cronfile}
	}

	# desensitization function
	keylab=(track_2 track_3 pan exp_dt ic_dat pin_key track_key trm_key tpklmk zaklmk zpklmk pin_data)
	desensitization(){
	for key in ${keylab[*]}
	do
		f1="<"${key}">"
		f2="s/<"${key}">/\n"${key}"=/gp"
		f3="s/<\/"${key}">/\n/gp"
		f4="/"${key}"=/p"
		f5="s/"${key}"=//gp"
		f6=":1;N;s/^(\S+)((\n.*)*)\n\1$/\1\2/M;$!b1"

		list=`grep -r ${f1} $1 | sed -n ${f2} | sed -n ${f3} | sed -n ${f4} | sed -n ${f5} | sed -r ${f6} | cat`
	
		for l in ${list}
		do 
			cnb=""
			len=${#l}
			if [ ${len} -gt 8 ]
			then
				cnb="5-"$((${len} - 4))
			else
				cnb="2-3"
			fi
			mid=`echo ${l} | cut -nb ${cnb}`
			mid=`echo "${mid}" | sed 's/*/\\\*/g'`
			h=`echo ${l} | sed 's/'${mid}'/*******/g'`
			f=`echo "${l}" | sed 's/*/\\\*/g'`
			grep -rl ${f} $1 | xargs sed -i 's/'${f}'/'${h}'/g'
		done
	done
	}

	# check whether config file exists 
	if test -f ${logcfg}
	then
    	echo "${logcfg}..." >> ${shlogpath}
	else
    	echo "log config file not exist" >> ${shlogpath}
	fi

	# read config file and handle every file and directiory
	filelist=`cat ${logcfg}`
	for f in ${filelist}
	do
		desensitization ${f}
	done

	docrontab

	exit


讲解先不写了，手都麻了

 

希望下次看到还能看明白。。