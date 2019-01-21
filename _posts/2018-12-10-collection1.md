---
layout: post
title: "[Java] Collection"
category: java
tags: [java]
---
## Collection package

**collectionTest01.java**  

```java
package my.examples.javaexam;
import java.util.HashSet;
import java.util.Iterator;
import java.util.Set;

public class CollectionTest01 {
    public static void main(String[] args){
        Set<String> set  = new HashSet<>();

        set.add("Hello");
        set.add("!!!");
        set.add("!!!"); // false 반환

        Iterator<String> iterator = set.iterator();

        while(iterator.hasNext()){
            String str = iterator.next();
            System.out.println(str);
        }

    }
}
```  
<li> import는 메모리에 올리라는 뜻이 아니라 컴파일러, JVM에게 클래스가 어떤 패키지에 있는지 알려주려는 것이다.</li>
<li>  String은 lang package 에 있으므로 따로 import 안해도 된다. </li>
<li> Collection, HashSet 은 util package 에 있어서 import 해야 한다. </li>
<li> Collection을 레퍼런스 타입으로 사용하는 것을 바람직하지 않다. </li>
<li> Set, List, Map을 레퍼런스 타입으로 사용하도록 한다. </li>    

--------------

**collectionTest02.java**  

```java
package my.examples.javaexam;

import java.util.ArrayList;
import java.util.List;

public class CollectionTest02 {
    public static void main(String[] args){
        List<String> list = new ArrayList<>();
        //  interface타입으로 쓰는 것이 좋다.
        //  List 인터페이스를 구현해주는 ArrayList 클래스의 인스턴스.
        list.add("hello");  // index 0
        list.add("world");  // index 1
        list.add("!!!");    // index 2

        for(int i = 0; i < list.size(); i++){   
        // size()는 collection의 메서드.
            System.out.println(list.get(i));
        }
    }
}
```
<li> List Interface 는 중복이 가능하고 순서를 기억하는 자료구조다.</li>
<li> ArrayList, LinkedList, Stack, Vector 등이 List Interface를 구현한다.</li>

---------------------
**collectionTest03.java**
```java
package my.examples.javaexam;

import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;
import java.util.Set;

public class CollectionTest03 {
    public static void main(String[] args){
        Map<String, String> map = new HashMap<>();
        map.put("001","둘리");
        map.put("002","도우너");
        map.put("003","또치");

        System.out.println(map.get("002"));
        System.out.println(map.get("004"));

        Set<String> keySet = map.keySet();
        Iterator<String> keyIterator = keySet.iterator();
        while(keyIterator.hasNext()){
            String key = keyIterator.next();
            System.out.println("key : "+ key + "  --  value : "+ map.get(key));
        }
    }
}
```
<li>Map interface는 key-value 형태로 값을 관리하는 자료구조다.</li> <li>다음과 같은 메소드를 가진다.</li>

  * put(key, value)  
  * get(key)  --> return Value  
  * keySet()  --> return Set

<li>key값은 중복이 허용되지 않고 순서가 없는 자료구조로 Set으로 나타낼 수 있다.</li>
<li>keySet()이 Set을 반환하기 때문에 Map은 Set에 의존관계이다.</li>    

------------------------
**collectionTest04.java**
```java
package my.examples.javaexam;

import java.util.ArrayList;
import java.util.List;
import java.util.Random;

public class CollectionTest04 {
    public static void main(String[] args) {
        List<Integer> list = new ArrayList<>();
        // 1. 1 ~ 45 까지 숫자를 순서대로 list에 저장.
        for (int i = 0; i < 45; i++) {
            list.add(i + 1);
            //list.add(new Integer(i)); --> 오토박싱, 언박싱
        }

        Random rn = new Random();
        int index1 = 0;
        int index2 = 0;
        int tmp = 0;
        // 2. 랜덤한 위치의 두 값을 꺼내 서로 위치를 바꿔 저장한다.
        //    이를 100번 반복한다.

        // Math.random()  0.0 <= x < 1.0 실수.  ex) 0.345
        // index1 = (int)(Math.random() * 45); 와 같다.
        for (int i = 0; i < 100; i++) {
            index1 = rn.nextInt(45);
            index2 = rn.nextInt(45);
            if(index1 != index2){
                tmp = list.get(index1);
                list.set(index1, list.get(index2));
                list.set(index2, tmp);
            }
        }

        // == Collections.shuffle(list);
        // 위의 반복문과 같은 역할. 값을 섞어준다. (순서가 있는 list만 사용가능.)
        // 클래스명.메소드명    --> static 메소드.
        // Collections , Collection 둘 다 알아야한다.

        // 3. list의 index 0~5에 위치한 값들을 출력한다.
        for (int i = 0; i < 6; i++) {
            System.out.print(list.get(i) + "\t");
        }
        System.out.println();


    }

}
```
