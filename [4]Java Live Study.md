# [4주차 과제] 제어문

[TOC]

### 1. 선택문

### 2. 반복문

# 0. 코드 실행 흐름 제어

- 자바 프로그램을 시작하면. main() 메소드의 시작 중괄호 { 에서 시작하여 끝 중괄호 } 까지 위에서 아래로 실행하는 흐름을 가짐

- 이러한 실행 흐름을 개발자가 원하는 방향으로 바꿀 수 있도록 해주는 것이 흐름 제어문 (= 제어문)

### 제어문

- 조건식 + 중괄호 {} 블록
- 조건식의 연산 결과에 따라 블록 내부의 실행 여부가 결정됨

### 제어문의 종류

#### 1. 조건문

- if문, switch문
- 제어문 블록이 실행 완료된 경우, 제어문 블록을 빠져나와 정상 흐름으로 다시 돌아옴

#### 2. 반복문

- for문, while문, do-while문
- 제어문 블록이 실행 완료된 경우, 제어문 처음으로 다시 되돌아가 반복 실행함

#### 제어문 블록 내부에는 또 다른 제어문을 사용 가능

- if문 내부에 for문, while문을 가질 수 있음
- 개발자가 원하는 복잡한 흐름 제어 가능



# 1. 선택문

## 1.1 if문

### 조건식의 결과에 따라 블록 실행 여부가 결정

- 조건식이 true 또는 false 값을 산출할 수 있는 연산식이나, boolean 변수가 올 수 있음
- 조건식이 true이면 블록을 실행하고 false면 블록을 실행하지 않음

### if문 예시

```java
if (조건식) {
		실행문;
		실행문;
}
```



## 1.2 if-else문

- if문은 else 블록과 함께 사용되어 조건식의 결과에 따라 실행 블록을 선택
- if문의 조건식이 true이면 if문의 블록이 실행되고, 조건식이 false이면 else 블록이 실행됨

### if-else문 예제

```java
public class IfElseExample {
    public static void main(String[] args) {
        int score = 90;

        if (score >= 90) {
            System.out.println("점수가 90보다 큽니다.");
        } else {
            System.out.println("점수가 90보다 작습니다.");
        }
    }
}
```



## 1.3 if-else if-else문

- 조건이 여러 개인 if문의 경우 처음 if문의 조건식이 false일 경우 다른 조건식의 결과에 따라 실행 블록을 선택할 수 있는데, if 블록의 끝에 else if문을 붙이면 됨
- else if문의 개수는 제한이 없고 여러 개의 조건식 중 true가 되는 블록만 실행하고 전체 if문을 벗어남
- else if블록의 마지막에는 else 블록을 추가할 수 있는데, 모든 조건식이 false일 경우  else 블록을 실행하고 if문을 벗어남

### if-else if-else문 예제

```java
public class IfElseIfElseExample {
    public static void main(String[] args) {
        int score = 90;

        if (score >= 90) {
            System.out.println("점수가 90보다 큽니다.");
        } else if (score >= 80) {
            System.out.println("점수가 80 ~ 89입니다.");
        } else if (score >= 70) {
            System.out.println("점수가 70 ~ 79입니다.");
        } else {
        		System.out.println("점수가 70미만 입니다.");
        }
    }
}
```



## 1.4 중첩 if문

### if문의 블록 내부에 또 다른 if문을 사용하는 것

- 중첩의 단계는 제한이 없음

### 중첩 if문 예제

```java
public class IfNestedExample {
    public static void main(String[] args) {
        int score = 90;
      	String grade;

        if (score >= 90) {
            if (score >= 95) {
              	grade = "A+";
            } else {
              	grade = "A";
            }
        } else if (score >= 80) {
            if (score >= 85) {
              	grade = "B+";
            } else {
              	grade = "B";
            }
            
        } else if (score >= 70) {
            if (score >= 75) {
              	grade = "C+";
            } else {
              	grade = "C";
            }
        } else {
        		grade = "D";
        }
    }
}
```



## 1.5 switch문

### 변수가 어떤 값을 갖느냐에 따라 실행문이 선택되는 조건 제어문

- if문은 조건식이 true일 경우에 블록 내부의 실행문을 실행함
- switch문은 괄호 안의 값과 동일한 값을 갖는 case로 가서 실행문을 실행시킴
- 괄호 안의 값과 동일한 값을 갖는 case가 없으면 default로 가서 실행문을 실행시킴
- default는 생략 가능

### switch문 예제

```java
public class SwitchExample {
    public static void main(String[] args) {
        int num = (int)(Math.random() * 6) + 1;
      	switch (num) {
          case 1:
            System.out.println("1번이 나왔습니다.");
            break;
          case 2:
            System.out.println("2번이 나왔습니다.");
            break;
          case 3:
            System.out.println("3번이 나왔습니다.");
            break;
          case 4:
            System.out.println("4번이 나왔습니다.");
            break;
          case 5:
            System.out.println("5번이 나왔습니다.");
            break;
          case default:
            System.out.println("6번이 나왔습니다.");
            break;
        }
}
```

