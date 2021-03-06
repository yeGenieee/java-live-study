# [15주차과제] 람다식

### 람다식 사용법

### 함수형 인터페이스

### Variable Capture

### 메소드, 생성자 Reference

## 1. 람다식 사용법

### 람다식 (Lambda Expressions)

- 함수적 프로그래밍을 위해 자바 8부터 지원되는 익명 함수를 생성하기 위한 식
- 람다식을 통해 자바 코드가 매우 간결해지고 컬렉션의 요소를 필터링하거나 매핑하여 원하는 결과를 쉽게 얻을 수 있음
- 형태
  - 매개 변수를 가진 코드 블록 `(매개변수) -> { 실행 코드 }`
  - 런타임 시 익명 구현 객체를 생성함

### 기본 문법

```java
(타입 매개변수, ... ) -> { 실행문; ... }
```

#### (타입 매개변수, ...)

- 오른쪽 `{}` 블록을 실행하기 위해 필요한 값을 제공하는 역할
- 매개 변수의 타입은 개발자가 자유롭게 줄 수 있음

#### ->

- 매개 변수를 이용해서 `{}` 를 실행한다는 뜻

```java
(int a) -> { System.out.println(a); } // 매개 변수 a의 값을 출력
```

- 매개 변수 타입은 런타임 시에 대입되는 값에 따라 자동으로 인식될 수 있기 때문에 람다식에서는 매개변수의 타입을 일반적으로 언급하지 않음

```java
(a) -> { System.out.println(a); }
```

- 하나의 매개 변수만 있다면, 괄호 생략 가능
- 하나의 실행문만 있다면, 중괄호도 생략 가능

```java
a -> System.out.println(a);
```



- 만약 매개변수가 없어진다면, 빈 괄호를 반드시 작성해야 함

```java
() -> { 실행문; ... }
```



- 중괄호 실행 후 결과값 리턴

```java
(x, y) -> { return x + y; }
```

- 중괄호 {}에 return문만 있을 경우, 람다식에서는 return문을 사용하지 않고 아래와 같이 작성

```java
(x, y) -> x + y
```



## 2. 함수형 인터페이스

  람다식의 형태는 매개 변수를 가진 코드 블록이기 때문에 마치 자바의 메소드를 선언하는 것처럼 보여진다. 자바는 메소드를 단독으로 선언할 수 없고, 항상 클래스의 구성 멤버로 선언하기 때문에 람다식은 단순히 메소드를 선언하는 것이 아니라 이 메소드를 가지고 있는 객체를 생성해 낸다. 그러면, 어떤 타입의 객체를 생성하는 것일까?

```java
인터페이스 변수 = 람다식;
```

  람다식은 인터페이스 변수에 대입된다. 결국, 람다식은 인터페이스의 익명 구현 객체를 생성한다는 뜻이다. 인터페이스는 직접 객체화할 수 없기 때문에, 구현 클래스가 필요한데, 람다식은 익명 구현 클래스를 생성하고 객체화한다. 람다식은 대입될 인터페이스의 종류에 따라 작성 방법이 달라지기 때문에 람다식이 대입될 인터페이스를 람다식의 타겟 타입 (target type)이라고 한다.

### 함수적 인터페이스 (Functional Interface)

- 하나의 추상 메소드가 선언된 인터페이스

  모든 인터페이스를 람다식의 타겟 타입으로 사용할 수는 없다. 람다식이 하나의 메소드를 정의하기 때문에, 두 개 이상의 추상 메소드가 선언된 인터페이스는 람다식을 이용하여 구현 객체를 생성할 수 없다. 결국 하나의 추상 메소드가 선언된 인터페이스만이 람다식의 타겟 타입이 될 수 있다. 

  함수적 인터페이스를 작성할 때 두 개 이상의 추상 메소드가 선언되지 않도록 컴파일러가 체킹해주는 기능이 있는데, 인터페이스 선언 시 `@FunctionalInterface` 어노테이션을 붙여주면, 두 개 이상의 메소드 선언 시 컴파일 오류를 발생시킨다.

```java
@FunctionalInterface
public interface MyFunctionalInterface {
		public void method();
		public void otherMethod(); // 컴파일 오류 발생
}
```

#### @FunctionalInterface

- 이 어노테이션을 꼭 붙일 필요는 없음
- 어노테이션이 없더라도 하나의 추상 메소드만 있다면 모두 함수적 인터페이스가 된다



  람다식은 타겟 타입인 함수적 인터페이스가 가지고 있는 추상 메소드의 선언 형태에 따라서 작성 방법이 달라진다. 아래에서 더 자세히 살펴보자.

