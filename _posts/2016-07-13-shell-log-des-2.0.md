---
title: shell实战之日志脱敏-2.0
author: k
layout: post
permalink: /shell-log-des-2.0/
post_views_count:
  - 0
categories:
  - shell
  - 引言
tags:
  - shell
---

cfg

	# This is generated to be a configuration file.
	# kay
	# 06/01/2016

	# This is a parameter for crontab and objective file.
	interval=10
	begin=1
	end=13

	# There are some log path 
	# logpath=/root/shell/log/26;/root/shell/log/27;/root/shell/log/28
	logpath=/home/pospadm/trc

	# sh path
	shpath=/home/pospadm/log-shell/log-secret/start.sh

	# keylab path
	keylabpath=/home/pospadm/log-shell/log-secret/key

collect.sh


	#!/bin/bash -

	# Used to collect value list
	# kay
	# Version: 1.0
	# 06/01/2016

	# key list
	keylist=`cat ${keylabpath}`

	# golobal value list
	valuelist=""
	index=0

	# collect value by key in directory 
	# @Parameter: directory file
	collect_list()
	{
    for key in ${keylist}
    do
        if grep -qr ${key} $1 
        then
            f0="/<"$key">/p"
            f1="<"$key">"
            f2="s/<"$key">/\n"$key"=/gp"
            f3="s/<\/"$key">/\n/gp"
            f4="/"$key"=/p"
            f5="s/"$key"=//gp"
            #f6="\$!N;/^\(.*\)\n\key$/!P;D"
            #f6=":key;N;s/^(\S+)((\n.*)*)\n\key$/\key\2/M;\$!bkey"

            #list=`sed -n ${f0} $1 | sed -n ${f2} | sed -n ${f3} | sed -n ${f4} | sed -n ${f5}`
            #list=`grep ${f1} $1 | sed -n ${f2} | sed -n ${f3} | sed -n ${f4} | sed -n ${f5}`
            list=`grep -r ${f1} $1 | sed -n ${f2} | sed -n ${f3} | sed -n ${f4} | sed -n ${f5}`
            #list=`grep -r ${f1} $2 | sed -n ${f2} | sed -n ${f3} | sed -n ${f4} | sed -n ${f5} | sort -k2n | sed ${f6}`
            #list=`grep -r ${f1} $2 | sed -n ${f2} | sed -n ${f3} | sed -n ${f4} | sed -n ${f5} | sed -r ${f6}`
        
            for l in ${list}
            do
                valuelist[index]=${l}
                ((index++))
            done
        fi    
    done
	}

crontab.sh


	#!/bin/bash -

	# Used to do a crontab task
	# kay
	# Version: 1.0
	# 06/01/2016

	# @Name: docrontab
	# @Parameter: specific crontab task
	docrontab()
	{
    cronfile="/tmp/crontab.${USER}"
    crontab -l > ${cronfile}
    query=`echo "$1" | sed 's/*/\\\*/g'`
    grep -q "${query}" ${cronfile} && echo "" ||
    {
            echo "$1" >> ${cronfile}
            crontab ${cronfile}
    }
    rm -f ${cronfile}
	}	

key


	track_2
	track_3 
	pan 
	exp_dt 
	ic_dat 
	pin_key 
	track_key 
	trm_key 
	tpklmk 
	zaklmk 
	zpklmk 
	pin_data

readme


	本脚本使用方法说明：
	1.配置环境变量
	# vi ~/.bash_profile
	export desen_path=/root/shell2
	#source ~/.bash_profile
	2.配置文件说明
	文件cfg
	interval为定时任务间隔时间
	begin为查找的文件范围起始时间点
	end为查找的文件范围的戒指时间点
	logpath为要处理的日志路径，多个路径用;隔开
	shpath为本脚本所在绝对路径
	keylabpath为关键字字典文件所在绝对路径
	3.关键字字典文件
	里面的每一个关键字应该是日志内<></>内的内容
	要替换的内容为标签内的内容
	4.启动脚本
	./start.sh
	调试方法：
	#nohup bash -x start.sh 1>tt.log &
	5.停止脚本
	#./stop.sh

