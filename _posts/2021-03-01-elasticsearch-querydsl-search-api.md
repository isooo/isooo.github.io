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
Elasticsearch에서 제공하는 검색 관련 기능은 Query DSL을 이용해 모두 활용할 수 있다.   

### **Match Query**(Full Text Query)


### **Term Query**
문자열 형태의 값을 검색하기 위해 Elasticsearch는 2가지 매핑 유형을 지원한다.
- **text** 데이터 타입
    - 필드에 데이터가 저장되기 전, 데이터가 분석되어 역색인 구조로 저장됨
- **keyword** 데이터 타입
    - 데이터가 분석되지 않고 그대로 필드에 저장됨

Match Query(Full Text Query)는 쿼리를 수행하기 전, 먼저 분석기를 통해 키워드를 분석한 후 검색을 수행한다. 하지만 Term Query는 별도의 분석 작업을 수행하지 않고, 입력된 텍스트를 가진 문서를 찾는다.
따라서 keyword 데이터 타입을 사용하는 필드를 검색하려면 Term Query를 사용해야 한다



### **Bool Query**

### **Exists Query**
> 필드의 값이 null이거나, 필드 자체가 없는 문서 검색하고자 할 때  

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

### **Count API**
> 검색된 문서의 개수만 가져오고자 할 때

`movieNm` 필드가 `나라`라는 값을 포함하고 있는 document의 개수를 알고자 한다면
```json
POST movie_search/_count

{
  "query" : {
    "match" : {
      "movieNm" : "나라"
    }
  }
}
```

### 정렬
> 전체 문서를 대상으로, 특정 기준으로 정렬하고자 할 때

```json
{
  "sort": {
    "name": {
      "order": "desc"
    }
  }
}
```

Object 구조에서도 사용 가능  
```json
{
  "sort": {
    "goods.id": {
      "order": "desc"
    }
  }
}
```
