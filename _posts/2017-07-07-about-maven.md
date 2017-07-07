---
layout: post
title: Maven에 대해
---

About Maven
===============
 
### 메이븐이란
- 빌드 도구
- 프로젝트 관리 도구
- 빌드되고 있는 작업의 추상적인 컨테이너
- 수많은 3rd party component를 사용하는 상호적인 모듈들과 라이브러리로 이루어진 대규모 컬렉션을 일관된 방법으로 관리하고 빌드하는 프로젝트
- 매일 3rd party의 의존성을 관리해야하는 모든 엔지니어들의 부담을 덜어주는 도구
 
한마디로 **수많은 플러그인과 라이브러리로 이루어진 큰 프로젝트들을 관리하고 빌드하는데 많은 노력과 시간이 필요하지 않게 도와주는 도구**
 
 
* Ant같은 빌드 도구는 only `preprocesssing, compliation, testing, 배포`하는 것만이 목적
 
 
* Maven같은 프로젝트 관리 도구는 빌드도구가 제공하는 빌드 기능뿐 아니라, `리포트를 생성`하고, `웹사이트를 구축`하며, 함께 작업하는 개발자들간의 `커뮤니케이션을 더욱 용이하게 해주는 기능들을 제공`함.
 
 
#### Maven은 
- 프로젝트 객체 모델(project object model)
- 표준 세트(set of standards)
- project lifecycle
- 의존 관리 시스템(dependency management system)
- 각각의 플러그인들의 빌드논리
- etc.
위를 모두 포함한 프로젝트 관리 도구
 
 
### Convention Over Configuration (CoC)
- **CoC란 system, framework, library, 그리고 framework가 default 값을 가지고 있어 개발자가 불필요한 설정없이 쉽게 시스템을 실행할 수 있도록 만든 방법**
- CoC의 개념: 관습적으로 사용되어지는 수많은 설정들을 default값으로 제공해서 개발자들로 하여금 프로젝트 설정에 들이는 시간과 노력을 줄여주고, 또한 설정을 공통적으로 사용함으로서 유연성과 단순성을 잃어버리지 않도록 하기 위한 소프트웨어 디자인 패러다임
 
 
maven은 프로젝트를 체계적/합리적인 패턴을 제공하기 위하여 CoC 컨셉을 적용함.  
CoC가 적용된 maven은 설정값을 변경하지 않는다면,
- 소스코드는 `${basedir}/src/main/java` 에
- 리소스들은 `${basedir}/src/main/resource` 에
- 테스트코드들은 `${basedir}/src/test` 에
- 프로젝트 파일은 `${basedir}/target` 안에 
JAR 파일로 제공된다.

Maven의 강점은 CoC를 거의 강요하는데서 온다. __기본 설정을 변경하지 않는 한 메이븐은 개발자에게 설정에 필요한 노력을 거의 요구하지 않는다.__

### A Common Interface
maven이 common interface를 제공하기 전에는 빌드 방법과 소스코드의 방식이 모두 달랐기 때문에 하나의 테스트 코드만으로도 어떻게 적용을 할 것이고 어떻게 모든 프로젝트에 맞출 수 있는지를 고민해야 했다. 또한 빌드를 하기 위해서 필요한 것들과 어떤 라이브러리가 필요한지, 어디에다 넣을 것인지, 어떤 골을 가지고 빌드를 실행시킬수 있는 지등 사소한 모든 것들을 하나하나 명시해야 했다. __maven은 common interface를 제공함으로써 mvn install이란 멸령 하나만으로도 손쉽게 빌드를 할 수 있게 되었다.__


### Universal Reuse through Maven Plugins
* __maven core는 각 세부적인 일에 대해 자세히 모름.__ maven은 대부분의 책임을 자신의 lifecycle에 영향을 미칠 수 있고, 빌드 goal에 접근할 수 있는 maven plugin에게 전가시키도록 디자인 됨.
* maven 안에서 이루어지는 대부분의 작업은 (compiling source, packaging bytecode, publishing site, etc. 빌드에 필요한 모든 일들을 관리하는) 플러그인 goal안에서 일어남.
    - 예를들어 WAR 파일로 패킹하는 것이나, JUnit으로 테스트를 하는 등의 일들은 순수 maven으로는 할 수 없음.
        - 이러한 일들은 대부분 플러그인들이 대신하고, 각각의 플러그인들은 maven repository에서 찾을 수 있음.
* maven은 수많은 플러그인들을 사용하여 하나의 프로젝트를 관리하고 빌드하는데, __각 플러그인의 호환성과 버전정보를 통합적으로 관리해야만 문제없이 자원을 공유할 수 있음.__
    - 이러한 관리는 `Project Object Model`에서 하는데 maven을 업그레이드 하거나, 새로운 소프트웨어를 설치하지 않아도, 특히 어떠한 프로젝트 파일도 바꾸지 않고 단지 Project Object Model(POM)이라는 메이븐 설정파일에서 버전정보만 바꾸는 것만으로 가능.
* maven은 중앙집중적이고 일반적인 재활용이 가능한 플러그인들을 위해 __추상적인 빌드를 함__. 예를 들어, 어떤 플러그인의 버전이 업그레이드 됬으면 그걸 위해서 프로젝트의 모든 빌드 시스템이나 설정등을 변경할 필요 없이 해당 플러그인은 remote repository에서 다운로드 되고, 그 플러그인은 스스로 중앙집중적이고 재활용이 가능한 플러그인 일 것임?(이것이 Universal Reuse through Maven Plugins)


### Project Model의 개념
* maven은 하나의 프로젝트에 `하나의 모델`을 가지고 있음. maven은 관련된 소프트웨어 프로젝트들과 소프트웨어 개발에 새로운 의미를 부여하는 플랫폼이다. 

모델로 정의된 모든 프로젝트들은 다음과 같은 특성을 갖음.
* 의존관계 정리(Dependency Management): 하나의 프로젝트는 `group identifier, artifact identifier, 그리고 version으로 이루어진 독립된 관계로 정의 되어지기 때문에` 의존관계를 설정할 수 있다.
* Remote Repositories: 의존관계 정리를 통해서 우리는 서로 연결된 maven artifacts를 Project Object Model(POM)을 통해서 정의하고 연결할 수 있다.
* 일반적인(통합적인) 빌드 로직 사용(Universal Reuse of Build Logic): 플러그인들은 POM에 정의된 설정 파라메터들과 기술된 데이터를 이용하는 로직을 포함한다. 플러그인들은 알 수 있는 위치에 존재하는 특별한 파일들을 실행하도록 만들어지지 않음.
* 툴 확장성 및 통합성(Tool Portability/Integration): 이클립스, 넷빈즈, 인텔리제이와 같은 툴들은 프로젝트에 관련된 정보들이 존재하는 특별한 위치가 존재한다. maven은 제각각 저장되던 설정파일을 표준화 시키고, 그로인해 모델로 부터 쉽게 실행 될 수 있게 한다.
easy searching andfiltering of Project Artifacts: 넥서스와 같은 도구가 POM에 저장된 정보를 이용하여, 레파지토리의 내용들을 쉽게 indexing 및 searching을 할 수 있게 도와줌.
