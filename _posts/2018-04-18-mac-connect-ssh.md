---
layout: post
title:  "맥에서 ssh로 리눅스 서버에 접속하기"
date:   2018-04-18
categories: [os]
tags: [linux,ssh,putty]
---

* 맥에서 터미널로 ssh 접속하기

	> `ssh 계정ID@호스트혹은IPaddress -p 포트번호`

	~~~bash
		isoo@isoo-MacBook-Air:~ » ssh isoo@ ▩▩▩▩▩▩▩ -p ▩▩▩▩▩▩▩
		The authenticity of host ▩▩▩▩▩▩▩▩ cannot be established.
		ECDSA key fingerprint is ▩▩▩▩▩▩▩▩.
		Are you sure you want to continue connecting (yes/no)? yes
		Warning: Permanently added ▩▩▩▩▩▩▩▩ (ECDSA) to the list of known hosts.
		isoo@ ▩▩▩▩▩▩▩s password:
		Welcome to Ubuntu 12.04.1 LTS (GNU/Linux 3.2.0-29-generic x86_64)

		 * Documentation:  https://help.ubuntu.com/

		 System information disabled due to load higher than 8.0

		New release '14.04.1 LTS' available.
		Run 'do-release-upgrade' to upgrade to it.

		*** System restart required ***
		Last login: ▩▩▩▩▩▩▩▩▩▩
		isoo@ ▩▩▩▩▩▩▩:~$ ls
	~~~


* 리눅스 접속 끊고 터미널로 돌아가기

	* ctrl + d
