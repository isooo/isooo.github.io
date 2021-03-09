---
layout: post
title:  "[Elasticsearch] "aggregation"
date:   2021-03-08
categories: [ETC]
tags: [Elasticsearch]
---

> ### [엘라스틱서치 실무 가이드 (2019)](http://www.yes24.com/Product/Goods/71893929)을 읽고 정리한 내용입니다.     

<br/>

SQL에서 `GROUP BY` 연산과 비슷한 기능을 Elasticsearch에서도 제공한다.  
바로 집계(aggregation)기능이다. 둘을 비교해보자면 아래와 같다.  
- **SQL의 `GROUP BY` 구문을 이용한 집계**  
    ```sql
    SELECT SUM(ratings) FROM movie_review GROUP BY movie_no;
    ```

- **Elasticsearch의 Query DSL로 집계하는 쿼리**  
    ```json
    {
        "aggs" : {
            "movie_no_agg" : {
                "terms" : {
                    "field" : "movie_no"
                },
                "aggs" : {
                    "ratings_agg" : {
                        "sum" : {
                            "field" : "ratings"
                        }
                    }
                }
            }
        }
    }
    ```