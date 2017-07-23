---
layout: post
title: 테스트, 그리고 JUnit에 대한 간단한 요약
---

JUnit and Test
===============

### 테스트 픽스처
테스트를 반복적으로 수행할 수 있게 도와주고 매번 동일한 결과를 얻을 수 있게 도와주는 `기반이되는 상태나 환경`. `일관된 테스트 실행환경`이라고도 하며 때로는 테스트 컨텍스트라 부르기도 한다.  
그 예로는
* 테스트 케이스에서 사용할 객체의 인스턴스를 만드는 것
* 데이터베이스와 연동할 수 있는 참조를 선언하는 것
* 파일이나 네트워크 등의 자원을 만들어 지정하는 것.
* 그 작업의 결과로 만들어진 대상을 통칭.



### JUnit 기본 단정문
* `assertEqual`: 두 값이 같은지 비교하는 단정문
* `assertSame`:  두 객체가 정말 동일한 객체인지 주소값으로 비교하는 단정문.
* `assertTrue`: 예상 값의 참/거짓을 판별하는 단정문
* `assertNull`: 대상 값이 null이면 참이 되는 단정문
* `fail([message])`: 호출 즉시 테스트 케이스를 실패로 판정하는 단정문


# JUnit 4

### JUnit 4의 특징
1. Java 5 애노테이션 지원
2. test라는 글자로 method 이름을 시작해야 한다는 제약 해소
    - Test 메소드는 `@Test`를 붙인다.
3. 좀더 유연한 픽스처
    - @BeforeClass, @AfterClass, @Before, @After
4. 예외 테스트
    - @Test(expected=NumberFormatException.class)
5. 시간 제한 테스트
    - @Test(timeout=1000)
6. 테스트 무시
    - @Ignore("this method isn't working yet")
7. 배열 지원
    - assertArrayEquals([message], expected, acutal);
8. @RunWith(클래스이름.class)
    - JUnit Test 클래스를 실행하기 위한 러너(Runner)를 명시적으로 지정
    - @RunWith는 junit.runner.Runner를 구현한 외부 클래스를 인자로 갖는다.
9. @SuiteClasses(Class[])
    - 보통 여러 개의 테스트 클래스를 수행하기 위해 쓰임
    - @RunWith를 이용해 Suite.class를 러너로 사용한다.
10. 파라미터를 이용한 테스트
```java
@RunWith(Parameterized.class)
@Parameters
public static Collection data(){
    ...
}
```

### JUnit 4의 테스트 픽스처 메소드 추가 지원
JUnit 3에서는 setUp과 tearDown이라는 두 개의 테스트 픽스쳐 메소드를 제공했는데, JUnit 4에서는 각각을 @Before, @After라는 이름의 애노테이션으로 지원함.  
JUnit 3에서는 테스트 클래스에서 단 한 번만 실행할 수 있게 해주는 방법을 공식적으로 제공하지 않아 다소 불편했는데, JUnit 4에서는 `@BeforeClass`, `@AfterClass` 두 개의 애노테이션을 이용해 하나의 테스트 클래스 내에서 한 번만 실행하는 메소드를 만들 수 있게 됐다.

### @RunWith
JUnit 프레임워크에서 테스트 클래스 내에 존재하는 각각의 테스트 메소드 실행을 담당하고 있는 클래스를 Test Runner라고 함. 이 Test Runner는 테스트 클래스의 구조에 맞게 테스트 메소드들을 실행하고 결과를 표시하는 역할을 수행한다.  

`@RunWith` 애노테이션은 JUnit에 내장된 기본 테스트 러너인 BlockJUnit4ClassRunner 대신에 @RunWith로 지정된 클래스를 이용해 클래스 내의 테스트 메소드들을 수행하도록 지정해주는 애노테이션.(일종의 JUnit 프레임워크의 확장지점.)
사용하는 입장에서는 이렇게 자세히 알 필요는 없지만, 이런 구조를 이용해서 많은 애플리케이션이나 프레임워크가 잣니에게 필요한 테스트 러너를 직접 만들어 자신만의 고유한 기능을 추가해 테스트를 수행하고 있다.
eg. Spring Framework에서 제공하는 SpringJUnit4ClassRunner 같은 클래스는 이 확장 기능을 이용한 대표적 사례.
    - @RunWith(SpringJUnit4ClassRunner.class)를 지정하면 @Repeat, @ProfileValueSourceConfiguration, @IfProfileValue 등 스프링에서 자체적으로 만들어놓은 추가적인 테스트 기능을 이용할 수 있게 된다.

