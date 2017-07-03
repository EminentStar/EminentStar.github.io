---
layout: post
title: HAProxy doc/intro.txt 한글 번역(Incomplete)
---

--------------
HAProxy Starter Guide
--------------
    version 1.8


이 문서는 모든 사람들; 내용을 다시 찾고자 하는 이전 버전을 알고 있는 사람뿐만 아니라 HAProxy를 모르는 사람들을 위한 HAProxy 소개서이다.

여기서 가장 첫번째로 집중하고자하는 것은 사용자에게 모든 내용을 제공하고자 함인데, 이는 HAProxy가 그들이 찾고자하는 것인지 아닌지를 결정할 때 도움이 될 것이다. 숙련된 사용자들은 아마 여기서 그들이 가지고 있는 아이디어에대한 몇몇의 해결책을 찾을 것인데, 그들이 아마 알지못했던 새로운 피쳐에 대한 것일 것이다. 몇몇의 sizing information이 또한 제공될 것이고 제품의 lifecycle이 설명되고, 부분적으로 겹치는 제품들과의 비교또한 제공될 것이다.

이 문서는 그 어떤 configuration이나 hint에 대한 도움을 주지 않을 것이다. 하지만 문서는 관련된 문서를 어디서 찾아야할지는 설명할 것이다. 아래의 summary는 당신이 section들을 이름을 통해 찾는데 도움을 주는 수단이며 문서를 전체적으로 navigate할 것이다.

Note to documentation contributors:
- 이 문서는 한 라인당 80 칼럼으로 포맷팅됬는데 하물며 indentation이나 tab을 제외한 공간의 수이다. 이 문서가 어디서는 쉽게 보일 수 있도록 제발 이 rule을 강력하게 따라주길 바란다. 만약 당신이 sections을 추가한다면 제발 쉬운 검색을 위해 아래의 summary를 업데이트하라.



Summary
-------

1. Available documentation

2. Quick introduction to load balancing and load balancers

3.    Introduction to HAProxy
3.1.      What HAProxy is and is not
3.2.      How HAProxy works
3.3.      Basic features
3.3.1.        Proxying
3.3.2.        SSL
3.3.3.        Monitoring
3.3.4.        High availability
3.3.5.        Load balancing
3.3.6.        Stickiness
3.3.7.        Sampling and converting information
3.3.8.        Maps
3.3.9.        ACLs and conditions
3.3.10.       Content switching
3.3.11.       Stick-tables
3.3.12.       Formated strings
3.3.13.       HTTP rewriting and redirection
3.3.14.       Server protection
3.3.15.       Logging
3.3.16.       Statistics
3.4.      Advanced features
3.4.1.        Management
3.4.2.        System-specific capabilities
3.4.3.        Scripting
3.5.      Sizing
3.6.      How to get HAProxy

4.    Companion products and alternatives
4.1.      Apache HTTP server
4.2.      NGINX
4.3.      Varnish
4.4.      Alternatives


1. Available documentation
-------------------------------------

아래 오는 문서안에 완전한 HAProxy 문서가 포함되어 있다. 시간을 절약하고 당신이 필요한 것에 대한 거의 정확한 response를 얻기 위해서 제발 관련된 문서를 consult하는 걸 확실하게 하라. 또한, 아래의 문서에서 현재 mailing list의 response가 나타나 있는 것을 질문하는 것을 자제해달라.

- intro.txt(this document): 이는 로드 밸런싱의 기초, 제품으로써의 HAProxy, HAProxy가 무엇을 하고 안하는지, 알려진 몇몇의 피해야할 함정, 몇몇의 OS에 특화된 제한, 그것을 어떻게 얻는지, 어떻게 발전시켜야할 지, 모든 알려진 fixes와 함께 당신이 실행하는걸 어떻게 보장해야할 지, 그것을 어떻게 업데이트해야할 지, 이의 보완이나 대안에 대해 나타낸다.

- management.txt: haproxy를 시작하는 방법, 실행중에 관리하는 방법, multiple node상에서 관리하는 방법, 보이지않게 업그레이드하는 방법에 대해 설명한다.

- configuration.txt: 모든 configuration keywords와 그들의 옵션에 대해 상세히 기술한 reference manual

