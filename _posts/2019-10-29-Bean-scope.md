---
layout: post
title:  "[Spring] Bean scopes"
date:   2019-10-29
categories: [ETC]
tags: [Spring, bean, scope, singleton, prototype]
---

# Bean scopes
- **singleton scope** (**싱글톤**)
- **prototype scope** (**프로토타입**)
> `Spring Web Application`에서만 유효한 request, session, application, websocket 등의 스코프도 있다.
- Spring은 기본적으로 모든 bean을 singleton으로 생성하여 관리한다.
    - 명시적으로 별도의 스코프가 지정되지 않으면 기본적으로 모든 bean은 singleton
    - *Spring을 통해 제공받은 bean은 동일한 객체*!


<br/>


## Singleton scope
- singleton bean은 Spring IoC container에서 한 번 생성된다.
- 생성된 하나의 인스턴스는 singleton beans 캐시에 저장되었다가, 해당 bean에 대한 요청과 참조가 있다면 캐시된 객체를 반환한다.
    - 하나만 생성되기 때문에, 동일한 것을 참조하게 됨.
- `per-container and per-bean`
    - 하나의 Spring Container에서 하나의 bean을 정의하면, 해당 Spring Container는 정의된 bean에 대한 하나의 인스턴스만을 생성한다.

```xml
<!-- xml 설정 (singleton이 default라 굳이 명시할 필요는 없음) -->
<bean id="..." class="..." scope="singleton"/>
```

```java
// annotation 설정 (대상 클래스에 붙여주기)
@Scope("singletone")
```


<br/>


## Prototype scope
- prototype bean은 모든 요청에서 새로운 인스턴스를 생성하여 반환한다.
- 의존 관계에서 주입될 때나 또는 `getBean()`을 통해 호출될 때, prototype scope에서는 새로운 인스턴스가 생성되어 주입된다.
    - Spring Container의 역할은 `new` 연산자를 대체한 것이라 볼 수도 있다.
- Spring은 prototype bean의 전체 라이프 사이클을 관리하지 않는다.
    - destroy에 대한 콜백은 호출되지 않음

```xml
<!-- xml 설정 -->
<bean id="..." class="..." scope="prototype"/>
```

```java
// annotation 설정 (대상 클래스에 붙여주기)
@Scope("prototype")
```




<br/><br/>




# 각 스코프에 적합한 객체


<br/>


## singleton으로 적합한 객체


<br/>


### 1. 상태가 없는 공유 객체
상태를 가지고 있지 않은 객체는 동기화 비용이 없다. 따라서 매번 이 객체를 참조하는 곳에서 새로운 객체를 생성할 이유가 없다.

### 2. 읽기용으로만 상태를 가진 공유 객체
1번과 유사. 상태를 가지고 있으나 읽기 전용이므로 여전히 동기화 비용이 들지 않는다. 매 요청마다 새로운 객체 생성할 필요가 없다. 

### 3. 공유가 필요한 상태를 지닌 공유 객체
객체 간의 반드시 공유해야 할 상태를 지닌 객체가 하나 있다면, 이 경우에는 해당 상태의 쓰기를 가능한 동기화 할 경우 singleton도 적합하다.

### 4. 쓰기가 가능한 상태를 지니면서도 사용빈도가 매우 높은 객체
애플리케이션 안에서 정말로 사용빈도가 높다면, 쓰기 접근에 대한 동기화 비용을 감안하고서라도 singleton을 고려할만하다.  
아래의 경우, singleton은 유용한 선택이 될 수 있다.
- 장시간에 걸쳐 매우 많은 객체가 생성될 때
- 해당 객체가 매우 작은 양의 쓰기상태를 가지고 있을 때
- 객체 생성비용이 매우 클 때


<br/>


## non-singleton으로 적합한 객체


<br/>


### 1. 쓰기가 가능한 상태를 지닌 객체
쓰기가 가능한 상태가 많아서 동기화 비용이 객체 생성 비용보다 크다면 singleton으로 적합하지 않다.

### 2. 상태가 노출되지 않은 객체
일부 제한적인 경우, 내부 상태를 외부에 노출하지 않는 bean을 참조하여 다른 의존객체와는 독립적으로 작업을 수행하는 의존 객체가 있다면 singleton보다 비singleton 객체를 사용하는 것이 더 나을 수 있다.




<br/><br/>




# Singleton bean과 Prototype bean의 의존 관계


<br/>


## prototype이 singleton에 의존하는 경우
- 문제되지 않음
    - 항상 동일한 인스턴스만을 참조


<br/>


## singleton이 prototype에 의존하는 경우
> 추가 예정



<br/><br/>

---

<br/>

## 참고
- [docs.spring.io : 1. The IoC Container : 1.5. Bean Scopes](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-scopes)
- [Heee's Development Blog : Spring Bean의 개념과 Bean Scope 종류](https://gmlwjd9405.github.io/2018/11/10/spring-beans.html)
- [goodGid's Dev Blog : Spring 프레임워크 핵심 기술 - Bean의 Scope](https://goodgid.github.io/Spring-Framework-Bean-Scope/)
