---
layout: post
title:  "Stream API"
date:   2019-01-14
categories: [JAVA]
tags: [java, java8, Stream, Stream API]
---

# Stream
자바 8부터 추가되었으며 다량의 데이터 처리 작업을 도와주는 기능이다.  


<br/>
# 스트림 API의 구조
**스트림 생성 -> 중간 연산(intermediate operation) -> 종단 연산(terminal operation)**
- **중간 연산**은 스트림을 어떠한 방식으로 변환(transform)한다.
  - 각 원소에 함수를 적용하거나 특정 조건을 만족 못하는 원소를 걸러낼 수 있다.
- **종단 연산**은 마지막 중간 연산이 내놓은 스트림에 최후의 연산을 가한다.  
  - 원소를 정렬해 컬렉션에 담거나, 특정 원소 하나를 선택하거나, 모든 원소를 출력할 수 있다.  

~~~java
orders.stream().filter(o -> o.getPrice() > 100).count();
~~~


<br/><br/><br/>  
# 스트림 생성
배열이나 컬렉션을 이용하여 스트림을 만들 수도 있고, 그 외 특정 메서드를 이용하여 생성할 수도 있다.  
<br/>

### Arrays  
- stream(*)  

~~~java
String[] arr = new String[]{"a", "b", "c"};
Stream<String> stream1 = Arrays.stream(arr);

Stream<String> stream2 = Arrays.stream(arr, 1, 3);    // 1~2 요소 [b, c]
~~~
<br/>  

### Collection
- stream(), parallelStream()

~~~java
List<String> collection = new ArrayList<>();
Stream<String> stream = collection.stream();
~~~    
<br/>  

### Stream
- builder(), of(*), generate(...), iterate(...)

~~~java
Stream<String> builderStream = Stream.<String>builder()
        .add("a").add("b").add("c")
        .build(); // [a, b, c]

Stream<String> ofStream = Stream.of("a", "b", "c");

Stream<Integer> iteratedStream =
        Stream.iterate(10, n -> n + 2).limit(5); // [10, 12, 14, 16, 18]
~~~




<br/><br/><br/>  
# 중간 연산  
중간 작업은 스트림을 리턴하기 때문에 여러 작업을 이어 붙여서 작성할 수 있다.  
<br/>  

### filter  
- `Stream<T> filter(Predicate<? super T> predicate)`
- 스트림 내 요소들을 하나씩 평가하여, **조건에 만족하는 요소**를 제공하는 새로운 스트림을 생성한다.  
- 인자로는 boolean을 리턴하는 함수형 인터페이스 평가식이 들어간다.  

~~~java
Stream<String> stream = orders.stream()
        .filter(o -> o.getPrice() > 100);
// o로 스트림 요소를 받고, 각 요소의 가격이 100 이상인 스트림이 리턴된다
~~~  
<br/>


### map  
- `Stream<R> map(Function<? super T, ? extends R> mapper)`  
- 입력 T타입 요소를, R타입 요소로 **변환**하는 스트림을 생성한다.  

~~~java
Stream<String> stream = arr.stream()
        .map(s -> s.charAt(0));
// [apple, banana, cherry] ==> [a, b, c]

Stream<Integer> stream = orders.stream()
        .map(o -> o.getPrice());
// Order객체의 가격을 꺼낼 수도 있다(Order를 price로 맵핑) ==> [100, 90, 80]
~~~  
<br/>  


### flatMap
- `Stream<R> flatMap(Function<? super T, ? extends Stream<? extends R>> mapper)`
- T 타입 요소를 1:N의 R타입 요소로 변환한 스트림을 생성한다.
- 중첩 구조를 한 단계 제거하고 **단일 컬렉션**으로 만들어준다.  

~~~java
String[][] strArr = new String[][]{
        {"a", "b"}, {"a", "c"}, {"b", "c"}
};
Stream<String> stream = Arrays.stream(strArr)
        .flatMap(arr -> Arrays.stream(arr));
// [a, b, a, c, b, c]    
~~~  
<br/>


### sorted
- `Stream<T> sorted()`
- `Stream<T> sorted(Comparator<? super T> comparator)`
- 스트림의 요소를 **정렬**한 스트림을 생성한다.

~~~java
// 인자 없는 sorted
IntStream.of(2, 4, 1, 5, 3)
        .sorted();
// 1, 2, 3, 4, 5


// 인자 있는 sorted
Stream.of("banana", "apple", "cherry")
        .sorted(Comparator.reverseOrder());
// cherry, banana, apple
~~~  
<br/>


### skip
- Stream<T> skip(long n)
- 처음 n개의 **요소를 제외**하는 스트림을 생성한다.

~~~java
IntStream.of(2, 4, 1, 5, 3)
        .skip(3);
// 5, 3
~~~  
<br/>


### peek
- `Stream<T> peek(Consumer<? super T> action)`
- 스트림 내 요소들 각각을 대상으로 특정 **연산을 수행**한다.
- 특정 작업을 수행할 뿐 결과에는 영향을 미치지 않나, 작업을 처리하는 중간에 상태를 확인해볼 때 사용할 수 있다.

~~~java
int sum = IntStream.of(1, 2, 3, 4, 5)
        .peek(System.out::println)
        .sum();
// 1 2 3 4 5 가 출력되며, 15라는 int가 리턴된 상태
// sum()은 종단 연산에 해당한다.
~~~  



<br/><br/><br/>  
# 종단 연산
가공한 스트림을 가지고 내가 사용할 결과값으로 만들어 내는 단게이다. (스트림을 끝내는 단계)
<br/>  

### count(), sum()
- `long count()`
-

~~~java
Long count = IntStream.of(1, 2, 3, 4, 5).count();   // 5
int sum = IntStream.of(1, 2, 3, 4, 5).sum();        // 15
~~~  
<br/>


### min(), max(), average()
- `Optional<T> min(Comparator<? super T> comparator)`
- `Optional<T> max(Comparator<? super T> comparator)`
- 만약 스트림이 비어있는 경우, `count()`나 `sum`은 0을 출력하면 되지만,  
최소, 최대, 평균의 경우에는 표현할 수 없기 때문에 `Optional`을 이용하여 리턴한다

~~~java
OptionalInt optionalInt = IntStream.of(2, 1, 4, 5, 3).min();
~~~  
- `OptionalInt, OptionalLong, OptionalDouble`은 `getAsInt, getAsLong, getAsDouble` 등을 포함한다.  

~~~java
int min = IntStream.of(2, 1, 4, 5, 3).min().getAsInt();
Long max = LongStream.of(2, 1, 4, 5, 3).max().getAsLong();
Double avg = IntStream.of(1, 2, 3, 4, 5).average().getAsDouble();
~~~
<br/>  


### reduce()
- `void forEach(Consumer<? super T> con)`  
- T타입의 요소 둘 씩 reduce로 계산하여 최종적으로 하나의 값을 계산  



### forEach
- `void forEach(Consumer<? super T> con)`
- 인수로 전달된 함수에 T타입의 요소를 하나씩 전달하여 처리한다.




<br/><br/><br/><br/>  
---
### 참고
- [이펙티브 자바 3판(Effective Java 3/E)](http://www.insightbook.co.kr/13027)
- [https://futurecreator.github.io/2018/08/26/java-8-streams/](https://futurecreator.github.io/2018/08/26/java-8-streams/)

- [자바8 스트림 API 소개 - beom kyun choi](https://www.slideshare.net/madvirus/8-api)  

- [씹고 뜯고 맛보고 즐기는 스트림 API - Arawn Park](https://www.slideshare.net/arawnkr/api-42494051)
