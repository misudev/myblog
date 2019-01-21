---
published: true
layout: post
title: "[Java] Exception"
category: java
tags: [java]
comments: true
---
Exception 처리
----------------
[즐거운 자바 9편 Exception 처리](https://www.youtube.com/watch?time_continue=2850&v=aCU4tbEKhW4)

-------------
인스턴스가 만들어지면 heap메모리에 올라간다.

메소드가 실행 되면 현재 실행되는 메소드 정보가 Stack Entry라고 불리는 공간에 저장이 되고 그 Stack Entry는 Java Stack에 한 건 저장된다.


JVM에서 한줄씩 실행하다가 _int value = i/k;_  실행할때 예외가 발생하면 당시 스택에 있던 메서드의 정보와 에러 메시지가 출력되며 프로그램은 죽는다.

```java
public class CalBean {

  public static void main(String[] args){
    int i = 6;
    int k = 0;
    int value = divide(i, k);  // 내부적으로(JVM) throw ArithmeticException
    System.out.println(value);
  }
  public static int divide(int a , int b){
    int value = a / b;
    return value;
  }
}
```  
메소드 안에 선언된 변수는 java stack에 저장된다.  
에러 발생시 java stack에 있던 메소드를 순차적으로 꺼내 에러메시지로 출력한다. 이를 통해 메소드 호출 순서를 알 수 있다.  
![image-center](/assets/image/exception1.jpg){: .align-center}

----------------------------------------

Exception은 크게 2가지 종류가 있다.  
1. **RuntimeException** : RuntimeException을 상속받는 Exception. 실행 되지만 실행시 오류가 발생한다.
2. **CheckedException** : RuntimeException을 상속받지 않고, Exception을 상속받은 클래스. Exception처리를 하지 않으면 컴파일 오류가 발생한다.  

메소드를 만들때 파라미터가 올바르지 않으면 아예 그 메소드를 실행할 필요가 없다.  즉 <U>올바르지 않은 결과를 반환하는 것보다  Exception을 발생하는 것이 좋을 수 있다.</U> 파라미터가 올바르지 않을 경우 강제로 Exception을 발생시킨다.  
 - method 내에서 **throw new OOOExeption** : Exception 발생.

Exception을 발생시키면 이 메소드가 어떤 Exception이 발생할 수 있는지 알려줘야한다.   
 - method 뒤에 **throws OOOExeption** :  사용자가 Exception을 처리하라. (여러개 가능)



 사용자 예외 정의시 너무 중요한 Exception만 CheckedException 으로 해주고 대부분은 RuntimeException으로 구현해준다.  


**try-catch-finally**
 ```
  try{


    예외 발생 가능 코드

  }catch(예외클래스 e) {
    예외 처리
  }finally{
    예외가 발생했든 안했든 항상 실행
  }
 ```
 try-catch-finally 블록으로 예외 처리를 해준다.

 --------------------------------------------
 **최종 예제  (사용자 정의 예외 클래스)**
```java
public class CalBean {

    public static void main(String[] args){
        int i = 6;
        int k = 0;
        int value = divide(i , k);  // 내부적으로(JVM) throw ArithmeticException
        System.out.println(value);
    }
}

class Cal {
    public static int divide(int a , int b) throws ParameterException{
        if( b == 0 ){
            throw new ParameterException("0으로 나눌 수 없습니다.");
        }
        int value = 0;
        value = a / b;
        return value;
    }
}

class ParameterException extends RuntimeException{
    public ParameterException(String msg){
        super(msg);
    }
    public ParameterException(Exception ex){
        super(ex);
    }
}

```
