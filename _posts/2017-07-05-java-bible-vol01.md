---
layout: post
title: 자바의 정석 Volume 01(Incomplete)
---

Chapter 01. 자바를 시작하기전에
==============================

### 자바언어의 특징
1. 운영체제에 독립적이다
    - 일종의 에뮬레이터인 자바가상머신(JVM)을 통해서 가능하게 됨.
    - 자바 응용프로그램을 (OS나 HW가 아닌) JVM하고만 통신하고 JVM이 자바 응용프로그램으로부터 전달 받은 명령을 해당 OS가 이해할 수 있도록 변환하여 전달한다.
    - 자바로 작성된 프로그램은 OS에 독립적이지만 JVM은 OS에 종속적이어서 썬에서는 여러 OS에 설치할 수 있는 서로 다른 버전의 JVM을 제공하고 있다.
    - 그래서 자바로 작성된 프로그램은 OS/HW에 관계없이 실행 가능하며 이것을 **한번 작성하면, 어디서나 실행된다.(Write once, run anywhere)**라고 표현하기도 함.

2. 객체지향 언어
    - 객체지향개념의 특징인 상속, 캡슐화, 다형성이 잘 적용된 순수한 객체지향언어라는 평가를 받고 있음.

3. 비교적 배우기 쉽다.

4. Garbage Collection(자동 메모리 관리)
    - 자바로 작성된 프로그램이 실행되면, 가비지컬렉터가 자동적으로 메모리를 관리해주기 때문에 프로그래머는 메모리를 따로 관리하지 않아도 된다.
    - 이는 다소 비효율적인 면도 있지만, 프로그래밍에 집중할 수 있도록 도와준다.

5. 네트워크와 분산처리를 지원
    - 다양한 네트워크 프로그래밍 라이브러리를 통해서 비교적 짧은 시간에 네트워크 관련 프로그램을 쉽게 개발 할 수 있도록 지원

6. 멀티쓰레드 지원
    - (일반적으로OS에 따라서 멀티스레드의 지원은 구현, 처리 방법이 다르지만) 자바에서 개발되는 멀티스레드 프로그램은 시스템과는 관계없이 구현가능하며 관련 라이브러리가 제공되어 구현이 쉽다. 
    - 그리고 여러 쓰레드에 대한 스케줄링을 자바 인터프리터가 담당하게 된다.

7. 동적 로딩(Dynamic Loading)을 지원
    - 실행 시에 모든 클래스가 로딩되지 않고 필요한 시점에 클래스를 로딩하여 사용할 수 있다는 장점
    - 일부 클래스가 변경되어도 전체 애플리케이션을 다시 컴파일하지 않아도 되며, 애플리케이션의 변경사항이 발생해도 비교적 적은 작업만으로도 처리할 수 있는 유연한 애플리케이션 작성 가능

> 자바의 단점으로는 속도문제가 가장 대표적인데, Byte code(자바로 작성된 파일을 컴파일하면 생기는 파일; .class)를 하드웨어의 기계어로 바로 변환해주는 JIT 컴파일러와 Hotspot과 같은 신기술 도입으로 JVM의 기능이 향상됨으로써 속도문제가 상당히 개선됨


### JVM(Java Virtual Machine)
* Java Application

|Java 애플리케이션|
|---|
|**JVM**|
|OS|
|Computer(HW)|

* 일반 Application

|일반 애플리케이션|
|---|
|OS|
|Computer(HW)|


일반 애플리케이션의 코드는 OS만 거치고 하드웨어로 전달되는데 비해 Java 애플리케이션은 JVM을 한 번 더 거치기 대문에, 그리고 하드웨어에 맞게 완전히 컴파일된 상태가 아니고 실행 시에 해석(interpret)되기 때문에 속도가 느리다는 단점을 가지고 있다. 그러나 Byte code를 하드웨어의 기계어로 바로 변환해주는 JIT 컴파일러와 Hotspot과 같은 신기술 도입으로 JVM의 기능이 향상됨으로써 속도문제가 상당히 개선됨


### 자바 프로그램 작성
`Hello.java` 클래스를 작성한다음에 `javac(자바 컴파일러)`를 통해서 `소스파일(Hello.java)`로 부터 `클래스파일(Hello.class)`를 생성해야 한다. 그다음에 `클래스파일`을 `자바 인터프리터(java)`로 실행한다.


