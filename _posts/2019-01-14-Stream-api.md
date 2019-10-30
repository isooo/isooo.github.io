---
layout: post
title:  "[Java] Stream API"
date:   2019-01-14
categories: [ETC]
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

~~~java
Long count = IntStream.of(1, 2, 3, 4, 5).count();   // 5
int sum = IntStream.of(1, 2, 3, 4, 5).sum();        // 15
~~~  
<br/>


### min(), max(), average()
- `Optional<T> min(Comparator<? super T> comparator)`
- `Optional<T> max(Comparator<? super T> comparator)`
- `OptionalDouble average()`
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
- 최종적으로 어떠한 결과를 만들어내기 위해 사용하는 메소드이다.
- `Optional<T> reduce(BinaryOperator<T> accumulator)`  
  - <strong>accumulator</strong>
    - 각 요소를 처리하는 계산 로직. 각 요소가 올 때마다 중간 결과를 생성하는 로직
    - 같은 타입의 인자 두 개를 받아, 같은 타입의 결과를 반환하는 함수형 인터페이스
~~~java
Optional<Integer> result = numbers.stream()
        .reduce((x, y) -> x > y ? x : y);
~~~
- `T reduce(T identity, BinaryOperator<T> accumulator)`  
  - <strong>identity</strong>
    - 계산을 위한 초기값. 스트림이 비어서 계산할 내용이 없더라도 이 값은 리턴
~~~java
Integer multi = numbers.stream()
        .reduce(1, (x, y) -> x * y);
~~~
- `<U> U reduce(U identity, `  
  `BiFunction<U, ? super T, U> accumulator, `  
  `BinaryOperator<U> combiner)`  
  - <strong>combiner</strong>
    - 병렬(parallel)스트림에서 나눠 계산한 결과를 하나로 합치는 로직
~~~java
Double reduce = numbers.parallelStream()
        .reduce(0.0,
                (val1, val2) -> Double.valueOf(val1 + val2 / 10),
                (val1, val2) -> val1 + val2 );
~~~

~~~java
// 인자 하나(accumulator) 예시
// 1 + 2
// (1 + 2) + 3
// ((1 + 2) + 3) + 4
// ... 최총값 15
OptionalInt reduced =
        IntStream.range(1, 6) // [1, 2, 3, 4, 5]
                .reduce((a, b) -> (a + b));

// 인자 둘(identity) 예시
// -10 + 1
// (-10 + 1) + 2
// ... 최종값 5
int reduced =
        IntStream.range(1, 6)
                .reduce(-10, (a, b) -> (a + b));

// 인자 셋(combiner) 예시
// (a + b)작업과 병렬로 (a * b)작업이 일어남
// 1 + 30, 1 + 10, 1 + 40
// (1 + 30) * (1 + 40), 1 + 50,
// 1 + 20, ((1 + 30) * (1 + 40)) * (1 + 50)
// ... 최종적값 14973651
Integer reducedParallel = Arrays.asList(10, 20, 30, 40, 50)
        .parallelStream()
        .reduce(1,
                (a, b) -> a + b,
                (a, b) -> a * b);
// 로그확인용
Integer reducedParallelTest = Arrays.asList(10, 20, 30, 40, 50)
        .parallelStream()
        .reduce(1,
                (a, b) -> {
                    System.out.println(a + " + " + b);
                    return a + b;
                },
                (a, b) -> {
                    System.out.println("\t\t" + a + " * " + b);
                    return a * b;
                });                

~~~  
<br/>  


### collect
- 스트림의 요소를 모으고자 할 때 사용한다.

> <small> *`<R, A> R collect(Collector<? super T, A, R> collector)`*  
  *T: 입력 타입, A: 결과 축적용 타입, R: 최종 타입*  </small>

- <small>위의 방법을 사용할 수도 있지만,</small>  
java에서 제공하는 **Collectors 클래스**를 이용하여 좀 더 간단하게 처리할 수 있다.
- `Collectors.toList()`, `Collectors.toSet()`, `Collectors.toMap()`

~~~java
List<Integer> intList = Arrays.asList(1, 2, 3, 4, 5)
        .stream()
        .skip(2)    // 3, 4, 5
        .collect(Collectors.toList());        
~~~  
  - `Collectors.joining()`
    - 스트림에서 작업한 결과를 하나의 문자열로 모을 수 있다.    

~~~java
String str = Arrays.asList("a", "b", "c","d", "e")
        .stream()
        .skip(2)
        .collect(Collectors.joining());
// cde
~~~
<br/>


### forEach
- `void forEach(Consumer<? super T> con)`
- 인수로 전달된 함수에 T타입의 요소를 하나씩 전달하여 처리한다.

~~~java
IntStream.of(2, 4, 1, 5, 3)
        .sorted()
        .forEach(System.out::println);
// 1 2 3 4 5        
~~~  


<br/><br/><br/>  
## 정리
- 각 단계별 메소드는 위 내용 외에도 많이 있으니 찾아보고 필요에 따라 사용하면 될듯하다.
- stream을 사용하면 코드가 단순해 보인다는 장점이 있지만, 원치 않은 결과를 받을 수도 있으므로 잘 사용해야할 듯 하다.



<br/><br/><br/><br/>  
---
### 참고
- [이펙티브 자바 3판(Effective Java 3/E)](http://www.insightbook.co.kr/13027)  
-  [https://futurecreator.github.io/2018/08/26/java-8-streams/](https://futurecreator.github.io/2018/08/26/java-8-streams/)  
- [자바8 스트림 API 소개 - beom kyun choi](https://www.slideshare.net/madvirus/8-api)  
- [씹고 뜯고 맛보고 즐기는 스트림 API - Arawn Park](https://www.slideshare.net/arawnkr/api-42494051)  
- [자바8 Streams API 를 다룰때 실수하기 쉬운것 10가지](http://hamait.tistory.com/547)  
