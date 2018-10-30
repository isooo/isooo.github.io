---
layout: post
title:  "윈도우 로컬에서 지킬 서버 띄우기"
date:   2018-10-30
categories: [Etc]
tags: [Jekyll, blog]
---

윈도우 환경에서 지킬 서버를 띄워, 블로그를 편집하고 싶었다.  
포스팅 하나를 하려할 때마다 commit을 하는 것은 ~~지저분하기 그지없고~~ 비효율적이라  
로컬에서 수정하고 완성본이 되었을 때 commit-push 하기위하여 본 작업을 하게 되었다.

#  　　

1. [https://rubyinstaller.org/downloads/](https://rubyinstaller.org/downloads/){: target="_blank" } 에서  
  `WITHOUT DEVKIT [Ruby 2.3.3 (x64)]` 과 `DEVELOPMENT KIT (OLD) For use with Ruby 2.0 to 2.3 (x64 - 64bits only)` 을 다운로드

2. 다운로드받은 `Ruby 2.3.3 (x64)` 를 더블클릭하여 설치   
  - 설치시, 체크박스 중 `Add Ruby executables to your PATH`를 체크하면, 환경변수까지 자동으로 세팅해준다

3. 다운로드받은 `DEVKIT(DevKit-mingw64-64-4.7.2-20130224-1432-sfx.exe)` 이 설치된 위치에서 cmd를 열어, 설치한다!  
```bash
  isooo@DESKTOP ~/Desktop/gitre/ruby
  $ ruby dk.rb init
  [INFO] found RubyInstaller v2.3.3 at C:/Ruby23-x64

  Initialization complete! Please review and modify the auto-generated
  'config.yml' file to ensure it contains the root directories to all
  of the installed Rubies you want enhanced by the DevKit.

  isooo@DESKTOP ~/Desktop/gitre/ruby
  $ ruby dk.rb install
  [INFO] Updating convenience notice gem override for 'C:/Ruby23-x64'
  [INFO] Installing 'C:/Ruby23-x64/lib/ruby/site_ruby/devkit.rb'
  
  isooo@DESKTOP ~/Desktop/gitre/ruby
  $ ruby --version
  ruby 2.3.3p222 (2016-11-21 revision 56859) [x64-mingw32]

  isooo@DESKTOP ~/Desktop/gitre/ruby
  $ gem --version
  2.5.2
```
  - 여기까지 하면, ruby와 gem 설치확인까지 끝난 단계
      - gem은 ruby에서 쓰는 패키지 매니저이다  

4. 이제 지킬을 설치한다
```bash
  gem install jekyll
```  


5. 깃 블로그를 clone한 위치로 이동해서, 지킬 서버를 실행해본다
```bash
  isooo@DESKTOP ~/Desktop/gitre/blog/isooo.github.io (master)
  $ jekyll serve
  ......
      Server address: http://127.0.0.1:4000
    Server running... press ctrl-c to stop.
```

6. [http://127.0.0.1:4000](http://127.0.0.1:4000) 로 접속하면, 로컬에서 띄운 블로그를 확인할 수 있다