- architecture.txt: 최상의 로드밸런스드된 인프라를 아키텍쳐링하는 방법과 서드파티 제품과 interact하는 방법에 대해 설명한 아키텍쳐 매뉴얼

- coding\-style.txt: 본 프로젝트에 코드를 적용시키고 싶어하는 개발자를 위한 문서. 이는 코드를 적용시키기 위한 스타일에 대해 설명한다. 이는 매우 엄격하지는 않으며 모든 코드베이스가 이를 따르지는 않는다. 하지만 이 스타일을 너무 지키지 않은 컨트리뷰션은 reject될 것이다.

- proxy-protocol.txt: HAProxy와 수 많은 서드 파티 제품들에 의해서 구현된 PROXY protocol의 de-facto 명세서이다.

- README: haproxy를 소스로 부터 빌드하는 방법


2. Quick introduction to load balancing and load balancers
------------------------------------------------------------

로드 밸렁싱은 복수의 컴포넌트를 결합시키는 것으로 구성되어있는데, 이는 scalable한 방법으로, end user로 부터 어떤 방해없이 각각의 컴포넌트의 개별 capacity위의 전체의 processing capacity를 성취하기 위해서이다. 이는 오직 한 작업을 수행하는 한개의 노드가 남기전까지 더 많은 작업이 동시다발적으로 수행되게 한다. 그러나 하나의 작업은 여전히 한번에 하나의 노드에서 수행될 것이고 로드 밸런싱이 없는 것보다 빠를 수 없다. 많은 작업일 수록 많은 노드가 필요로 할 것이며 모든 컴포넌트를 사용하도록 하기위해 그리고 로드 밸런싱으로 부터 완전한 이익을 얻기 위해서 효율적인 로드 밸런싱 메카니즘이 필요로 할 것이다. 이에 대한 좋은 예는 고속도로상에서의 수많은 lanes이고 이 lanes는 많은 차들이 그들의 속도를 높이는 것 없이 같은 시간에 통과하도록 한다.

Examples of load balancing
- 멀티 프로세서 시스템에서의 process scheduling
- Link load balancing(eg: EtherChannel, Bonding?)
- IP address load balancing (eg: ECMP, DNS roundrobin)
- Server load balancing (via load balancers) 

로드 밸런싱 작업을 수행하는 메카니즘이나 컴포넌트가 로드 밸런서로 불린다. 웹 환경에서 이 컴포넌트들은 "network load balancer"라고 불리며 더 흔히는 "load balancer"로 불리는데, 이게 가장 많이 알려진 로드 밸렁싱의 예이다.

로드 밸런서는 아마 이렇게 동작할 것인데,
- link level에선: link load balancing이라 불리며, 어떤 네트워크 링크로 패킷을 보내야할 지를 고르는 것으로 구성된다.
- network level에선: network load balancing이라 불리며, 패킷의 흐름이 어디로 가야할지에 대한 라우트를 고르는 것으로 구성된다.
- server level에선: server load balancing으로 불리며, 어떤 서버로 커넥션이나 리퀘스트를 진행해야할 지를 고르는 것으로 구성된다.


몇몇의 겹치는 것이 있긴하지만, **두가지 구분되는 기술**이 존재하며 다른 니즈를 반영한다. 각각의 케이스에서, 로드밸런싱은 자연스러운 흐름으로 부터 트래픽을 분리하는다는 것으로 구성되고 이 동작이 모든 라우팅 결정사이에서 필요되는 consistency를 유지하기 위해서 최소한의 주의를 기울일 필요가 있다는 것을 명심하는 것이 중요하다.