start.sh


	#!/bin/bash -

	# This file is used to handle log desensitization
	# kay
	# Version: 1.0
	# 05/31/2016

	. ${HOME}/.bash_profile
	. ${desen_path}/util.sh
	. ${desen_path}/crontab.sh
	. ${desen_path}/collect.sh

	# write log
	logfile="log.txt"
	echo "["`date`"] start script." >> ${logfile}

	# add crontab
	task="*/"${interval}" * * * *   "${shpath}
	docrontab "${task}"

	dir1=""
	dir2=""
	# read config file and handle every file and directiory
	for d in ${logpath}
	do
    tmp="/tmp/find.ret"
    # search for all changed file between time begin and end
    ret=`find "${d}" -mmin +"${begin}" -mmin -"${end}" -type f`
     if [ "${ret}" != "" ]
    then
        echo ${ret} | xargs ls -rt > ${tmp} 
        file1=`sed -n '1p' ${tmp}` 
        file2=`sed -n '$p' ${tmp}` 
        rm -f ${tmp}
        dir1=`echo ${file1} | sed -r 's/^(\/.*\/)[^/]+\/?/\1/g'`
        dir2=`echo ${file2} | sed -r 's/^(\/.*\/)[^/]+\/?/\1/g'`
        collect_list ${dir1}
        if [ "${dir1}" != "${dir2}" ] 
        then
            collect_list ${dir2}
        fi 
    fi    
    
	done

	# filter the global list. delete the same value.
	# "echo ${globallist[@]} | sort -k2n | sed ":1;N;s/^(\S+)((\n.*)*)\n\1$/\1\2/M;\$!b1"
	valuelist=($(awk -vRS=' ' '!a[$1]++' <<< ${valuelist[@]}))

	# hanlde a directory
	handle_directory()
	{
    for l in ${valuelist[@]}
    do
        cnb=""
        len=${#l}
        if [ ${len} -gt 8 ]
        then
            cnb="5-"$((${len} - 4))
        else
            cnb="1-"${len}
        fi
        mid=`echo ${l} | cut -nb ${cnb} | sed 's/*/\\\*/g'`
        h=`echo ${l} | sed 's/'${mid}'/*******/g'`
        f=`echo "${l}" | sed 's/*/\\\*/g'`

        #sed -i 's/'${f}'/'${h}'/g' ${c}
        grep -rl "${f}" $1 | xargs sed -i 's/'${f}'/'${h}'/g'
    done
	}

	if [ "${dir1}" != "" ] 
	then
    handle_directory ${dir1}
    if [ "${dir1}" != "${dir2}" ] 
    then
        handle_directory ${dir2}
    fi
	fi

	echo "["`date`"] end script." >> ${logfile}

	exit

stop.sh


	#!/bin/bash -

	# Used to stop script and crontab
	# kay
	# Version: 1.0
	# 06/03/2016

	crontab -r
	ps -ef | grep 'start' | awk -F ' ' '{ print $2 }' | xargs kill -9

util.sh


	#!/bin/bash -

	# Read config
	# kay
	# Version: 1.0
	# 06/01/2016

	# configuration file path
	config=${desen_path}/"cfg"

	# config parameters
	prefix=`grep -v "^#" ${config}`
	interval=`echo "${prefix}" | grep  "interval" | awk -F "=" '{ print $2 } '`
	begin=`echo "${prefix}" | grep  'begin' | awk -F '=' '{ print $2 } '`
	end=`echo "${prefix}" | grep  'end' | awk -F '=' '{ print $2 } '`
	logpath=`echo "${prefix}" | grep  'logpath' | awk -F '=' '{ print $2 }' | awk -F ';' '{ for(i=1;i<=NF;i++) print $i }'`
	shpath=`echo "${prefix}" | grep  'shpath' | awk -F '=' '{ print $2 }'`
	keylabpath=`echo "${prefix}" | grep  'keylabpath' | awk -F '=' '{ print $2 }'`