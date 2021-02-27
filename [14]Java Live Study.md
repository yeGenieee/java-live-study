# [14주차 과제] 제네릭 (Generic)

### 제네릭 사용법

### 제네릭 주요 개념 (바운디드 타입, 와일드 카드)

### 제네릭 메소드 만들기

### Erasure

## 0. 제네릭이란

  Java 5부터 제네릭 (Generic) 타입이 새로 추가되었다. 제네릭 타입을 이용함으로써 잘못된 타입이 사용될 수 있는 문제를 컴파일 과정에서 제거할 수 있게 되었다. 제네릭은 클래스와 인터페이스, 메소드를 정의할 때 타입 (type)을 파라미터(parameter)로 사용할 수 있도록 한다. 타입 파라미터는 코드 작성 시 구체적인 타입으로 대체되어 다양한 코드를 생성하도록 해준다. 제네릭을 사용하는 코드는 비제네릭 코드에 비해 아래와 같은 이점을 가진다.

#### 1. 컴파일 시 강한 타입 체크를 할 수 있다.

- 자바 컴파일러는 코드에서 잘못 사용된 타입 때문에 발생하는 문제점을 제거하기 위해 제네릭 코드에 대해 강한 타입 체크를 한다. 
- Runtime 에러가 발생하는 것 보다는 컴파일 시에 에러를 판별하여 사전에 방지하는 것이 좋다.

#### 2. 타입 변환 (casting) 을 제거한다.

- 비제네릭 코드는 불필요한 타입 변환을 하므로, 프로그램 성능에 악영향을 미친다.

  - 예를 들어, 같은 문자열을 저장하는 List를 예시로 보자

    ```java
    List list = new ArrayList();
    list.add("hello");
    String str = (String) list.get(0);
    ```

    - item을 얻어올 때 반드시 String으로 **타입 변환 필요**

  - 제네릭 코드로 수정

    ```java
    List<String> list = new ArrayList<String>();
    list.add("hello");
    String str = list.get(0);
    ```

    - List에 저장되는 요소를 String 타입으로 국한하기 때문에 item을 찾아올 때 **타입 변환을 할 필요가 없어서** 프로그램 **성능이 향상됨**



## 1. 제네릭 사용법

### 제네릭 타입

- 타입을 파라미터로 가지는 클래스와 인터페이스
- 클래스 또는 인터페이스 이름 뒤에 `<>` 부호가 붙고, 사이에 타입 파라미터가 위치함

```java
public class 클래스명<T> { ... }
public interface 인터페이스명<T> { ... }
```

- 타입 파라미터는 일반적으로 대문자 알파벳 한 글자로 표현한다
- 제네릭 타입을 실제 코드에서 사용하려면 타입 파라미터에 구체적인 타입을 지정해야 한다



### 타입 파라미터를 사용하는 이유

```java
public class Box {
    private Object object;
    public void set(Object object) {
        this.object = object;
    }
    public Object get() {
        return object;
    }
}
```

- 위의 Box 클래스에서는 필드 타입이 `Object` 타입이다

  - 필드에 모든 종류의 객체를 저장하고 싶어서 `Object` 타입으로 선언한다

  - 모든 자바 객체는 `Object` 타입으로 자동 타입 변환되어 저장된다

    ```java
    Object object = 자바의 모든 객체;
    ```

    > 자동 타입 변환
    >
    > 2주차 참고

- `set()` 메소드에서 매개 변수 타입으로 `Object` 를 사용함으로써 매개값으로 자바의 모든 객체를 받을 수 있다

- `get()` 메소드는 `Object` 필드에 저장된 객체를 `Object` 타입으로 리턴한다

  - 만약 필드에 저장된 원래 타입의 객체를 얻으려면 **강제 타입 변환 필요**하다

    ```java
    Box box = new Box();
    box.set("hello"); // String 타입을 Object 타입으로 자동 타입 변환해서 저장
    String str = (String) box.get(); // Object 타입을 String 타입으로 강제 타입 변환해서 얻음
    ```

#### Object 타입을 사용하면

##### 장점

- 모든 종류의 자바 객체를 저장할 수 있다

##### 단점

- 빈번한 타입 변환으로 프로그램 성능이 저하될 수 있다
  - 저장할 때 타입 변환이 발생하기 때문
  - 읽어올 때도 타입 변환이 발생하기 때문



#### 모든 종류의 객체를 저장하면서 타입 변환이 발생하지 않도록 하는 제네릭

###### 수정된 Box 클래스

```java
public class Box<T> {
    private T t;
    public void set(T t) {
        this.t = t;
    }
    public T get() {
        return t;
    }
}
```

- 타입 파라미터 `T` 를 사용해서 `Object`  대체

- `T` 는 Box 클래스로 객체를 사용할 때 구체적인 타입으로 변경된다

  ```java
  Box<String> box = new Box<String>();
  ```

- 타입 파라미터 `T` 는 `String` 타입으로 변경되어 `Box` 클래스의 내부가 자동으로 재구성된다

  ```java
  public class Box<String> {
  	 private String t;
  	 public void set(String t) {
  	 		this.t = t;
  	 }
  	 public String get() {
  	 		return t;
  	 }
  }
  ```

##### 이렇게 되면, set() 과 get() 메소드를 이용할 때 전혀 타입 변환이 발생하지 않는다

### 제네릭은 클래스 설계 시 구체적인 타입을 명시하지 않고, 타입 파라미터로 대체했다가 실제 클래스가 사용될 때 구체적인 타입을 지정함으로써 타입 변환을 최소화 시킴



## 2. 제네릭 주요 개념 (바운디드 타입, 와일드 카드)