첫번째 것은 packet 레벨에서 행동하며 패킷의 묶음이나 패킷 개별로 진행을 한다. input 패킷과 output 패킷사이에는 1대1 관계가 있기 때문에 일반적인 network sniffer를 이용하여 트래픽이 양쪽 사이드의 로드밸런서를 따르는게 가능하다. 이 기술은 매우 저렴하며 엄청나게 빠르다. 이것은 보통 하드웨어(ASICs)로 구현되는데 ECMP를 하는 스위치와 같은 line rate에 도달하도록 허락한다. 보통 stateless하지만, 이는 물론 stateful할 수도 있다.(세션을 Layer4-LB또는 L4에 속하는  하나의 패킷으로 간주하면). 그리고 아마 DSR(Direct Server Return; LB에 다시 보내지 않고 서버가 직접 리턴하는)을 지원 할 수도 있다 만약 패킷이 수정되지 않다면. 그러나 content awareness를 제공하지 않는다. 이 기술은 network-level의 로드 밸런싱에 매우 적합하다; 가끔 고속의 매우 기본적인 서버 로드밸런싱에서도 사용되긴 하지만.


두번째 것은 session contents상에서 행동한다. 이는 input stream이 전체로써 다시 모이고 진행되는 게 필요하다. contents는 아마 수정될 수도 있으며 output stream은 새로운 패킷들로 분리된다. 이러한 이유때문에 이들은 주로 프록시에 의해 수행되며 그들은 layer 7 load balancers 또는 L7로 불린다. 이는 각각의 사이드에서 두개의 구분된 커넥션이 있다는 것을 내포하며 input/output 패킷의 사이즈나 갯수사이에 아무 관계가 없다는 것을 내포한다. 클라이언트와 서버는 같은 프로토콜(예를 들면 IPv4 vs IPv6, clear vs SSL)을 사용할 필요가 없다. 작업은 항상 stateful하며 return되는 트래픽은 항상 로드 밸런서를 통과한다. the extra processing은 비용과 관련이 있는데, 그래서 이는 line rate를 성취하는 것이 항상 가능한 것은 아니다; 특히 작은 패킷들과 함께는. 반면에 그것은 넒은 가능성을 제공하며, 비록 하드웨어 제품에 포함될수 있더라도, 일반적으로 pure software에 의해서 수행된다. 이 기술은 서버 로드 밸런싱에 매우 적합하다.


packet-based 로드 밸런서는 일반적으로 cut-through mode로 배포된다.(cut-through: In coputer networking, cut-through switching is a method for packet switching systems, wherein the switch starts forwarding a frame(or packet) before the whole frame has been received, normally as soon as the destination address is processed) 그래서 그들은 트래픽의 normal path상에 설치되고 configuration에 따라 분리한다. return traffic은 로드밸런서를 필수로 통과하진 않는다. 몇몇의 편경이 네트워크 목적지 주소에 적용될 수 있는데, 이는 트래픽을 적절한 목적지로 직접 보내기 위함이다. 이러한 경우, return traffic이 로드밸런서를 통화가는 것은 의무적이다. 만약 라우트가 이것을 가능하게 하지 않는다면, 로드 밸런서는 물론 패킷의 source address를 스스로 변경할 것인데, 이는 return traffic이 그것을 통화하도록 강제하기 위함이다.


Proxy-based 로드 밸런서는 하나의 서버로써 자신의 IP address, port를 가지며 아키텍쳐의 변경없이 배포된다. 때때로 이것은 애플리케이션으로의 adaptations를 수행할 필요가 있는데, 이는 클라이언트가 적절히 로드밸런서의 IP address로 접근하고 직접 애플리케이션 서버에 접근하지 않도록 하기 위함이다. 몇몇의 로드 밸런서는 아마 어떤 서버들의 리스폰스를 제대로 맞춰야만 한다 이것이 가능하게 하기 위해(eg: HTTP redirect에 쓰이는 HTTP Location header field). 몇몇의 proxy-based load balancer는 아마 그들이 소유하고 있지 않은 주소에 대한 트래픽을 방해할 수도 있고, 서버에 연결할때 클라이언트의 주소를 spoof할 수도 있다. 이것은 그들이 마치 regular router나 firewall인 것처럼 배포되게 만든다. 이것은 특히도 몇몇 프로덕트에 적합한데, 패킷모드와 프록시 모드를 묶는 것들이 그런 프로덕트이다. 이의 경우 DSR은 명백히 불가능하며 return traffic은 여전히 로드 밸런서로 route back되야만 한다.