`public static void main(String[] args)`는 main 메서드의 선언부인데, 프로그램을 실행할 때 `java(자바 인터프리터)`에 의해 호출될 수 있도록 미리 약속된 부분이므로 항상 똑같이 적어주어야 함.


하나의 Java 애플리케이션에는 main 메서드를 포함한 클래스가 반드시 하나는 있어야 한다.

소스 파일의 이름은 public class의 이름과 일치해야 한다.

소스파일 내에 public class가없다면, 소스파일의 이름은 소스파일 내의 어떤 클래스의 이름으로 해도 상관없다.


### 자바 프로그램의 실행과정
Java 애플리케이션을 실행시켰을 때
1. 프로그램의 실행에 필요한 클래스(\*.class파일)를 로드한다.
2. 클래스파일을 검사한다.(파일형식, 악성코드 체크)
3. 지정된 클래스(Hello)에서 main(String[] args)를 호출한다.



Chapter 02. 변수
==============================

### 변수의 명명규칙
1. 대소문자가 구분되며 길이에 제한이 없다.
2. 예약어를 사용해서는 안 된다.
3. 숫자로 시작해서는 안 된다.
4. 특수문자는 '_'와 '$'만을 허용한다.

### 그외 권장 규칙
1. 클래스 이름의 첫 글자는 항상 대문자로 한다.
    - 변수와 메서드의 이름의 첫 글자는 항상 소문자로 한다.
2. 여러 단어로 이루어진 이름은 단어의 첫 글자를 대문자로 한다.
     - lastIndexOf, StringBuffer
3. 상수의 이름은 모두 대문자로 한다. 여러 단어로 이루어진 경우 '_'로 구분한다.
    - PI, MAX_NUMBER

변수의 이름은 짧을수록 좋지만, 약간 길더라도 용도를 알기 쉽게 `의미있는 이름`으로 하는 것이 바람직하다.


### 변수의 타입
* 기본형(primitive type)
    - boolean, char, (byte, short, int, long), (float, double); 계산을 위한 실제 값을 저장.
* 참조형(reference type)
    - 객체의 주소를 저장. 8개의 기본형을 제외한 나머지 타입


상수: final 키워드 사용
```java
final int MAX_SPEED = 100;
```

### 정수형의 표현형식과 범위
정수형의 저장방식은 타입의 크기가 n이라고 할때 S(부호비트) + n-1비트(값의 범위 표현)

### 정수형의 선택기준
JVM의 피연산자 스택(operand stack)이 피연산자를 4 byte 단위로 저장하기 때문에 크기가 4byte보다 작은 자료형(byte, short)의 값을 계산할 때는 4 byte로 변환하여 연산한다. 그래서 오히려 int를 사용하는 것이 더 효율적이다.

결론적으로는 정수형 변수를 선언할 때는 int타입으로 하고, int의 범위(+-20억)을 넘어서는 수를 다뤄야할 때는 long을 사용하면 된다.(long타입의 범위를 벗어나는 값을 다룰 때는, 실수형 타입이나 BigInteger클래스를 사용하면 됨)


`부동소수점에 대해서는 추후에 공부해보자`

* 자동 형변환의 규칙: 기존의 값을 최대한 보존할 수 있는 타입으로 자동 형변환한다.


Chapter 03. 연산자
==============================


Chapter 04. 조건문과 반복문
==============================

### 향상된 for문(enhanced for statement)
- JDK 1.5부터 배열과 컬렉션에 저장된 요소에 접근할 때 기존보다 편리한 방법으로 처리할 수 있도록 for문의 새로운 문법이 추가됨
```
for(타입 변수명 : 배열 또는 컬렉션){
    // 반복할 문장
}
```
```java
int[] arr = {10, 20, 30, 40, 50};

for(int tmp : arr) {
    System.out.println(tmp);
}
```


Chapter 05. 배열
==============================
### 배열의 선언 방식
1. 타입[] 변수이름;
    - int[] score;
2. 타입 변수이름[];
    - int score[];

### 배열의 생성
* 변수이름 = new 타입[길이];
```java
int[] score;
score = new int[5];
```

