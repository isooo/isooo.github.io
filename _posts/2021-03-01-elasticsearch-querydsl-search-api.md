---
layout: post
title:  "[Elasticsearch] 자주 사용하는 `_search` API"
date:   2021-03-01
categories: [ETC]
tags: [Elasticsearch]
---

> ### [엘라스틱서치 실무 가이드 (2019)](http://www.yes24.com/Product/Goods/71893929)을 읽고 정리한 내용입니다.     

<br/>

Elasticsearch에서 제공하는 검색 API는 기본적으로 질의(Query)를 기반으로 동작한다.  
검색 질의에는 검색하고자 하는 각종 조건들을 명시할 수 있으며, 동일한 조건을 아래 2가지 방식으로 표현할 수 있다.
- **URI 방식**
    - 루씬에서 사용하던 전통적인 방식의 URI 표기법
    - e.g. `GET movie_search/_search?q=prdtYear:2018`
- **Request Body 검색**
    - RESTful API를 이용해 body에 조건을 표기

두 가지 방법 중 `Request Body` 방식을 이용한 검색을 알아보자.  

<br/>

## Request Body 검색
Request Body 방식은 HTTP 요청 시 body에다 검색할 컬럼과 검색어를 **json 형태**로 전달하는 방식이다. 이 방식은 복잡한 옵션을 가진 검색 쿼리를 URI 방식에 비해 깔끔한 json 형태로 넘길 수 있다는 장점이 있다.  
덧붙여 **[Query DSL](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl.html)**을 이용한다면 여러 질의를 조합하거나, 질의 결과에 대해 재검색을 수행하는 등 다양하게 조합하여 표현할 수 있다.   
     
Query DSL을 이용하여 Elasticsearch에 질의하는 방법을 알아보자.    

<br/>

## Query DSL 주요 쿼리
es에서 제공하는 검색 관련 기능은 Query DSL을 이용해 모두 활용할 수 있다.   

### 필드의 값이 null이거나, 필드 자체가 없는 문서 검색
만약 `create_date`라는 필드의 값이 null이거나 `create_date`이라는 필드 자체가 없는 문서를 검색하고자 한다면 
```json
POST 인덱스명/_search

{
  "query": {
    "bool": {
      "must_not": {
        "exists": {
          "field": "create_date"
        }
      }
    }
  }
}
```