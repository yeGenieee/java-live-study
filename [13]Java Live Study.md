# [13주차 과제] I/O

### Stream / Buffer / Channel 기반의 I/O

### InputStream과 OutputStream

### Byte와 Character 스트림

### 표준 스트림 (System.in, System.out, System.err)

### 파일 읽고 쓰기

------

# 0. I/O 패키지 소개

  프로그램에서는 데이터를 외부에서 읽고 다시 외부로 출력하는 작업이 빈번히 일어난다. 데이터는 사용자로부터 키보드를 통해 입력될 수도 있고, 파일 또는 네트워크로부터 입력될 수도 있다. 

  자바에서 데이터는 Stream을 통해 입출력된다. Stream은 단방향의 특징을 가진다.

### 입력 스트림과 출력 스트림

- 프로그램이 출발지냐 또는 도착지냐에 따라 스트림의 종류가 결정된다
  - 프로그램을 기준으로 데이터를 입력받는다 (데이터가 들어온다) = InputStream (입력 스트림)
  - 프로그램을 기준으로 데이터를 보낸다 (데이터가 나간다) = OutputStream (출력 스트림)

- 프로그램이 네트워크상의 다른 프로그램과 데이터 교환을 하려면, 양쪽 모두 입력 스트림과 출력 스트림이 따로 필요하다
  - Stream은 단방향의 특징을 가지므로, 하나의 스트림으로 입력과 출력을 모두 할 수 없기 때문이다

### Java의 I/O API (java.io 패키지에서 제공)

| java.io 패키지의 주요 클래스                                 | 설명                                                  |
| ------------------------------------------------------------ | ----------------------------------------------------- |
| File                                                         | 파일 시스템의 파일 정보를 얻기 위한 클래스            |
| Console                                                      | 콘솔로부터 문자를 입출력하기 위한 클래스              |
| InputStream / OutputStream                                   | 바이트 단위 입출력을 위한 최상위 입출력 스트림 클래스 |
| FileInputStream / FileOutputStream<br />DataInputStream / DataOutputStream<br />ObjectInputStream / ObjectOutputStream<br />PrintStream<br />BufferedInputStream / BufferedOutputStream | 바이트 단위 입출력을 위한 하위 스트림 클래스          |
| Reader / Writer                                              | 문자 단위 입출력을 위한 최상위 입출력 스트림 클래스   |
| FileReader / FileWriter<br />InputStreamReader / OutputStreamReader<br />PrintWriter<br />BufferedReader / BufferedWriter | 문자 단위 입출력을 위한 하위 스트림 클래스            |

- 스트림 클래스는 크게 두 종류로 구성된다
  1. Byte 기반 스트림
  2. Character 기반 스트림

자세한 건 뒤에서 알아보자.

