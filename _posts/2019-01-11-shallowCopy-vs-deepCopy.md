---
layout: post
title:  "Shallow Copy and Deep Copy "
date:   2019-01-11
categories: [JAVA]
tags: [java, 얕은복사, 깊은복사]
---

# 얕은 복사(Shallow Copy)와 깊은 복사(Deep Copy)
얕은 복사는 참조 복사(call by reference)로,  
깊은 복사는 값 복사(call by value)로 볼 수 있다.

---

## 예시

``` java

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

class Fruit {
    private String name;
    private int count;

    public Fruit(String name, int count) {
        this.name = name;
        this.count = count;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getCount() {
        return count;
    }

    public void setCount(int count) {
        this.count = count;
    }

    @Override
    public String toString() {
        return "Fruit{" +
                "name='" + name + '\'' +
                ", count=" + count +
                '}';
    }
}

public class CopyTest_Object {
    static List<Fruit> origin = new ArrayList<>();
    static List<Fruit> copy = new ArrayList<>();

    private static void shallowCopy() {
        copy = origin;
        copy.add(new Fruit("cherry", 3));
        Fruit f = copy.get(1);
        f.setCount(100);
    }

    private static void deepCopy() {
        copy = new ArrayList<>(origin);
        copy.add(new Fruit("cherry", 3));
        Fruit f = copy.get(1);
        f.setCount(100);
    }

    public static void printArr() {

        System.out.println("============= origin ==============");
        for(int i = 0 ; i < origin.size() ; i++) {
            System.out.println("origin [" + i + "] : " + origin.get(i));
        }

        System.out.println("\n============= copy ==============");
        for(int i = 0 ; i < copy.size() ; i++) {
            System.out.println("copy [" + i + "] : " + copy.get(i));
        }
    }

    public static void main(String[] args) {
        origin.add(new Fruit("apple", 1));
        origin.add(new Fruit("banana", 2));

        // operation
    }
}
```

<br/>
## 1. 얕은 복사

```java
shallowCopy();
printArr();
```
결과 :
```
============= origin ==============
origin [0] : Fruit{name='apple', count=1}
origin [1] : Fruit{name='banana', count=100}
origin [2] : Fruit{name='cherry', count=3}

============= copy ==============
copy [0] : Fruit{name='apple', count=1}
copy [1] : Fruit{name='banana', count=100}
copy [2] : Fruit{name='cherry', count=3}
```  

<br/>
## 2. 깊은 복사 <small>(일까요? **아닙니다! 진정한 깊은 복사는 3번에서 확인!** )</small>

```java
deepCopy();
printArr();
```  
결과 :
```
============= origin ==============
origin [0] : Fruit{name='apple', count=1}
origin [1] : Fruit{name='banana', count=100}

============= copy ==============
copy [0] : Fruit{name='apple', count=1}
copy [1] : Fruit{name='banana', count=100}
copy [2] : Fruit{name='cherry', count=3}
```
깊은 복사에서 기대했던 값은,  
`copy`에서 1번째 인덱스의 count를 100으로 변경했을 때,  
`origin`의 1번째 인엑스의 count는 영향을 받지 않는 것이었다.  
하지만 위 결과에서는 `origin`도 영향을 받음.
~~*이것은 진정한 깊은 복사라 할 수 없다*~~  

<br/>
## 3. <small>진정한</small> 깊은 복사  

(먼저 Fruit 클래스에서 **clone()**을 오버라이드 해주었다.)  
```java
class Fruit {
...
    @Override
    public Fruit clone() {
        return new Fruit(this.name, this.count);
    }
...
}
```  

(이제 operation 부분)  
```java
List<Fruit> deepCopy = origin.stream()
        .map(Fruit::clone)
        .collect(Collectors.toList())
;

deepCopy.add(new Fruit("cherry", 3));
Fruit f = deepCopy.get(1);
f.setCount(100);

printArr();
```  
결과 :

```
============= origin ==============
origin [0] : Fruit{name='apple', count=1}
origin [1] : Fruit{name='banana', count=2}

============= deepCopy ==============
deepCopy [0] : Fruit{name='apple', count=1}
deepCopy [1] : Fruit{name='banana', count=100}
deepCopy [2] : Fruit{name='cherry', count=3}
```  
이제 `deepCopy`에서 1번째 인덱스에 대한 count값을 변경하여도  
`origin`에는 영향을 미치지 않는다!

<br/>
### 참고
- [http://library1008.tistory.com/47](http://library1008.tistory.com/47)