자바에서 JVM은 모든 배열의 길이를 별도로 관리하며, `배열이름.length`(상수임; 변경할 수 없음)를 통해서 배열의 길이에 대한 정보를 얻을 수 있다.

### 배열의 길이를 변경하는 방법
1. 더 큰 배열을 새로 생성
2. 기존 배열의 내용을 새로운 배열에 복사


배열의 복사는 for문보다 System.arraycopy()를 사용하는 것이 효율적


### String 클래스의 주요 메서드
- char charAt(int index)
- int length()
- String substring(int from, int to)
- boolean equals(String str) : 문자열의 내용이 같은지 다른지 확인하는데 사용
    * 기본형 변수의 값을 비교하는 경우 `==`연산자를 사용하지만, 문자열의 내용을 비교할 때는 equals()르 사용해야함.(대소문자 구별)
- char[] toCharArray()


Chapter 06. 객체지향 프로그래밍 1
==============================

* 객체지향이론의 기본 개념은 `실제 세계는 사물(객체)로 이루어져 있으며, 발생하는 모든 사건들은 사물간의 상호작용이다.`라는 것
* 객체지향이론은 상속, 캡슐화, 추상화 개념을 중심으로 점차 구체적으로 발전

### 갹체지향언어의 주요특성
1. 코드 재사용성
2. 코드의 관리 용이
    - 코드간의 관계를 이용해서 적은 노력으로 쉽게 코드 변경
3. 신뢰성이 높은 프로그래밍이 가능
    - 제어자와 메서드를 이용해서 데이터를 보호하고 올바른 값을 유지하도록 하고, 코드의 중복을 제거해서 코드 불일치로 인한 오동작 방지


### JVM의 메모리 구조
- 응용 프로그램이 실행되면 JVM은 시스템으로부터 프로그램을 수행하는데 필요한 메모리를 할당받고 JVM은 이 메모리를 용도에 따라 여러 영역으로 나누어 관리한다.

|Method Area(class data, class variable)|
|---|
|Call Stack(local variable)|
|Heap(instance, instance variable)|


1. method area
    - 프로그램 실행 중 어떤 클래스가 사용되면, JVM은 해당 클래스의 `클래스파일(.class)`을 읽어서 분석하여 클래스에 대한 정보(클래스 데이터)를 이곳에 저장한다. 이때 그 클래스의 클래스변수도 이 영역에 함께 생성된다.
2. heap
    - 인스턴스가 생성되는 공간. 프로그램 실행 중 생성되는 인스턴스는 모두 이곳에 생성된다. 즉, 인스턴스 변수가 생성되는 공간
3. call stack or execution stack
    - call stack은 메서드의 작업에 필요한 메모리 공간을 제공한다. 
    - 메서드가 호출되면 호출스택에 호출된 메서드를 위한 메모리가 할당되며, 이 메모리는 작업을 수행하는 동안 지역변수(매개변수 포함)들과 연산의 중간결과 등을 저장하는데 사용.
    - 메서드가 작업을 마치면 할당되었던 메모리공간은 반환되어 비워진다.


### call stack의 특징
- 메서드가 호출되면 수행에 필요한 만큼의 메모리를 스택에 할당받는다.
- 메서드가 수행을 마치고나면 사용했던 메모리를 반환하고 스택에서 제거된다.
- 호출스택의 제일 위에 있는 메서드가 현재 실행 중인 메서드이다.
- 아래에 있는 메서드가 바로 위의 메서드를 호출한 메서드이다.

### 기본형 매개변수/참조형 매개변수
- 기본형 매개변수: 변수의 값을 읽기만 할 수 있다.(read only)
- 참조형 매개변수: 변수의 값을 읽고 변경할 수 있다.(read and write)


### 클래스 메서드(static method)/인스턴스 메서드
* 인스턴스 메서드는 `인스턴스 변수와 관련된 작업`을 하는, 즉 메서드의 작업을 수행하는데 인스턴스 변수를 필요로 하는 메서드이다.
* 인스턴스와 관계없는(인스턴스 변수나 인스턴스 메서드를 사용하지 않는) 메서드를 클래스 메서드(static 메서드)로 정의(일반적으로)
1. 클래스 설계시, 멤버변수 중 모든 인스턴스에 공통적으로사용해야하는 것에 static을 붙인다.(모든 인스턴스에서 같은 값이 유지되어야 하는 변수에)
2. 클래스 변수(static 변수)는 인스턴스를 생성하지 않아도 사용할 수 있다.
    - 클래스가 메모리에 올라갈 때 이미 자동적으로 생성되기 때문
