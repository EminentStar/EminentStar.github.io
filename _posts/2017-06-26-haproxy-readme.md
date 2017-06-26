---
layout: post
title: HAProxy README 한글 번역
---

<div class="message">
이제부터 오픈소스를 시작하려고 하고 다양한 프로젝트 중에서도 소프트웨어 로드밸런서인 HAProxy를 선택했다. 첫번째 이유로는 이전부터 대규모 서비스, 고가용성에 관심이 많았고, 공부를 해보고 싶었다. 그리고 그 중에서도 API 서버 확장에 관련된 프로젝트를 선택한 이유는 프로젝트에 대해 공부를 하면서 부족한 네트워크 지식을 같이 공부할 수 있지 않을까라는 생각에서이다.
</div>

HAProxy how-to
version 1.8
willy tarreau
2016/11/25


1) How to build it
------------------------------

이것은 development 버전이고, 그래서 가끔씩 이전의 알림없이 피쳐를 추가하거나 삭제하는 것은 깨질 수 있고, 이런 것은 production에 사용되선 안된다. 만약 당신이 소스로부터 빌드를 해본 적이 없거나 업데이트를 팔로우한 적이 없다면 Linux distribution이나 다른 소프트웨어 벤더에서 제공하는 패키지를 사용하는 것을 추천한다. 그들의 대부분은 이 작업을 심각하게 다루고 있고 중요한 수정사항의 backporting에 대해서도 잘하고 있다. 만약에 어떤 이유에서 당신의 시스템을 위한 패키지된 버전보다 다른 버전을 선호한다면, 또 모든 수정사항에 대해 확실하게 잡고 가고 싶거나 상업적인 지원을 얻고 싶다면 아래의 사이트에서 다른 선택에 대해 확인할 수 있다.

