---
layout: post
title:  "[Java] BigDecimal"
date:   2019-01-17
categories: [ETC]
tags: [java, BigDecimal]
---


~~~java
System.out.println(2.0 - 1.1);
~~~
위 코드를 실행시킨 결과는 `0.8999999999999999`이다.  
<small>*~~아직 쪼무래기인 나는 당연히 `0.9`가 나올것이라 예상했...~~*</small>

위와 같은 결과로 나오는 이유는  
정확히 `0.9`라는 값을 이진법으로 나타낼 수 없고 근사치만 만들 수 있기 때문이다.  
그렇다면 예상과 동일한 결과를 도출하기 위해서는 어떻게 해야할까?  


<br/>
# BigDecimal  

자바에서는 `BigDecimal`이라는 클래스를 제공하고 있다.  

~~~java
BigDecimal b1 = new BigDecimal("2.0");
BigDecimal b2 = new BigDecimal("1.1");

System.out.println(b1.subtract(b2));
// 0.9
~~~  

위와 같은 방법으로  
부동소수점에 대한 정확한 계산을 할 수 있다!!  
위의 예시 외에도 다양한 함수가 존재한다.

~~~java
System.out.println(b1.min(b2));     // 1.1
System.out.println(b1.max(b2));     // 2.0
System.out.println(b1.divide(b2, RoundingMode.FLOOR));      // 1.8
System.out.println(b1.divide(b2, 4, RoundingMode.FLOOR));   // 1.8181
System.out.println(b1.pow(3));      // 8.000

~~~  
