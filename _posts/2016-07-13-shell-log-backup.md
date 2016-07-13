---
title: shell实战之日志备份
author: k
layout: post
permalink: /shell-log-backup/
post_views_count:
  - 0
categories:
  - C/c++
  - 引言
tags:
  - shell
---

util.sh


	#!/bin/bash -

	# Read config
	# kay
	# Version: 1.0
	# 06/22/2016

	# configuration file path
	config=${log_bak}/"cfg"

	# config parameters
	prefix=`grep -v "^#" ${config}`
	begin=`echo "${prefix}" | grep  'begin' | awk -F '=' '{ print $2 } '`
	logpath=`echo "${prefix}" | grep  'logpath' | awk -F '=' '{ print $2 }' | awk -F ';' '{ for(i=1;i<=NF;i++) print $i }'`
	shpath=`echo "${prefix}" | grep  'shpath' | awk -F '=' '{ print $2 }'`
	bakpath=`echo "${prefix}" | grep  'bakpath' | awk -F '=' '{ print $2 }'`

cfg


	# This is generated to be a configuration file.
	# kay
	# 06/22/2016

	# This is a parameter for crontab and objective file.
	begin=15

	# This are some log path 
	# logpath=/root/shell/log/26;/root/shell/log/27;/root/shell/log/28
	logpath=/home/pospadm/trc

	# bak path
	bakpath=/home/kycap/bak

	# sh path
	shpath=/home/pospadm/log-bak/start.sh

crontab.sh


	#!/bin/bash -

	# Used to do a crontab task
	# kay
	# Version: 1.0
	# 06/22/2016

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

readme

	1.设置环境变量
	log_bak=脚本所在目录
	2.启动脚本
	./start.sh
	3.停止脚本
	./stop.sh

start.sh


	#!/bin/bash -

	# This file is used to handle log bak 
	# kay
	# Version: 1.0
	# 06/22/2016

	. ${HOME}/.bash_profile
	. ${log_bak}/util.sh
	. ${log_bak}/crontab.sh

	# add crontab
	task="0 0 * * * "${shpath}
	docrontab "${task}"

	#echo "${bakpath}" >> ret.log
	cd ${bakpath}
	current=`date +"%Y%m%d"`
	mkdir ${current} 2>/dev/null 
	cd ${current}

	# handle every specific file 
	for d in ${logpath}
	do
    	# search for all directories before time begin
    	filelist=`find "${d}" -mtime +"${begin}" -type f`
    	for f in ${filelist[@]}
    	do
       		dir=`echo ${f} | awk -F '/' '{ print $(NF - 1) }'`    
        	mkdir ${dir} 2>/dev/null
        	mv ${f} ${dir}
    	done
	done

	# tar log
	cd ${bakpath}
	tar -zcf "${current}.tar.gz" ${current}
	rm -rf ${current}

stop.sh


	#!/bin/bash -

	# Used to stop script and crontab
	# kay
	# Version: 1.0
	# 06/22/2016

	crontab -r
	ps -ef | grep 'start' | awk -F ' ' '{ print $2 }' | xargs kill -9