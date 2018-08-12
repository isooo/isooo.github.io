---
layout: post
title:  "리눅스에서 파일 내 문자열 검색"
date:   2018-01-14 16:35:00 +0900
categories: [Linux]
tags: [linux,grep,find]
---

* 리눅스 파일명 검색

	* `find 찾을위치 -name "파일명"`

		ex) 확장자가 txt 인 파일을, 현재 폴더부터 하위폴더를 포함하여 찾아줘
		`find ./ -name "*txt"`
	~~~bash
		$ find ./ -name "*txt"
		./test111.txt
		./test1120.txt
	~~~
	
	* 만약 전체 폴더에서 찾고 싶다면 `find / -name "파일명"`   
<br/><br/>
* 리눅스 **파일** 내부 문자열 검색  
	* `grep -r "찾을문자열" 찾을위치`
		
		ex) 현재 폴더부터 하위 모든 폴더 내 파일에서 test 라는 문자열을 찾아줘
			`grep -r "찾을문자열" ./*`
	~~~bash
		$ grep -r "test" ./*
		./test111.txt:for test
		./test171120.txt:for test 20171120 Mon
	~~~

	* `-n` 옵션을 주면, 해당 문자열이 몇 번째 라인에 있는지도 출력할 수 있다

		ex) 현재 폴더에 있는 test111.txt 에서 test라는 문자열이 몇 번째 라인에 있는지 찾아줘
	~~~bash
		$ grep -r -n "test" ./test111.txt
		1:for test
	~~~	