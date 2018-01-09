---
layout: post
title:  "window에서 동작중인 프로세스 확인"
date:   2017-12-14 16:35:00 +0900
categories: [Window]
tags: [window,process,cmd]
---


cmd를 통해 윈도우에서 현재 동작중인 모든 프로세스를 확인해보자

1. cmd에서 `tasklist` 입력

	~~~bash
		C:\Users\helloworld>tasklist

		이미지 이름        ID    세션 이름    세션#    메모리 사용
		========================= ======== ================ =========== ============
		System Idle Process   0     Services     0      8 K
	~~~


2. 좀 더 자세히 알고 싶다면 `tasklit /V` 
	나의 경우엔 실행중인 mssql을 알고 싶었기 때문에 findstr로 sql 문자열을 잡아주는 명령을 추가했다



	~~~bash
		C:\Users\helloworld>tasklist /V | findstr sql
	~~~