JUnit 내부에서 기본 러너를 확장한 클래스중 SuiteClasses가 있다.

#### @SuiteClasses
`@SuiteClasses`를 이용하면 여러 개의 테스트 클래스를 일괄적으로 수행할 수 있다.
ATest, BTest, CTest 세 개의 테스트 클래스를 순차적으로 실행하고 싶을 때, 

```java
@RunWith(Suite.class)
@SuiteClasses(Atest.class, BTest.class, CTest.class)
public class ABCSuite{
}
```

#### 파라미터화된 테스트(Parameterized Test)
하나의 메소드에 대해 다양한 테스트 값을 한꺼번에 실행시키고자 할 때 사용.(eg. 과세표준 전 구간을 테스트하려 할 때, 각자 연봉이나 세율, 누진공제액이 다 다름.)
테스트 메소드가 많아지거나 구문이 장황해질 수 있는데, 이때 파라미터화된 테스트를 사용하면 테스트 케이스를 효율적으로 작성 가능.

#### Rule
(JUnit 4.7 버전부터 추가된 기능.)  
테스트 클래스내에서 각 테스트 메소드의 동작 방식을 재정의하거나 추가하기 위해 사용하는 기능  
테스트 케이스 수행을 좀 더 세밀하게 조작할 수 있게 됨.

* TemporaryFolder : 테스트 메소드 내에서만 사용 가능한 임시 폴더나 임시 파일을 만들어준다.
* ExternalResource : 외부 자원을 명시적으로 초기화한다.
* ErrorCollector : 테스트 실패에도 테스트를 중단하지 않고 진행할 수 있게 도와준다.
* Verifier : 테스트 케이스와는 별개의 조건을 만들어서 확인할 때 사용한다.
* TestWatchman : 테스트 실행 중간에 사용자가 끼어들 수 있게 도와준다.
* TestName : 테스트 메소드의 이름을 알려준다.
* Timeout : 일관적인 타임아웃을 설정한다.
* ExpectedException : 테스트 케이스 내에서 예외와 예외 메시지를 직접 확인 할 때 사용.



#### Theory
테스트 데이터와 상관없이 `작성 대상 메소드를 항상 유지해야 하는 논리적인 규칙을 표현할 때` 사용한다.(아직ㅇ느 실험적인 성격이 강한 기능..)  




## Hamcrest
jMock이라는 Mock 라이브러리 저자들이 참영해 만들고 있는 Matcher 라이브러리. 테스트 표현식을 작성할 때 좀 더 문맥적으로 자연스럽고 우아한 문장을 만들 수 있게 도와줌.(Matcher는 어떤 값들의 상호 일치 여부나 특정한 규칙 준수 여부 등을 판별하기 위해 만들어진 메소드나 객체를 지칭.)

Hamcrest 라이브러리는 기본적으로 assertEquals 대신에 `assertThat`이라는 구문 사용을 권장.(좀 더 문맥적인 흐름을 만들기 위해.)

```java
assertEquals("YoungJin", customer.getName());

assertThat(customer.getName(), is("YoungJin"));
```
이와 같이 자연스런 문장을 통해서 테스트 비교구문을 만드는 것이 Hamcrest의 사용 목적.

assertThat의 일반적인 사용 방법.
```
assertThat(테스트대상, Matcher구문);
assertThat("메시지", 테스트대상, Matcher구문);
```

