---
layout: post
title:  "깃허브블로그에 댓글달기 기능 추가하기"
date:   2018-09-04
categories: [Etc]
tags: [disqus, Jekyll]
---

# disqus 기능 붙이기
블로그 템플릿에 댓글달기 기능이 붙어있었는데  
연동을 시켜놓지 않아 ~~마치 모델하우스처럼~~ 텅 빈 기능으로 자리잡고 있었다  
미루고 미루다 이제서야 기능을 붙여보았다  
  
  
  

##  TO-DO
- disqus를 이용하여 깃허브블로그에 소셜 댓글 서비스 기능 붙이기  
  
  
  
  
## 진행 순서
1. [disqus 가입하기](#ch1)
2. [블로그에 적용하기](#ch2)  
  
  
  
  
### 1. disqus 가입하기 <a name="ch1"></a>
- 가입 후 로그인 > 우측 상단에 프로필 이미지 클릭 > `Settings` > 우측 상단에 톱니바퀴 클릭 > `Add Disqus To Site`  

- 화면 아래 쪽에 `GET STARTED` > `I want to install Disqus on my site` 클릭  

- `Create a new site` 페이지가 나오면, `Website Name`에 원하는 정보로 입력하기
 
- `Select a plan` 에서는 `Basic`을 선택해주면 ~~과금되지 않는 무료서비스로 선택이~~ 된다 > Subscribe now

- 여러 아이콘들이 나오는데, 어디서 코멘트 서비스를 이용할 건지 선택할 수 있는 부분!   
    - 나는 `Jekyll` 을 이용해서 블로그를 만들었기에 `Jekyll` 선택!

- disqus 메인페이지에서 우측 상단에 `Admin` 선택 > 좌측 상단에 `Your Sites` 라는 부분에서 내가 방금 생성한 사이트 클릭 > 좌측 메뉴 중 `General`을 클릭하여 `Website URL` 에 내가 적용하고자 하는 url을 입력
    ```
      Website URL   https://isooo.github.io
    ```   


  
  
### 2. 블로그에 적용하기 <a name="ch2"></a>
- `_layouts/blog.html` 에 다음과 같이 `comments: true` 를 입력
    ```html
      ---
      layout: default
      comments: true
      ---
    ```

- `_includes/comments.html`에서 `s.src` 부분에, disqus url 을 넣어준다
    ``` js
      s.src = 'https://isooo.disqus.com/embed.js';
    ```

- `_config.yml` 에서 disqus Website Name을 적어준다
    ```yml
      disqus: "isooo"
    ```

- 이제 commit 하고 push!!  




## 참고
- [Github Blog에 댓글(disqus) 기능 추가하기](https://devminjun.github.io/blog/addComments)
- [지킬 블로그에 Disqus 추가하기](https://hanjungv.github.io/2017-02-03-2_ETC_AddDisqus/)  