1. 매개 변수와 리턴값이 없는 람다식
2. 매개 변수가 있는 람다식
3. 리턴값이 있는 람다식

### 1. 매개 변수와 리턴값이 없는 람다식

- 매개 변수와 리턴값이 없는 추상 메소드를 가진 함수적 인터페이스

  ```java
  @FunctionalInterface
  public interface MyFunctionalInterface {
      public void method();
  }
  ```

- 위의 인터페이스를 타겟 타입으로 갖는 람다식은 아래와 같은 형태로 작성해야 함

  ```java
  MyFunctionalInterface fi = () -> { ... }
  ```

- 람다식이 대입된 인터페이스의 참조 변수 `fi` 는 다음과 같이 `method()` 를 호출할 수 있음

  ```java
  fi.method();
  ```

```java
public class MyFinctionalInterfaceExample {
    public static void main(String[] args) {
        MyFunctionalInterface fi;

        fi = () -> {
            String str = "method call1";
            System.out.println(str);
        }
        fi.method();

        fi = () -> { System.out.println("method call2"); };
        fi.method();

        fi = () -> System.out.println("method call3");
        fi.method();
    }
}
```



### 2. 매개 변수가 있는 람다식

- 매개 변수가 있고 리턴값이 없는 추상 메소드를 가진 함수적 인터페이스

  ```java
  @FunctionalInterface
  public interface MyFunctionalInterface {
  		public void method(int x);
  }
  ```

- 위의 인터페이스를 타겟 타입으로 갖는 람다식은 아래와 같은 형태로 작성해야 함

  ```java
  MyFunctionalInterface fi = (x) -> { ... } 또는 x -> { ... }
  ```

- 람다식이 대입된 인터페이스 참조 변수 `fi` 는 다음과 같이 `method()` 를 호출할 수 있음

  ```java
  fi.method(5);
  ```

```java
public class MyFinctionalInterfaceExample {
    public static void main(String[] args) {
        MyFunctionalInterface fi;

        fi = (x) -> {
            int result = x * 5;
            System.out.println(result);
        }
        fi.method(2);

        fi = (x) -> { System.out.println(x*5); };
        fi.method(2);

        fi = x -> System.out.println(x*5);
        fi.method(2);
    }
}
```



### 3. 리턴값이 있는 람다식

- 매개 변수가 있고 리턴값이 있는 추상 메소드를 가진 함수적 인터페이스

  ```java
  @FunctionalInterface
  public interface MyFunctionalInterface {
  		public int method(int x, int y);
  }
  ```

- 위의 인터페이스를 타겟 타입으로 갖는 람다식은 아래와 같은 형태로 작성해야 함

  ```java
  MyFunctionalInterface fi = (x,y) -> { ...; return 값; }
  ```

- 람다식이 대입된 인터페이스 참조 변수 `fi` 는 다음과 같이 `method()` 를 호출할 수 있음

  ```java
  int result = fi.method(2, 5);
  ```

```java
public class MyFinctionalInterfaceExample {
    public static void main(String[] args) {
        MyFunctionalInterface fi;

        fi = (x, y) -> {
            int result = x + y;
            return result;
        }
        System.out.println(fi.method(2, 5));

        fi = (x, y) -> { return x + y; };
        System.out.println(fi.method(2, 5));
      
      	fi = (x, y) -> x + y;
      	System.out.println(fi.method(2, 5));

        fi = (x,y) -> sum(x + y);
        System.out.println(fi.method(2, 5));
      
      	public static int sum(int x, int y) {
          	return (x + y);
        }
    }
}
```



## 3. Variable Capture

  람다 바디에서 파라미터로 넘겨진 변수가 아닌 외부에서 정의된 변수 (자유 변수)를 참조하는 행위를 Capturing 이라고 한다.

자바에서 람다식 외부에 선언된 변수 중에서 람다식 내에서 사용 가능한 변수의 범위는 아래와 같다.

1. Local variables
2. Instance variables
3. Static variables



  이 중에서 로컬 변수만 변경이 불가능하고, 나머지는 람다 내부에서 읽거나 쓰기가 가능하다. 이는 java 메모리와 관련이 있다. 

나머지 변수와 다르게 로컬 변수는 stack 영역에 저장이 된다. 즉, 어떤 쓰레드가 함수를 호출하면 stack에 메모리를 잡고, 로컬 변수와 필요한 공간을 할당하게 된다. 그리고 함수 호출이 끝나고 리턴되면 stack에서 해당 함수가 제거된다.