3. 클래스 메서드(static method)는 인스턴스 변수를 사용할 수 없다.
4. 메서드 내에서 인스턴스 변수를 사용하지 않는다면, static을 붙이는 것을 고려한다.
    - 클래스의 멤버변수 중 모든 인스턴스에 공통된 값을 유지해야하는 것이 있는지 살펴보고 있으면 static을 붙여준다.
    - 작성한 메서드 중에서 인스턴스 변수나 인스턴스 메서드를 사용하지 않는 메서드에 static을 붙일 것을 고려한다.


클래스내에서 인스턴스 변수, 클래스 변수를 멤버변수라고 함.

### 오버로딩
한 클래스 내에 같은 이름의 메서드를 여러 개 정의하는 것  
* 오버로딩의 조건
    1. 메서드 이름이 같아야 한다.
    2. 매개변수의 개수 또는 타입이 달라야 한다.
리턴 타입은 오버로딩을 구현하는데 아무런 영향을 주지 못함  

* 오버로딩의 장점
    1. 같은 기능을 하는 다른 매개변수의 메서드들의 이름을 하나로 정의하여 쉽게 예측 가능
    2. 메서드의 이름을 절약 가능


### 가변인자(varargs)와 오버로딩
가변 인자는 `타입... 변수명`과 같은 형식으로 선언하며, printf()가 대표적인 예  
```
public PrintStream printf(String format, Object... args) { ... } // O

public PrintStream printf(Object... args, String format) { ... } // X
```

같은 매개변수가 여러개 나열될 때 가변인자를 사용해서 하나로 대체할 수 있음.

```
String concatenate(String s1, String s2) { ... }
String concatenate(String s1, String s2, String s3) { ... }
String concatenate(String s1, String s2, String s3, String s4) { ... }

String concatenate(String... str) { ... } 
```

가변인자는 내부적으로 배열을 새로 생성하기에 비효율성이 좀 있다.

* 가변인자와 매개변수의 타입을 배열로 하는 것의 차이
    - 배열로 했을시 반드시 인자를 지정해 줘야하기 때문에 아래와 같이 인자를 생략할 수 없음.

```
String concatenate(String[] str) { ... }
String result = concatenate(new String[0]); // O
String result = concatenate(null); // O
String result = concatenate(); // X
```

### 가변인자 사용시 주의사항
1. 함수 호출을 할 때 가변인자 부분에 배열 선언문을 넣으면 안됨
```
static String concatenate(String delim, String... args){ ... }
System.out.println(concatenate("-", {"100", "200", "300"})); // X
```

2. 가변인자를 선언한 메서드를 오버로딩하면 메서드가구분되지 않아서 에러가 발생할 수 있음. 주의 해야함. 않하는 것이 좋을 듯.
```
/*
Compile Error
*/
static String conatenate(String delim, String... args){ ... }
static String conatenate(String... args){ ... }
```


### 생성자(Constructor)
* 생성자의 조건
    - 생성자의 이름은 클래스의 이름과 같아야 한다.
    - 생성자를 리턴 값이 없다.

* 생성자는 오버로딩이 가능하므로 하나의 클래스에 여러개의 생성자가 존재할 수 있다.
* 인스턴스의 생성은 연산자 new가 하는 것이고, 생성자가 하는 것이 아니다.
    - 생성자는 단순히 인스턴스변수들의 초기화에 사용되는 조금 특별한 메서드일 뿐이다.

* 클래스의 인스턴스가 생성되는 과정
```java
Card c = new Card();
```
1. 연산자 `new`에 의해서 메모리(heap)에 Card 클래스의 `인스턴스가 생성`된다.
2. `생성자` Card()가 호출되어 수행된다.
3. 연산자 `new의 결과`로, 생성된 Card `인스턴스의 주소가 반환되어 참조변수 c에 저장`된다.


