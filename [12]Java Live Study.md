# [12주차 과제] Annotation

### Annotation 정의하는 방법

### @Retention

### @Target

### @Documented

### Annotation Processor

# 1. Annotation 정의하는 방법

## 1. Annotation

  클래스나 메소드 등의 선언 시에 @를 사용하는 것을 말한다. Annotation은 metadata 라고도 한다. Metadata란 애플리케이션이 처리해야 할 데이터가 아니라, 컴파일 과정과 실행 과정에서 코드를 어떻게 컴파일하고 처리할 것인지를 알려주는 정보이다. JDK 5부터 등장했다.

### Annotation의 사용 용도

1. 컴파일러에게 코드 문법 에러를 체크하도록 정보를 제공
2. 소프트웨어 개발 툴이 빌드나 배치 시 코드를 자동으로 생성할 수 있도록 정보를 제공
3. 실행 시 (Runtime) 특정 기능을 실행하도록 정보를 제공

이러한 Annotation이 프로그램에 영향을 미칠까?

영향이 있는 Annotation이 있고, 그렇지 않은 것도 있다.

## 2. Annotation 타입 정의와 적용

  Annotation 타입을 정의하는 방법은 인터페이스를 정의하는 것과 유사하다. @interface 를 사용하여 Annotation을 정의하며, 그 뒤에 사용할 annotation 이름이 온다.

```java
public @interface AnnotationName {
}
```

이렇게 정의한 Annotation은 코드에서 다음과 같이 사용한다.

```java
@AnnotationName
```



Annotation은 element(엘리먼트)를 멤버로 가질 수 있다. 각 엘리먼트는 타입과 이름으로 구성되며, 디폴트 값을 가질 수 있다.

```java
public @interface AnnotationName {
	타입 elementName() [default 값]; // element 선언
}
```

- Element의 타입으로는 int나 double과 같은 기본 데이터 타입이나 String, Enum, Class Type 그리고 이들의 배열 타입을 사용할 수 있다.
- Element의 이름 뒤에는 메소드를 작성하는 것처럼 `()` 를 붙여야 한다.

##### Element 선언 예시

```java
public @interface AnnotationName {
	String elementName1();
	int elementName2() default 5;
}
```

이렇게 정의한 Annotation을 코드에 적용할 때는 아래와 같이 기술한다.

```java
@AnnotationName(elementName1="값", elementName2=3);
또는 
@AnnotationName(elementName1="값");
```

- `elementName1` 은 default 값이 없기 때문에 반드시 값을 기술해야 한다

- `elementName2` 는 default 값이 있으므로 생략 가능하다

Annotation은 기본 element인 value를 가질 수 있다

```java
public @interface AnnotationName {
	String value();
	int elementName() default 5;
}
```

- Value element를 가진 annotation을 코드에서 적용할 때에는 아래와 같이 값만 기술할 수 있다

- 값은 기본 element인 value 값으로 자동 설정된다

  ```java
  @AnnotationName("값");
  ```

- 만약, value element와 다른 element의 값을 동시에 주고 싶다면, 아래와 같이 지정하면 된다

  ```java
  @AnnotationName(value = "값", elementName=3);
  ```

  

# 2. @Retention (Annotation 유지 정책)

### Annotation을 선언하기 위한 Meta Annotation

  Meta Annotation 이란, Annotation을 선언할 때 사용한다.

1. @Target
2. @Retentation
3. @Documented
4. @Inherited

의 4개가 존재한다. Annotation을 선언할 때 꼭 위의 4개를 모두 사용해야 하는 것은 아니지만, 4개가 있다는 것을 알아두면 좋다. 먼저 `@Retention` 부터 살펴보자

### 얼마나 오래 Annotation 정보가 유지되는지를 선언할 때 사용

```java
@Retention(RetentionPolicy.RUNTIME)
```

##### 괄호 내 지정하는 적용 가능 대상

|         | 대상                                                         |
| ------- | ------------------------------------------------------------ |
| SOURCE  | 어노테이션 정보가 컴파일시 사라짐                            |
| CLASS   | 클래스 파일에 있는 어노테이션 정보가 컴파일러에 의해서 참조 가능함.<br />하지만, Virtual Machine 에서는 사라짐 |
| RUNTIME | 실행시 어노테이션 정보가 가상 머신에 의해서 참조 가능        |

#### @Retention 사용 방법

```java
@Retention(RetentionPolicy.RUNTIME)
public @interface AnnotationName {
}
```



# 3. @Target (Annotation 적용 대상)

### Annotation을 어떤 것에 적용할지를 선언할 때 사용

```java
@Target(ElementType.METHOD)
```

##### 괄호 내 지정하는 적용 가능 대상

| 요소 타입      | 대상                               |
| -------------- | ---------------------------------- |
| CONSTRUCTOR    | 생성자 선언시                      |
| FIELD          | Enum 상수를 포함한 field값 선언시  |
| LOCAL_VARIABLE | 지역 변수 선언시                   |
| METHOD         | 메소드 선언시                      |
| PACKAGE        | 패키지 선언시                      |
| PARAMETER      | 매개 변수 선언시                   |
| TYPE           | 클래스, 인터페이스, enum 등 선언시 |

#### @Target 사용 방법

```java
@Target({ElementType.TYPE, ElementType,FIELD, ElementType.METHOD})
public @interface AnnotationName {
}
```



# 4. @Documented

### 해당 Annotation에 대한 정보가 Javadocs (API) 문서에 포함된다는 것을 선언할 때 사용

#### @Documented 사용 방법

```java
@Documented
public @interface AnnotationName {
}
```



# 5. Annotation Processor

  Annotation Processor는 컴파일 시간에 어노테이션들을 스캐닝하고 프로세싱하는 javac 에 속한 빌드툴(자바 컴파일러 플러그인 중 하나로 보면됨) 로, 컴파일 단계에서 어노테이션만 가지고 자바코드를 만들어내는데 사용된다. 어노테이션에 대한 코드를 수정하고 검사하는 역할을 한다. Lombok, JPA, QueryDSL 등에서 활용한다. 

어노테이션 프로세서를 활용하면 어노테이션만으로도 여러가지 모델 클래스들을 만들어낼 수 있는 팩토리를 구성할 수 있다.



## Reference

- 신용권, 『이것이 자바다』, 한빛미디어(2015), p.269 ~ p.276
- 이상민, 『자바의 신』, 로드북(2018), p.432 ~ p.444
- https://one-delay.tistory.com/80
- https://medium.com/@jason_kim/annotation-processing-101-%EB%B2%88%EC%97%AD-be333c7b913
- https://im-recording-of-sw-studies.tistory.com/37