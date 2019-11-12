---
layout: post
title:  "[Java] Collectors.joining()"
date:   2019-11-12
categories: [ETC]
tags: [Java, JDK8, Collectors, joining]
---

# Collectors.joining
JDK 8에 추가된 기능.   
특정 구분 기호를 삽입하여 순서대로 조합하고자 할 때, 이 메소드를 이용해서 간편하게 처리할 수 있다.

```java
final String result = Arrays.asList("apple", "banana", "cactus")
        .stream()
        .collect(Collectors.joining(", "));
System.out.println(result);
```

```
// 실행 결과

apple, banana, cactus
```

<br/>

```java
final String result = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
        .stream()
        .map(String::valueOf)
        .collect(Collectors.joining(" : "));
System.out.println(result);
```

```
// 실행 결과

1 : 2 : 3 : 4 : 5 : 6 : 7 : 8 : 9 : 10
```


<br/><br/>


---

<br/>

## 참고
- [docs.oracle.com : Class Collectors : joining()](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#joining--)