모든 클래스에는 반드시 하나 이상의 생성자가 정의되어 있어야 한다.
##### Default Constructor
`클래스에 생성자가 하나도 정의되지 않은 경우` 컴파일러는 자동적으로 아래와 같은 내용의 `기본 생성자를 추가하여 컴파일`한다.
```
ClassName() { }
```
클래스의 `접근제어자(Access Modifier)`가 `public`인 경우에는 기본생성자로 `public 클래스이름() {}`이 추가된다.

##### 생성자에서 다른 생성자 호춣하기 - this(), this
* 조건
    1. 생성자의 이름으로 클래스이름 대신 `this`를 사용한다. 
    2. 한 생성자에서 다른 생성자를 호출할 때는 `반드시 첫 줄에서만` 호출이 가능하다.
        - 이유: 생성자 내에서 초기화 작업도중에 다른 생성자를 호출하게 되면, 호출된 다른 생성자내에서도 멤버변수들의 값을 초기화 할 것이므로 다른 생성자를 호출하기 이전의 초기화 작업이 무의미해질 수 있기 때문

* 에러 발생 예
```java
Car(String color){
    door = 5;  // First line
    Car(color, "auto", 4); // error1: call constructor at second line; error2: didnt't called as this
}
```

* this: 인스턴스 자신을 가리키는 참조변수, 인스턴스의 주소가 저장되어 있다. 모든 인스턴스 메서드에 지역변수로 숨겨진 채 존재
* this(), this(매개변수): 생성자, 같은 클래스의 다른 생성자를 호출할 때 사용


##### 생성자를 이용한 인스턴스의 복사
* Bad Case
```java
Car (Car c){
    color = c.color;
    gearType = c.gearType;
    door = c.door;
}
```

* Better Case(기존의 코드를 활용할 수 없는지 고민하여야 함)
```java
Car (Car c){
    // Car(String color, String gearType, int door)
    this(c.color, c.gearType, c.door);
}
```

##### 인스턴스를 생성할 때는 다음의 2가지 사항을 결정해야만 함.
1. 클래스 - 어떤 클래스의 인스턴스를 생성할 것인가?
2. 생성자 - 선택한 클래스의 어떤 생성자로 인스턴스를 생성할 것인가?



##### 변수의 초기화
멤버변수는 초기화를 하지 않아도 자동적으로 변수의 자료형에 맞는 기본값으로 초기화가 됨.   
하지만 지역변수는 사용하기 전에 반드시 초기화해야 한다.



##### 멤버변수의 초기화 방법
1. explicit initialization
2. constructor
3. initialization block
    - 인스턴스 초기화 블록: 인스턴스변수를 초기화하는데 사용.
    - 클래스 초기화 블록: 클래스변수를 초기화하는데 사용

* explicit initialization: 변수를 선언과 동시에 초기화하는 것(가장 기본적이면서 간단함. 가장 우선적으로 고려되어야 함)

복잡한 초기화 작업이 필요할 때 initialization block이나 생성자를 사용

* initialization block
    - 초기화 블록을 작성하려면, 인스턴스 초기화 블록은 단순히 클래스 내에 블럭{}을 만들고 그안에 코드를 작성하기만 하면 된다.
    - 클래스 초기화 블럭은 인스턴스 초기화 블럭앞에 단순히 static을 덧붙이기만 하면 된다.
    - 초기화 블럭 내에는 메서드 내에서와 같이 조건문, 반복문, 예외처리구문 등을 자유롭게 사용할 수 있으므로, 초기화 작업이 복잡하여 명시적 초기화만으로는부족한 경우 초기화 블럭을 사용한다.

```java
class InitBlock {
    static {/* 클래스 초기화 블럭 */}
    
    { /* 인스턴스 초기화 블럭 */ }
    // ...
}
```
* 클래스 초기화 블럭은 클래스가 메모리에 처음 로딩될 때 한번만 수행되며, 인스턴스 초기화 블럭은 생성자와 같이 인스턴스를 생성할 때 마다 수행
    - 클래스가 처음 로딩될 때 클래스변수들이 자동적으로 메모리에 만들어지고, 곧바로 클래스 초기화블럭이 클래스변수들을 초기화하게 되는 것
* 생성자보다 인스턴스 초기화 블럭이 먼저 수행됨.