[http://www.haproxy.com](http://www.haproxy.com)

haproxy를 빌드하기 위해서 필요한 것
- GNU make. Solaris나 OpenBSD의 make는 GNU Makefile과 함께 동작하지 않는다. 만약 'make'를 실행할 때 많은 syntax error를 얻는다면, 당신은 아마 'gmake'(흔히 BSD 시스템상에서 GNU make를 위해 사용되는)와 함께 재시도하기를 원할 것이다.
- 2.95~4.8 사이의 GCC. 그외의 버전또한 작동은 하지만 테스트되지 않음.
- GNU ld(GNU linker)


또한 당신은 libpcre 지원과 함꼐 빌드하기를 원할지도 모르는데, libpcre는 매우 효율석인 regex 구현체를 제공할 것이며, 또한 Solaris상에서의 badness들을 픽스할 것이다.


haproxy를 빌드하기 위해서, 당신은 아래의 목록중에서 당신의 target OS를 선정해야되며 이를 TARGET 변수에 설정해야한다.

- linux22     for Linux 2.2
- linux24     for Linux 2.4 and above (default)
- linux24e    for Linux 2.4 with support for a working epoll (> 0.21)
- linux26     for Linux 2.6 and above
- linux2628   for Linux 2.6.28, 3.x, and above (enables splice and tproxy)
- solaris     for Solaris 8 or 10 (others untested)
- freebsd     for FreeBSD 5 to 10 (others untested)
- netbsd      for NetBSD
- osx         for Mac OS/X
- openbsd     for OpenBSD 5.7 and above
- aix51       for AIX 5.1
- aix52       for AIX 5.2
- cygwin      for Cygwin
- haiku       for Haiku
- generic     for any other OS or version.
- custom      to manually adjust every setting

당신은 또한 어떤 optimization으로 부터 이점을 얻기 위해 당신의 CPU를 고를 수 있다. 이는 특히 UltraSparc machine상에서 중요하다. 이를 위해, 당신은 아래의 목록중에서 하나를 선택해 CPU 변수에 설정할 수 있다.

- i686 for intel PentiumPro, Pentium 2 and above, AMD Athlon
- i586 for intel Pentium, AMD K6, VIA C3.
- ultrasparc : Sun UltraSparc I/II/III/IV processor
- native : use the build machine''s specific processor optimizations. Use with extreme care, and never in virtualized environments (known to break).
- generic : any other processor or no CPU-specific optimization. (default)


대안으로써, 당신은 그냥 CPU_CLFAGS 값을 단시의 플랫폼상에서 최적의 GCC option에 설정할 수 있다.


당신은 당신의 native compiler의 타겟과 match되지 않는 target binaries 빌드하길 원할 수도 있다. 이는 특히 당신이 64-bit 시스템에서 32-bit binary를 빌드하기를 원할 때가 그런 상황이다. 이런 목적으로 ARCH 변수를 사용해라. ... ...

만약 당신의 시스템이 PCRE(Perl Compatible Regular Expressions)을 지원한다면, 당신은 정말 libpcre와 함꼐 build해야한다.(libpcre는 다른 libc 구현체보다 2~10배 이상 빠르다) Regex는 header processing(deletion, rewriting, allow, deny)를 위해 사용된다. 단 한가지 libpcre의 불편한 점은 이것이 아직 널리 확산되지 않았다는 점인데, 그래서 만약 당신이 다른 시스템에서 빌드를 한다면, 그 시스템이 다양한 라이브러리를 가지고 있지 않다면 문제가 발생할 수도 있다. 이런 상황에서 당신은 정적으로 libpcre를 haproxy로 link를 해야하는데, 그러면 타겟 시스템에서 libpcre를 설치할 필요는 없을 것이다. PCRE를 위한 이용가능한 빌드 옵션은 다음과 같다.


- USE_PCRE=1 to use libpcre, in whatever form is available on your system (shared or static)

- USE_STATIC_PCRE=1 to use a static version of libpcre even if the dynamic one is available. This will enhance portability.

- with no option, use your OS libc''s standard regex implementation (default).  Warning! group references on Solaris seem broken. Use static-pcre whenever possible.


만약 당신의 시스템이 PCRE를 제공하지 않는다면, 당신이 그것을 [http://www.pcre.org](http://www.pcre.org)에서 다운받기를권장하고 그것을 혼자 빌드하기를 권한다. 이는 매우 빠르고 쉽다.

최근의 시스템들은 IPv6 hostname을 getaddrinfo()를 이용하여 해결할 수 있다. 이 primitve는 모든 libcs에서 최신이 아니며 모든 시스템상에서 작동하지 않는다. 그러므로 지원은 디폴트로 되지 않으며 그 뜻은 몇몇의 오직 IPv6로써 해결하는  host name는 해결하지 않을 것이며..? 설정은 parsing을 하는 동안 error를 생략할지도 모른다. 만약 당신이 당신의 OS libc가 getaddrinfo()를 지원하는 것에 신뢰할만하단 것을 안다면, 이를 활성화하기 위해 당신은 USE_GETADDRINFO=1을 make 커맨드라인에 추가할 수 있다. 이게 최근의 모든 mainstream distros상에서 정상적으로 작동되기 시작하면서 이것은 대부분의 Linux distro packager에 추천되는 옵션이 되었다. 이 옵션은 Solaris 8 이상부터는 자동으로 활성화된다.


그리고 "USE_OPENSSL=1"을 make 커맨드 라인에 사용함으로써 GNU makefile을 이용할 SSL의 native한 supoort를 더하는게 가능해진다. libssl과 libcrypto는 haproxy와 자동으로 linking될 것이다. 어떤 시스템들은 물론 libz를 필요로 할 것인데, 그래서 만약 build가 deflateInit()과 같은 symbol을 놓친 것 때문에 실패한다면 "ADDLIB=-lz"와 함게 다시 시도해라.


또한, [https://www.openssl.org](https://www.openssl.org)에서 찾아지는 것처럼 때때로 취약점들이 찾아지고 이런 취약점에 공격받기를 원하지 않기에,  당신은 항상 최신버전의 OpenSSL을 사용하기를 권고되어진다. HAProxy는 현재 모든 지원되는 브랜치상에서 제대로 빌드되고 있다. 브랜치 1.0.2가 가장 다양한 피쳐를 사용하기 위해 추천되어지는 버전이다.


haproxy에 정적으로 OpenSSL을 link하기 위해서 no-shared keyword와 함께 OpenSSL을 빌드해라. 그리고 그걸 로컬 디렉토리에 설치해라. 그래야 당신의 시스템에 영향을 안 끼칠 것이다.

```
$ export STATICLIBSSL=/tmp/staticlibssl
$ ./config --prefix=$STATICLIBSSL no-shared
$ make && make install_sw
```

haproxy를 빌드할때,(필요하다면) 부수적인 libs를 make하고 포함시키기 위해 ADDLIB와 함께  path를 SSL_INC와 SSL_LIB를 통해 통과시켜라.(아래의 예 참고)

```
$ make TARGET=linux26 USE_OPENSSL=1 SSL_INC=$STATICLIBSSL/include SSL_LIB=$STATICLIBSSL/lib ADDLIB=-ldl
```

또한 HTTP Compreesion으로 부터 이익을 얻기 위해 zlib에 대한 native support를 포함하는 것이 가능하다. 이를 위해서, "USE_ZLIB=1"을 make 커맨드 라인에 쓰고, zlib이 시스템상에서 최신의 것인지도 체크해라. 대안으로써 libslz를 사용하는 것이 가능한데("USE_SLZ=1"를 쓰면 된다), 이는 더 빠르고, 적은 메모리를 쓰지만 약간 압축의 효율이 낮다.


Zlib은 대부분의 시스템에서 흔히 찾아볼 수 있는데, 업데이트는 [http://www.zlib.net](http://www.zlib.net)에서 다운받을 수 있다. 매우 쉽고 빠르게 빌드할 수 있다. Libslz는 [http://1wt.eu/projects/libslz/](http://1wt.eu/projects/libslz/)에서 다운받을 수 있으며 심지어 빌드하기가 더 쉽다.


디폴트로, DEBUG 변수는 debug symbol을 활성화 시키기 위해서 '-g'를 설정되어진다. 이것은 흔하지 않은 시스템상에서 비활성화하기에 현명하지 않은 방법인데, 이것이 많은 상황에서 당신이 그걸 필요로 할때 완전한 코어를 얻을 수 있는 단 하나의 방법이기 때문이다. 이와 다르게 당신은 DEBUG를 '-s'으로 설정해서 binary를 생략할 수 있다.

예를 들어 내가 Solaris 8에서 빌드하기 위해사 이걸 사용한다면
```
$ make TARGET=solaris CPU=ultrasparc USE_STATIC_PCRE=1
```

그리고 내가 OpenBSD나 FreeBSD상에서 이 방법을 사용한다면
```
$ gmake TARGET=freebsd USE_PCRE=1 USE_OPENSSL=1 USE_ZLIB=1
```

그리고 고전적인 Linux상에서 SSL과 ZLIB 지원과 함꼐 사용한다면(eg: Red Hat 5.x)
```
$ make TARGET=linux26 USE_PCRE=1 USE_OPENSSL=1 USE_ZLIB=1
```

그리고 최근의 Linux >= 2.6.28상에서 SSL, ZLIB의 지원과 함꼐 사용한다면
```
$ make TARGET=linux2628 USE_PCRE=1 USE_OPENSSL=1 USE_ZLIB=1
```

OpenSSL이 어쨋든 ZLIB은 필요로 하지만 compression에 대한 지원 없이 SSL 지원과 함께 64bit linux 시스템 상에서 32-bit binary를 빌드하기 위해선
```
$ make TARGET=linux26 ARCH=i386 USE_OPENSSL=1 ADDLIB=-lz
```


SSL stack은 모든 runing processes간의 session cache synchronication을 지원한다. 이는 어떤 multiple flavor에 오는 atomic 작업과 synchronization 작업들이 시스템과 아키텍쳐에 따라 달려있다는 것을 말한다.


* Atomic operations:
    - internal assembler versions for x86/86_64 architectures
    - 다른 아키텍쳐를 위한 gcc builtins. 몇몇의 아키텍처는 아마 완벽하게 gcc의 최신 버전을 지원하지 않을 수도 있고 더 최신의 gcc를 필요로 할 수 있다. 만약 당신의 아키텍처가 지원되지 않는다면 당신은 지원이 된다면 pthread를 사용하거나 shared cache를 비활성화해야한다.
    - pthread(posix threads). Pthread는 매우 흔하지만, inter-process 지원은 그렇게 흔하진 않다. 그리고 몇몇의 오래된 OS는 multi-process mode를 활성화할때 에러를 보고하지 않는다. 그래서 그들은 이따금씩 실패하곤 했고 crash를 일으켰었다. Linux의 구현체는 괜찮다. OpenBSD는 그들을 지원하지 않고 빌드하지 않는다. FreeBSD 9는 빌드하고 런타임에 에러를 보고하ㄷ는데, 특정의 오래된 버전은 아마 실패할지도 모른다. Pthread는 USE_PTHREAD_PSHARED=1을 사용해서 활성화된다.

* Synchronization operations:
    - internal spinlcok : 이 모드는 OS에 독립적이고 가볍지만 많은 프로세스로 잘 확장가능하지는 않을 것이다. 그러나 세션 캐시로의 접근은 이 모드가 확실히 항상 사용될 수 있을만큼 거의 없다. 이것은 디폴트 모드다.
    -  Futexes;linux-specific한 매우 확장성이 있는 경량의 mutexes인데 구현된다. user-space상에서 커널로부터의 제한된 지원과 함꼐. 이는 Linux 2.6이상부터 디폴트이며 USE_FUTEX=1을 전달함으로써 활성화시킬 수 있다.
    - pthread(posix threads). 위 참조.


만약 이 메카니즘들중 아무것도 당신의 플랫폼에 의해 지원되지 않는다면, 당신은 아마 USE_PRIVATE_CACHE=1과 함께 빌드할 필요가 있을 것이다, SSL cache shasring을 완벽히 비활성화하기 위해서. 그후에 멀티 프로세스상에서 SSL을 실행시키지 않는 것이 좋다.


만약 당신이 다른 defines, includes, libraries, etc...를 전달할 필요가 있다면, Makefile을 확인해라, 당신의 상황에서 어느것이 이용가능한지, 그리고 USE_* 변수를 Makefile에 사용해라.


AIX 5.3은 generic target과 함께 동작하는 것으로 알려져 있다. 그러나 이 binary를 5.2나 그 이전의 버전에서 실행시키기 위해선, 또 당신은 DEFINE="-D_MSGQSUPPORT"와 함께 빌드할 필요가 있다. 그외에는 libc에서 최신이 아니라면 __fd_select()가 사용되어질 것이다. 그러나 이는 "aix52" target을 사용해서 쉽게 전달할 수 있다. 만약 당신이 낯선 symbol이나 section mismatches때문에 빌드 에러를 얻었다면 간단히 DEBUG_CLFAGS로 부터 -g를 제거해라.


당신은 또한 GNU Makefile과 함께 쉽게 당신의 target을 정의할 수 있다. 알려지지 않은 target은 USE_POLL=default를 제외하고는 디폴트 옵션을 전달받지 못한다. 그래서 당신은  당신만의 옵션셋을 정의하기 위해 이 프로퍼티를 매우 잘 사용할 수 있다. USE_POLL은 USE_POLL=""을 설정함으로써 비활성화가 가능하다. 예를 들면
```
$ gmake TARGET=tiny USE_POLL="" TARGET_CFLAGS=-fomit-frame-pointer
```


1.1) Device Detection
-------------------------

HAProxy는 서드파티 제품에 의존되어있는 몇몇의 device detection 모듈을 지원한다. 그둘중 몇몇은 아마 무료 코드를 제공하며, 다른 이들은 무료 libs, 다른이들은 무료 평가 라이센스를 제공한다. 아래의 파일에서 그들의 자세한 사항에 대해 읽어보길 바란다.


    doc/DeviceAtlas-device-detection.txt   for DeviceAtlas
    doc/51Degrees-device-detection.txt     for 51Degrees
    doc/WURFL-device-detection.txt         for Scientiamobile WURFL


2) How to install it
-------------------------

haproxy를 설치하기 위해선, 당신은 single resulting binary를 원하는 위치에 복사하거나, 아래의 커맨드를 실행시킬 수 있다.
```
$ sudo make install
```
만약 당신이 다른 시스템을 위해 그것을 패키징한다면, 당신은 일반적인 DESTDIR 변수에 그것의 root directory를 명세할 수 있다. 


3) How to set it up
-------------------------
몇개의 문서가 doc/ 디렉토리에 있다.
- **intro.txt** : 이는 haproxy의 소개이며, 이는 haproxy가 무엇인지를 설명한다. 초보자나 시스템 업그레이드 계획을 하고 있을때 이를 다시 보는 것이 유용하다
- **architecture.txt** : 아키텍처 매뉴얼. 이는 매우 오래됬고 멋진 새로운 피처에 대해서 설명하지 않는다. 그러나 이것은 여전히 좋은 시작지점인데 당신이 뭘원하는지 알지만 그걸 어떻게 해야할지를 모를때.
- **configuration.txt** : configuration 매뉴얼. 이는 몇몇의 필수적인 HTTP basic 개념을 상기시키고, 모든 configuration file syntax(keywords, units)를 상세히 섬ㄹ명한다. 또한 log와 stats 포맷을 묘사한다. 그리고 이 문서는 항상 최신이다. 만약 당신이 여기에서 놓쳐진 무언가를 확인한다면 bug라고 보고해라. 이파일은 매우 거대하며 일반적으로 Cyril Bontés HTML translation online를 리뷰하는 것이 더 편할 것임을 알린다. [http://cbonte.github.io/haproxy-dconv/configuration-1.6.html](http://cbonte.github.io/haproxy-dconv/configuration-1.6.html)
- **management.txt** :어떻게 haproxy를 시작해야할 지, 어떻게 실행중에 관리를 해야할지, 어떻게 멀티 노드상에서 이걸 관리해야 할지, 어떻게 보이지않는 업그레이드와 함께 진행할 것인지에 대해 설명한다.
- **gpl.txt/ lgpl.txt** : 소프트웨어를 덮고있는 라이센스 카피. 더많은 정보를 원하면 상단의 LICENSE 파일을 확인하라.
- 그외 나머지는 주로 개발자들을 위한 것이다.

haproxy와 관련된 사이트나 기사뿐만 아니라 `example`에 수많은 좋은 configuration example이 존재한다.



4) How to report a bug
-----------------------------------------

때때로 당신은 버그를 발견할 수 있다. 버그는 당신이 봤지만 문서에는 없는 것이다. 그외에는 misdesign이 될 수도 있다. 만약 당신이 어떤 멍청하게 디자인된 것을 찾는다면그것을 list에서 토론해라.("how to contribute" 섹션을 참조). 만약 당신이 느끼기에 당신이 맡고 haproxy가 그걸 따르지 않는다면, 첫번째로 당신 스스로에게 물어봐라 그것을 아무도 이전에 이런 이슈를 등록하지 않았다는 것이 가능한 것인지. 만약 그렇지 않따면 당신이 아마 당신의 setup에 이슈를 가질 것이다. 의심의 여지없이 mailing list archives와 상담하라.

[http://marc.info/?l=haproxy](http://marc.info/?l=haproxy)

그외에는 이슈를 재생산하고 mailing list에 그것을 보내는데 돕기위해 최대한의 정보를 얻기위해 시도해라.

haproxy@formilux.org


당신의 configuration과 logs를 포함시키기를 바란다. 당신은 당신의 IP와 password를 가릴 수 있다.(우린 그것들을 원하지 않는다.) 그러나 만약 당신이 무슨 일이 일어나는지 사람들과 토론하기 위해선 당신의 config를 post하는 것은 필수적이다.)

또한, haproxy는 절대 CRASH되지 않게 설계되었다는 것을 명심해라. 만약 당신이 어떤 이유도 없이 haproxy가 죽는 것을 봤다면 그것은 무조건 보고되어야하며 긴급으로 수정되어야하는 치명적인 버그이다. 이것은 과거에 몇번, 새로운 아키텍처상에서 실행되는 development version에서 있었다. 만약 당신의 setup이 매우 흔하다고 생각한다면 이런 이슈들이 전혀 관련이 없을 수도 있다. 어쨌든, 만약 무슨 일이 발생한다면 나에게 직접 연락하는 것을 꺼리지마라. 왜냐하면 내가 당신에게 어떻게 사용가능한 코어파일을 수집하는지에 대한 몇몇의 지침을 줄것이고 당신이 list상에서 공유하기를 원하지않는 다른 캡쳐가 있는지 물어볼 것이기 때문이다.


5) How to contribute
-----------------------------
소스상의 CONTRIBUTING 파일을 상세히 읽어보길 바란다. 이것을 의무이다.


-- end