Hamcrest의 적용전과 후
```java
assertEquals(100, account.getBalance());
assertThat(account.getBalance(), is(equalTo(10000)));

assertNotNull(resource.newConnection());
assertThat(resource.newConnection(), is(notNullValue()));

assertTrue(account.getBalance() > 0);
assertThat(account.getBalance(), isGreaterThan(0));

assertTrue(user.getLoginName().indexOf("Guest") > -1);
assertThat(user.getLoginName(), containsString("Guest"));
```

여기서의 is, equalTo, greaterThan 등의 메소드가 Matcher 구문에 해당함.


Hamcrest 라이브러리는 기본적으로 core 패키지와 그 외의 확장 패키지로 구성되어 있음.

### 자주 사용되는 대표적인 Matcher들
* 패키지
    - 메소드 : 설명 (클래스)

* Core
    - anything : 어떤 오브젝트가 사용되든 일치한다고 판별 (IsAnything)
    - describedAs : 테스트 실패 시에 보여줄 추가적인 메시지를 만들어주는 메시지 데코레이터 (DescribedAs)
    - equalTo : 두 오브젝트가 동일한지 판별. (IsEqual)
    - is : 내부적으로 equalTo와 동일. 가독성 증진용.(Is)
        - 아래의 세 문장은 의미가 동일함.
            * assertThat(entity, equalTo(expectedEntity));
            * assertThat(entity, is(equalTo(expectedEntity)));
            * assertThat(entity, is(expectedEntity));

* Object
    - hasToString : toString 메소드의 값과 일치 여부를 판별. (HasToString)
    * instanceOf : 동일 인스턴스인지 타입 비교(instance of)  (IsInstanceOf)
    * typeCompatibleWith : 동일하거나 상위 클래스, 인터페이스인지 판별 (IsCompatibleType)
    - notNullValue : Null인지, 아닌지를 판별 (IsNull)
    - nullValue : Null인지, 아닌지를 판별 (IsNull)
    * sameInstance : Object가 완전히 동일한지 비교. equals 비교가 아닌 ==(주소 비교)로 비교하는 것과 동일(IsSame)

* Logical
    - allOf : 비교하는 두 오브젝트가 각각 여러 개의 다른 오브젝트를 포함하고 있을 경우에, 이를테면 collection 같은 오브젝트일 경우 서로 동일한지 판별. Java의 숏서킷(&& 비교)과 마찬가지로 한 부분이라도 다른 부분이 나오면 그 순간 false를 돌려준다. (AllOf)
    - anyOf : allOf와 비슷하나 anyOf는 하나라도 일치하는 것이 나오면 true로 판단. Java의 숏서킷(||)과 마찬가지로 한 번이라도 일치하면 true를 돌려줌 (AnyOf)
    - not : 서로 같지 않아야 한다. (IsNot)

* Beans
    - hasProperty : Java 빈즈 프로퍼티 테스트 (HasProperty)

* Collection
    - array : 두 배열 내의 요소가 모두 일치하는지 판별 (IsArray)
    - hasEntry, hasKey, hasValue : Map 요소에 대한 포함 여부 판단 (IsMapContaining)
    - hasItem, hasItems : 특정 요소들을 포함하고 있는지 여부 판단 (IsCollectionContaining)
    - hasItemInArray : 배열 내에 찾는 대상이 들어 있는지 여부를 판별 (IsArrayContaining)

* Number
    - closeTo : 부동소수점(floating point) 값에 대한 근사값 내 일치 여부, 값(value)과 오차(delta)를 인자로 갖는다. (IsCloseTo)
    - greaterThan/greaterThanOrEqualTo : 값 비교, >, >= (OrderingComparison)
    - lessThan/lessThanEqualTo : 값 비교, <, <= (OrderingComparison)

* Text
    - containsString : 문자열이 포함되어 있는지 여부 (StringContains)
    - startsWith : 특정 문자열로 시작 (StringStartsWith)
    - endsWith : 특정 문자열로 종료 (StringEndsWith)
    - equalsToIgnoringCase : 대소문자 구분하지 않고 문자열 비교 (IsEqualIgnoringCase)
    - equalToIgnoringWhiteSpace : 문자열 사이의 공백 여부를 구분하지 않고 비교 (IsEqualIgnoringWhiteSpace)
 