매우 확장성있는 layered approach는 다수의 load balanced link로 부터 트래픽을 받는 front router를 가지는 것으로 구성되며, 이 트래픽을 다수의 stateful한 packet-based load balancers(L4)의 첫번째 layer로 트래픽을 분산시키기 위해 ECMP를 사용한다. 이 L4 load balancers는 트래픽을 훨씬 더 많은 수의 proxy-based  load balancers(L7)으로 통과시키는데, 이 L7은 어느 서버가 궁극적으로 이 트래픽을 받아야할지를 결정하기 위해서 content를 parse해야만 한다. 


컴포넌트의 갯수와 트래픽을 위한 가능한 path는 장애위험을 증가시킨다. 매우 거대한 환경에서, 소수의 장애가 나서 보수나 교체가 필요한 컴포넌트를 영구적으로 가지는 것은 극히 정상이다. 전체 스택의 health에 대한 인지없이 로드밸런싱을 하는 것은 가용성을 상당히 감소시킬 수 있다. 이러한 이유때문에 어떤 일반적인 로드밸런서라도 그것이 트래픽을 전달하기로 되어있는 컴포넌트가 여전히 살아있고 도달가능한지에 증명할 것이며, 장애가 난 것에는 트래픽 전송을 멈출 것이다. 이는 다양한 방법을 사용해서 성취될 수 있다.


가장 흔한 것은 주기적으로 probe를 보내서 컴포넌트가 여전히 작동하는지를 확인하는 것이다. 이 probes는 "health check"라고 부른다. They must be representative of the type of failure to address. 예를 들어 ping-based check는 웹 서버가 crash됬는지 감지하지 않을 것이며 port로 부터 더이상 listen하지 않을 것이다.반면에 port로의 connection은 이를 증명할 것이고 더 advanced request는 심지어 서버가 여전이 작동하고 그들이 의존하고 있는 데이터베이스가 여전히 accesible하다는 것을 검증할 것이다. Health check는 자주 소수의 retry를 포함하는데 이는 이따금씩의 에러를 커버하기 위해서이다. checks사이의 기간은 장애 컴포넌트가 에러 발생 후에 오랫동안 사용되지 않게 하는 걸 보장하기 위해서 충분히 짧아야만 한다. 


다른 방법은 트래픽들이 정확하게 진행됬는지 관측하고 부적절한 리스폰스를 return하는 컴포넌트를 파악하기 위해서 목적지로 보내진 프로덕션 트래픽을 sampling하는 것으로 구성된다. 그러나, 이는 프로덕션 트래픽의 일부의 희생이 필요하며 이는 항상 믿을만 하진 않다. 이 두 메카니즘의 조합은 양쪽 세상에서의 best를 으 양쪽 세계에 제공되는데 이는 장애를 감지하기 위해서 사용되고, 오직 health checks가 fault의 끝을 감지한다.

마지막 방법은 중앙화된 reporting을 포함한다: 중앙의 모니터링 agent는 주기적으로 모든 컴포넌트의 상태에 관해 모든 로드 밸런서를 업데이트한다. 이는 때때로 낮은 정확도나 반응성이 있긴 하지만, 모든 컴포넌트에게 인프라에 대한 전체적인 뷰를 제공한다.이는 다수의 로드 밸런서와 서버가 있는 환경에서 적합하다.


Layer 7 load balancer는 또한 stickiness 혹은 persistence라고 알려진 또다른 문제에 직면한다. 기본 원리는 그들이 일반적으로 같은 origin(such as an end user)로부터 같은 타겟에게로 다수의 미묘한 리퀘스트나 커넥션을 전달해야만 하는 것이다. 알려진 가장 좋은 샘플은 온라인 쇼핑몰에서의 shopping cart이다. 만약 각각의 클릭이 새로운 커넥션을 이끈다면, 유저는 항상 그의 shopping cart를 가지고 있는 server로 보내져야만 한다. Content-awareness는 element를 전달할 서버를 구분하기위해서 리퀘스트에서의 몇몇 elements를 가리키기 쉽게 한다??. 그러나 이게 항상 충분하진 않다. 예를 들어, 만약 source address가 server를 고르기 위한 key로써 사용된다면, hash-based algorithm이 사용될 것이고 주어진 IP address가 address 분할에 기반하여 항상 같은 서버로 보내질