메소드 내에서 생성된 익명 객체는 메소드 실행이 끝나도 힙 메모리에 존재해서 계속 사용할 수 있다. 매개변수나 로컬 변수는 메소드 실행이 끝나면 스택 메모리에서 사라지기 때문에 익명 객체에서 사용할 수 없게 되므로 문제가 발생하는 것이다.

따라서, 만약 람다식에서 메소드의 매개 변수나 로컬 변수를 사용하려면 이 변수는 **final 특성**을 가져야 한다. 자바 8 이후부터는 final 키워드 없이 선언해도, 값을 수정할 수 없는 final 특성을 갖는 변수가 된다. 

  이렇게, 매개 변수 또는 로컬 변수를 람다식에서 읽는 것은 허용되지만, 람다식 내부 또는 외부에서 변경할 수는 없는 특징을 갖게된다.



## 4. 메소드, 생성자 Reference

### Method Reference

- 메소드를 참조해서 매개 변수의 정보 및 리턴 타입을 알아내어, 람다식에서 불필요한 매개 변수를 제거하는 것이 목적

  람다식은 종종 기존 메소드를 단순히 호출만 하는 경우가 많다. 아래와 같이, 예를 들어 두 개의 값을 받아 큰 수를 리턴하는 Math 클래스의 max() 정적 메소드를 호출하는 람다식은 다음과 같다.

```java
(left, right) -> Math.max(left, right);
```

  람다식은 단순히 두 개의 값을 `Math.max()` 메소드의 매개값으로 전달하는 역할만 하기 때문에 이 경우에는 메소드 참조를 이용하여 깔끔하게 변경할 수 있다.

```java
Math::max;
```



  메소드 참조도 람다식과 마찬가지로 인터페이스의 익명 구현 객체로 생성되므로 타겟 타입인 인터페이스의 추상 메소드가 어떤 매개 변수를 가지고, 리턴 타입이 무엇인가에 따라 달라진다. 메소드 참조는 정적 또는 인스턴스 메소드를 참조할 수 있고, 생성자 참조도 가능하다.

1. 정적 메소드와 인스턴스 메소드 참조
2. 매개 변수의 메소드 참조
3. 생성자 참조



### 1. 정적 메소드와 인스턴스 메소드 참조

  static 메소드를 참조할 경우에는 클래스 이름 뒤에 `::` 기호를 붙이고 static 메소드 이름을 기술하면 된다

```java
클래스 :: 메소드
```

  인스턴스 메소드일 경우에는 먼저 객체를 생성한 다음 참조 변수 뒤에 :: 기호를 붙이고 인스턴스 메소드 이름을 기술하면 된다

```java
참조변수 :: 메소드
```



### 2. 매개 변수의 메소드 참조

  메소드는 람다식 외부의 클래스 멤버일 수도 있고, 람다식에서 제공되는 매개 변수의 멤버일 수도 있다. 다음과 같이 람다식에서 제공되는 a 매개 변수의 메소드를 호출해서 b 매개 변수를 매개값으로 사용하는 경우도 있다.

```java
(a, b) -> { a.instanceMethod(b); }
```

  이를 메소드 참조로 표현하면 다음과 같다. a 클래스 이름 뒤에 :: 기호를 붙이고 메소드 이름을 기술하면 된다.

```java
클래스 :: instanceMethod
```



### 3. 생성자 참조

  생성자를 참조한다는 것은 객체 생성을 의미한다. 단순히 객체를 생성하고 리턴하도록 구성된 람다식은 생성자 참조로 대치할 수 있다.

```java
(a, b) -> { return new 클래스(a,b); }
```

 위와 같은 단순 객체 생성 후 리턴하는 람다식은 생성자 참조로 간단하게 표현 할 수 있다. 클래스 이름 뒤에 :: 기호를 붙이고 new  연산자를 기술한다. 생성자가 오버로딩되어 여러 개가 있을 경우, 컴파일러는 함수적 인터페이스의 추상 메소드와 동일한 매개 변수 타입과 개수를 가지고 있는 생성자를 찾아 실행한다. 만약, 해당 생성자가 존재하지 않으면 컴파일 오류가 발생한다.

```java
클래스 :: new
```



## Reference

- 신용권, 『이것이 자바다』, 한빛미디어(2015), p.414 ~ p.415, p.678 ~ p.716
- https://perfectacle.github.io/2019/06/30/java-8-lambda-capturing/
- http://tutorials.jenkov.com/java/lambda-expressions.html#variable-capture
- https://tourspace.tistory.com/6