### 와일드 카드 타입 (<?>, <? extends ...>, <? super ...>)

- 제네릭 타입을 매개값이나 리턴 타입으로 사용할 때 구체적인 타입 대신에 와일드 카드(?)를 사용할 수 있다

#### 제네릭타입 <?> : Unbounded Wildcards (제한 없음)

- 타입 파라미터를 대치하는 구체적인 타입으로 모든 클래스나 인터페이스 타입이 올 수 있다

#### 제네릭타입 <? extends 상위타입> : Upper Bounded Wildcards (상위 클래스 제한)

- 타입 파라미터를 대치하는 구체적인 타입으로 상위 타입이나 하위 타입만 올 수 있다

#### 제네릭타입 <? super 하위타입> : Lower Bounded Wildcards (하위 클래스 제한)

- 타입 파라미터를 대치하는 구체적인 타입으로 하위 타입이나 상위 타입이 올 수 있다



## 3. 제네릭 메소드

### 매개 타입과 리턴 타입으로 타입 파라미터를 갖는 메소드

### 선언 방법

- 리턴 타입 앞에 `<>` 기호를 추가하고 타입 파라미터를 기술한 다음, 리턴 타입과 매개 타입으로 타입 파라미터를 사용한다

```java
public <타입파라미터,...> 리턴타입 메소드명(매개변수, ...) { ... }
```

```java
public <T> Box<T> boxing(T t) { ... }
```



### 제네릭 메소드의 호출 방법

1. 코드에서 타입 파라미터의 구체적인 타입을 명시적으로 지정한다

   ```java
   리턴타입 변수 = <구체적 타입> 메소드명 (매개값);
   ```

2. 컴파일러가 매개값의 타입을 보고 구체적인 타입을 추정하도록 한다

   ```java
   리턴타입 변수 = 메소드명(매개값);
   ```

   

#### 제네릭 메소드 호출 예제

```java
public class Util {
    public static <T> Box<T> boxing(T t) {
        Box<T> box = new Box();
        box.set(t);
        return box;
    }
}
```

```java
public class BoxingMethodExample {
    public static void main(String[] args) {
        Box<Integer> box1 = Util.<Integer>boxing(100);
        int intValue = box1.get();

        Box<String> box2 = Util.boxing("홍길동");
        String strValue = box2.get();
    }
}
```

###제한된 타입 파라미터 (<T extends 최상위타입>)

- 숫자를 연산하는 제네릭 메소드
  - 매개값으로 Number 타입 또는 하위 클래스 타입 (Byte, Short, Integer, Long, Double)의 인스턴스만 가져야한다

이렇게, 타입 파라미터에 지정되는 구체적인 타입을 제한할 필요가 종종 있다. 이것을 제한된 타입 파라미터 (bounded type parameter) 라고 한다

#### 제한된 타입 파라미터 선언

- 타입 파라미터 뒤에 `extends` 키워드를 붙이고 **상위 타입을 명시**

```java
public <T extends 상위타입> 리턴타입 메소드(매개변수, ...) { ... }
```

- 타입 파라미터에 지정되는 구체적인 타입은 상위 타입이거나, 상위 타입의 하위 또는 구현 클래스만 가능하다
- 중괄호 {} 안에서 타입 파라미터 변수로 사용 가능한 것은 상위 타입의 멤버 (필드, 메소드)로 제한된다
  - 하위 타입에만 있는 필드와 메소드는 사용할 수 없다

##### 숫자타입만 구체적인 타입으로 갖는 제네릭 메소드 예제

```
public <T extends Number> int compare(T t1, T t2) {
		double v1 = t1.doubleValue();
		double v2 = t2.doubleValue();
		return Double.compare(v1, v2);
} 
```

- `doubleValue()` 메소드는 Number 클래스에 정의되어 있는 메소드
  - 숫자를 double 타입으로 변환한다
- Double.compre() 메소드는 첫번째 매개값이 작으면 -1, 같으면 0, 크면 1을 리턴한다



```java
public class Util {
    public static <T extends Number> int compare(T t1, T t2) [
        double v1 = t1.doubleValue();
        double v2 = t2.doubleValue();
        return Double.compare(v1, v2);
    ]
}
```

```java
public class BoundedTypeParameterExample {
    public static void main(String[] args) {
        // String str = Util.compare("a", "b"); String은 Number 타입이 아니므로 호출 불가능

        int result1 = Util.compare(10,20);
        System.out.println(result1);

        int result2 = Util.compare(4.5, 3);
        System.out.println(result2);
    }
}
/**
 * -1
 * 1
 */
```



## 4. Erasure

Type Erasure

- 컴파일 타임에만 타입을 검사하고, 런타임에는 해당 타입 정보를 알지 못하는 것을 말한다

```java
public static <E> boolean constrainsElement(E [] elements, E element) {
		for (E e : elements) {
				if (e.equals(element)) {
						return true;
				}
		}
		return false;
}
```

- 컴파일러는 unbound type E를 Object 타입으로 대체하여, 타입 안정성을 확보하고 런타임 에러를 방지한다

```java
public static boolean containsElement(Object[] elements, Object element) {
		for (Object e : elements) {
				if (e.equals(element)) {
						return true;
				}
		}
		return false;
}
```



#### Type Erasure는 클래스와 메소드 레벨에서 발생할 수 있다

- 제네릭 타입에선 해당 타입 파라미터나 Object로 변경한다
- 타입 안정성 보존을 위해 필요하다면 type casting을 한다





## Reference

- 신용권, 『이것이 자바다』, 한빛미디어(2015), p.654 ~ p.671
- 이상민, 『자바의 신』, 로드북(2018), p.572~ p.583
- https://www.baeldung.com/java-type-erasure