인스턴스 변수의 초기화는 주로 생성자를 사용하고 인스턴스 초기화 블럭은 모든 생성자에서 공통으로 수행돼야 하는 코드를 넣는데 사용한다.
* Before
```java
Car () {
    count++; //overlapped
    serialNo = count; //overlapped
    color = "White";
    gearType = "Auto";
}

Car (String color, String gearType) {
    count++; //overlapped
    serialNo = count; //overlapped
    this.color = color;
    this.gearType = gearType;
}
```
이 경우 클래스의 모든 생성자에서 공통으로 수행되어야 하는 문장들이 있을 때, 이 문장들을 각 생성자마다 써주기 보다는 아래와 같이 인스턴스 블럭에 넣어주면 코드가 보다 간결해짐.

* After
```java

// instance initialization block
{
    count++; //overlapped
    serialNo = count; //overlapped
}
Car () {
    color = "White";
    gearType = "Auto";
}

Car (String color, String gearType) {
    this.color = color;
    this.gearType = gearType;
}
```

* 이렇게 코드의 중복을 제거하는 것은 코드의 신뢰성을 높이고, 오류의 발생 가능성을 줄여 준다는 장점이 있음.
    - 즉, 재사용성을 높이고 중복을 제거하는 것, 이것이 바로 객체지향프로그래밍이 추구하는 궁극적인 목표.


##### 멤버변수의 초기화 시기와 순서
* 클래스변수의 초기화시점: `클래스가 처음 로딩될 때` `단 한번` 초기화 된다.
* 인스턴스변수의 초기화시점: `인스턴스가 생성될 때마다` 각 인스턴스별로 초기화가 이루어진다.


* 클래스변수의 초기화순서: `기본값` -> 명시적초기화 -> 클래스 초기화 블록
* 인스턴스변수의 초기화순서: `기본값` -> 명시적초기화 -> 클래스 초기화 블록 -> 생성자


Chapter 07. 객체지향 프로그래밍 2
==============================

### 상속
상속이란, 기존의 클래스를 재사용하여 새로운 클래스를 작성하는 것.
```java
class Child extends Parent {
   // ...
}
```
여기서 Child와 Parent는 서로 상속 관계에 있다고 하며, 상속해주는 클래스를 `조상 클래스`, 상속 받는 클래스를 `자손 클래스`라고 한다.
* 조상 클래스: 부모(parent) 클래스, 상위(super) 클래스, 기반(base) 클래스
* 자손 클래스: 자식(child) 클래스, 하위(sub) 클래스, 파생된(derived) 클래스


상속을 받는다는 것은 조상 클래스를 확장(extend)한다는 의미로 해석할 수 있다.
- 생성자와 초기화 블럭은 상속되지 않는다. 멤버만 상속된다.
- 자손 클래스의 멤버 개수는 조상 클래스보다 항상 같거나 많다.


같은 내용의 코드를 하나 이상의 클래스에 중복적으로 추가해야하는 경우에는 상속관계를 이용해서 코드의 중복을 최소화해야한다.


##### 클래스간의 관계 - 포함관계
* 상속이외에 클래스 간에 `포함(Composite)`관계를 맺어 주는 것.
    - 클래스의 멤버변수로 다른 클래스 타입의 참조변수를 선언하는 것을 뜻함.

```java
class Point { ... }

class Circle {
    Point c = new Point(); // Composite
    int r;
```

##### 클래스간의 관계 설정하기
* 상속관계: 무엇은 뭐뭐이다. (is-a): Circle is a Point.
* 포함관계: 무엇은 뭐뭐를 가지고 있다. (has-a) Circle has a Point.



다중상속을 허용하면 여러 클래스로부터 상속받을 수 있기 때문에 복합적인 기능을 가진 클래스를 쉽게 작성할 수 있다는 장점이 있지만, 클래스간의 관계가 매우 복잡해진다는 것과 서로 다른 클래스로부터 상속받은 멤버간의 이름이 같은 경우 구별하 수 있는 방법이 없다는 단점을 가지고 있다.



##### Object 클래스 - 모든 클래스의 조상
다른 클래스로부터 상속 받지 않는 모든 클래스들은 자동적으로 Object 클래스로부터 상속받게 함으로써 이것을 가능하게 된다.


### 오버라이딩(overriding)
조상 클래스로부터 상속받은 메서드의 내용을 변경하는 것을 오버라이딩이라고 한다.