- case 끝에 break가 붙어있는 이유
  - 다음 case를 실행하지 말고 switch문을 빠져나가기 위함
  - break가 없으면 다음 case 값과는 상관없이 연달아 실행됨

# 2. 반복문

### 어떤 코드들이 반복적으로 실행되도록 할 때 사용됨

### 반복문의 종류

1. for문
2. while문
3. do-while문

- for문과 while문은 서로 변환이 가능
- for문
  - 주로 반복 횟수를 알고 있는 경우 사용
- while문
  - 주로 조건에 따라 반복할 때 사용
- do-while문
  - 반복문 내 실행문을 먼저 최초 1회 실행하고 조건에 따라 반복

## 2.1 for문

### 주어진 횟수만큼 실행문을 반복 실행할 때 적합한 반복 제어문

- 아래 코드는 1부터 5까지의 합을 구하는 코드

```java
int sum = 0;
sum = sum + 1;
sum = sum + 2;
sum = sum + 3;
sum = sum + 4;
sum = sum + 5;
```

- 이렇게 작성한다면, 코드 양이 엄청 늘어난다.

- for문을 이용하면, 1부터 100까지의 합도 쉽게 구할 수 있음

  ````java
  int sum = 0;
  for (int i=1; i<=100; i++) {
  	sum = sum + i;
  }
  ````

  - for문이 한 번 작성된 실행문을 여러번 반복 실행해주므로 코드가 간결해짐

### 작성 방법

```java
for (1. 초기화식; 2. 조건식; 4. 증감식) {
		3. 실행문; (2. 조건식이 true이면 3. 실행)
}
5. 실행문 (2.조건식이 false이면 5. 실행)
```

### for문 예제

- 0부터 9까지 출력

```java
for (int i=0; i<10; i++) {
  System.out.println(i);
}
```



## 2.2 while문 

### 조건식이 true일 경우에 계속해서 반복

### 작성 방법

```java
while (1. 조건식) {
		2. 실행문; (1. 조건식이 true이면 2. 실행)
}
3. 실행문 (1.조건식이 false이면 반복문을 빠져나와 3. 실행)
```



### while문 예제

- 1부터 100까지의 합 구하기

```java
public class WhileSumFrom1To100Example{
    public static void main(String[] args) {
        int sum = 0;

        int i = 1;

        while (i<=100) {
            sum += i;
            i++;
        }
    }
}
```

- while문 내에서 계속 누적되는 값을 갖는 변수는 while문 시작 전에 미리 선언해두어야 함

## 2.3 do-while문

### 조건식에 의해 반복 실행한다는 점은 while문과 동일, 대신 do-while문은 실행문을 최초 실행 후에 조건식에 따라 반복한다는 점에 차이가 있음

### 작성 방법

```java
do {
		1. 실행문; (1. 우선 실행, 2.조건식이 true인 경우 반복 실행)
} while (2. 조건식);
3. 실행문 (2.조건식이 false이면 반복문을 빠져나와 3. 실행)
```



### do-while문 예제

- q를 입력할 때 까지 반복

```java
public class DoWhileExample{
    public static void main(String[] args) {
        System.out.println("메시지를 입력하세요, 프로그램을 종료하려면 q를 입력하세요.");
        Scanner scanner = new Scanner(System.in);
        String input;

        do {
            System.out.print("입력 : ");
            input = scanner.nextLine();
            System.out.println(input);
        } while (input.equals("q"));

        System.out.println("프로그램 종료");
    }
}
```



## 2.4 break문

### 반복문을 실행 중지할 때 사용

- break문을 만나면, 반복문을 빠져나감

### break로 while문 종료 예제

- num이 6인 경우 반복문 종료

```java
public class BreakExample{
    public static void main(String[] args) {
        while (true) {
            int num = (int)(Math.random() * 6) + 1;
            System.out.println(num);
            if (num == 6) {
                break;
            }
        }
        System.out.println("프로그램 종료");
    }
}
```

#### 반복문이 중첩되어 있을 경우, break문은 가장 가까운 반복문만 종료하고 바깥쪽 반복문은 종료시키지 않음

- 바깥쪽 반복문까지 종료시키려면 바깥쪽 반복문에 이름(라벨)을 붙이고,
  -  `break 이름;` 을 해야함

## 2.5 continue문

### 반복문 블록 내부에서 continue문이 실행되면 반복문의 조건식으로 이동

#### 반복문을 종료하지 않고 계속 반복을 수행한다는 점이 break문과 다름

### continue문 예제

```java
public class ContinueExample{
    public static void main(String[] args) {
        for (int i=1; i<=10; i++) {
            if (i % 2 ==0){
                continue;
            }
        }
    }
}
```



#### 출처

1. 이것이 자바다 - 신용권의 Java 프로그래밍 정복 [한빛미디어]