```java
class Point {
    int x;
    int y;

    String getLocation() {
        return "x :" + x + ", y:" + y;
    }
}

class Point3D extends Point {
    int z;

    String getLocation() {
        return "x :" + x + ", y:" + y + ", z:" + z;
    }
}
```
* 오버라이딩의 조건 (자손 클래스에서 오버라이딩하는 메서드는 조상 클래스의 메서드와 ~)
    1. 이름이 같아야 한다.
    2. 매개변수가 같아야 한다.
    3. 리턴타입이 같아야 한다.

한마디로 `선언부가 일치해야 한다는 것`. 다만 접근 제어자와 예외(exception)는 제한된 조건 하에서만 다르게 변경할 수 있다.  
1. 접근 제어자는 조상 클래스의 메서드보다 좁은 범위로 변경 할 수 없다.
    - 조상 클래스의 정의된 메서드의 접근 제어자가 `protected`라면 이를 오버라이딩하는 자손 클래스의 메서드는 접근 제어자가 `protected`나 `public`이어야 한다. 대부분의 경우 같은 범위의 접근 제어자를 사용한다.
        * 점근 제어자의 범위: public -> protected -> (default) -> private
2. 조상 클래스의 메서드보다 많은 수의 예외를 선언할 수 없다.
```java
Class Parent {
    void parentMethod() throws IOException, SQLException {
        // ...
    }
}

Class Child extends Parent {
    void parentMethod() throws IOException {
        // ...
    }
}
```

* 조상 클래스의 메서드를 자손 클래스에서 오버라이딩할 때
    1. 접근 제어자를 조상 클래스의 메서드보다 좁은 범위로 변경할 수 없다.
    2. 예외는 조상 클래스의 메서드보다 많이 선언할 수 없다.
    3. 인스턴스 메서드를 static 메서드로 또는 그 반대로 변경할 수 없다.
        - 별도로, 조상 클래스에 정의된 static 메소드를 자손 클래스에서 똑같은 이름의 static메서드로 정의할 수 있다.
            * 이는 오버라이딩이 아니라 각 클래스에서 별개의 static메서드를 정의할 것뿐이다.


##### 오버로딩 vs 오버라이딩
* overloading: 기존에 없는 새로운 메서드를 정의하는 것(new)
* overriding: 상속받은 메서드의 내용을 변경하는 것(change, modify)


##### super
`super`는 자손 클래스에서 조상 클래스로부터 상속받은 멤버를 참조하는데 사용되는 참조변수  
상속받은 멤버도 자손 클래스 자신의 멤버이므로 super대신 `this`를 사용할 수 있다. 그래도 조상 클래스의 멤버와 자손클래스의 멤버가 중복정의되어 서로 구별해야하는 경우에만 super를 사용하는 것이 좋다.  
조상의 멤버와 자신의 멤버를 구별하는데 사용된다는 점을 제외하고는 super와 this는 근본적으로 같다.  
모든 인스턴스메서드에는 자신이 속한 인스턴스의 주소가 지역변수로 저장되는데, 이것이 참조변수인 this와 super의 값이 된다.  
static메서드(클래스 메서드)는 인스턴스와 관련이 없다. 그래서 this와 마찬가지로 super 역시 static 메서드에서는 사용할 수 없고, 인스턴스메서드에서만 사용할 수 있다.

그런데 조상 클래스에 선언된 멤버변수와 같은 이름의 멤버변수를 자손 클래스에서 중복해서 정의하게 되면 참조변수 super를 써서 구별할 수 있다.
```java
class SuperTest2 {
    public static void main(String[] args) {
        Child c = new Child();
        c.method();
    }
}

class Parent {
    int x = 10;
}

class Child extends Parent {
    int x = 20;

    void method(){
        System.out.println("x=" + x); // 20
        System.out.println("this.x=" + this.x); // 20
        System.out.println("super.x=" + super.x); // 10
    }
}
```

위와 같은 변수 뿐만 아니라 메서드 역시 super를 써서 호출할 수 있다. 특히 조상 클래스의 메서드를 자손 클래스에서 오버라이딩한 경우에 super를 사용한다.
```java
class Point {
    int x;
    int y;

    String getLocation() {
        return "x:" + x + ", y :" + y;
    }
}

class Point3D extends Point {
    int z;
    String getLocation() {
        // return "x :" + x + ", y :" + y + ", z:" + z;
        return super.getLocation() + ", z:" + z;
    }
}
```

##### super() - 조상 클래스의 생성자
자손 클래스의 인스턴스를 생성하면, 자손의 멤버와 조상의 멤버가 모두 합쳐진 하나의 인스턴스가 생성됨.  
그래서 자손의 인스턴스가 조상의 메멉를 모두 사용할 수 있다. 이 때 조상의 멤버의 초기화 작업이 수행되어야 하기 때문에 자손의 생성자에서 조상의 생성자가 호출되어야 한다.  
생성자의 첫 줄에서 조상의 생성자를 호출해야하는 이유는 자손의 멤버가 조상의 멤버를 사용할 수도 있으므로 조상의 멤버들이 먼저 초기화되어 있어야 하기 때문이다.  
그래서 Object 클래스를 제외한 모든 클래스의 생성자는 첫 줄에 반드시 자신의 다른 생성자 또는 조상의 생성자를 호출해야 한다. 그렇지 않으면 컴파일러는 생성자의 첫 줄에 `super();`를 자동적으로 추가할 것이다.


인스턴스를 생성할 때는 클래스를 선택하는 것만큼 생성자를 선택하는 것도 중요한 일이다.
1. 클래스 - 어떤 클래스의 인스턴스를 생성할 것인가?
2. 생성자 - 선택한 클래스의 어떤 생성자를 이용해서 인스턴스를 생성할 것인가?


### package와 import

##### package
클래스의 묶음. 클래스 또는 인터페이스를 포함시킬 수 있으며, 서로 관련된 클래스들끼리 그룹 단윌 묶어 놓음으로써 클래스를 효율적으로 관리할 수 있음.  
같은 이름의 클래스 일지라도 서로 다른 패키지에 존재하는 것이 가능하므로, 자신만의 패키지 체계를 유지함으로써 다른 개발자가 개발한 클래스 라이브러리의 클래스와 이름이 충돌하는 것을 피할 수 있다.  

* 클래스의 실제 이름은 패키지명을 포함한 것임.
    - eg. String 클래스의 패키지명을 포함한 이름은 `java.lang.String`
        * java.lang 패키지에 속한 String 클래스라는 의미다.

__클래스가 물리적으로 하나의 클래스파일(.class)인 것과 같이 패키지는 물리적으로하나의 디렉토리이다.__ 그래서 어떤 패키지에 속한 클래스는 해당 디렉토리에 존재하는 클래스파일(.class)이어야 한다.


* 패키지는 다른 패키지를 포함할 수 있으며 점`.`으로 구분한다.(eg. java.lang 패키지에서 lang패키지는 java패키지의 하위 패키지)
    - 하나의 소스파일에는 첫 번째 문장으로 단 한 번의 패키지 선언만을 허용
    - 모든 클래스는 반드시 하나의 패키지에 속해야 함
    - 패키지는 점`.`을 구분자로 하여 계층구조를 구성할 수 있음
    - 패키지는 물리적으로 클래스 파일(.class)을 포함하는 하나의 디렉토리임

* 패키지의 선언
```java
package 패키지명;
```
패키지명은 대소문자를 모두 허용하지만, 클래스명과 쉽게 구분하기 위해서 소문자로 하는 것을 원칙으로 하고 있다.


모든 클래스는 반드시 하나의 패키지에 포함되어야 한다. 그런데 여태껏 소스파일을 작성할 때 패키지를 선언하지 않고도 아무런 문제가 없었던 이유는 자바에서 기본적으로 제공하는 `이름없는 패키지(unnamed package)`때문이다.  
소스파일에 자신이 속할 패키지를 지정하지 않은 클래스는 자동적으로 `unnamed package`에 속하게 된다. 결국 패키지를 지정하지 않는 모든 클래스들은 같은 패키지에 속하는 셈이 된다.


### import 문
클래스의 코드를 작성하기 전에 import문으로 사용하고자 하는 클래스의 패키지를 미리 명시해주면 소스코드에 사용되는 클래스이름에서 패키지명은 생략할 수 있다.  
import 문의 역할은 컴파일러에게 소스파일에 사용된 클래스의 패키지에 대한 정보를 제공하 것.  



### 제어자 












