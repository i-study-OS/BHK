운영체제 



### 운영체제란 무엇인가?

---

*컴퓨터 하드웨어 바로 위에 설치되어 사용자 및 다른 모든 소프트웨어와 하드웨어를 연결하는 소프트웨어 계층*

협의의 운영체제: 커널

- 운영체제의 핵심부분으로 메모리에 상주하는 부분

광의의 운영체제

- 커널 뿐 아니라 각종 주변 시스템 유틸리티를 포함한 개념



### 운영체제의 목적

---

1. 컴퓨터 시스템을 편리하게 사용할 수 있는 환경 제공
   - 동시 사용자/ 프로그램이 각각 독자적 컴퓨터에서 수행되는 것 같은 환상을 제공
     - 실행 중인 프로그램들에게 짧은 시간씩 CPU를 번갈아 할당
   - 하드웨어를 다루는 복잡한 부분을 운영체제가 대행
2. 컴퓨터 시스템의 자원을 효율적으로 관리
   - Resource 효율적 관리 [프로세서, 기억장치, 입출력 장치 등등]
     - 사용자간 형평성 있는 자원 분배
     - 주어진 자원으로 최대한 성능을 낼 수 있게 관리
   - 사용자 및 운영체제 자신을 보호
   - 프로세스, 파일, 메시지 등 소프트웨어 자원까지 관리



### 운영체제의 분류

---

#### 동시작업 가능 여부

- 단일 작업 [single tasking]
  - 한 번에 하나의 작업만 처리
    - MS-DOS 한 명령의 수행을 끝내기 전에 다른 명령을 수행시킬 수 없었다.
    - 설계하기가 비교적 수월하다
- 다중 작업 [multi tasking]
  - 동시에 두 개 이상의 작업 처리
    - UNIX, Windows 등 한 명령의 수행이 끝나기 전 다른 명령이나 프로그램을 수행할 수 있다.

#### 사용자의 수

- 단일 사용자[single user]
  - MS DOS, MS Windows, iOS
- 다중 사용자[multi user]
  - UNIX, NT server

#### 처리방식

- 일괄처리 [batch processing]

  - 작업 요청의 일정량을 모아서 한꺼번에 처리
  - 작업이 완전 종료될 때 까지 기다려야 한다.
    - 초기 Punch Card 처리 시스템
  - non-interactive system

- 시분할[time sharing]

  - 여러 작업을 수행할 때 컴퓨터 처리 능력을 일정한 시간 단위로 분할하여 사용
  - 일괄 처리 시스템에 비해 **짧은 응답 시간**을 가짐
  - cpu를 아주 짧은 시간 단위로 분할하여  여러 작업을 수행
  - 범용 컴퓨터에서 사용됨

- 실시간[Real time]

  - 정해진  시간 안에 어떠한 일이 반드시 종료됨이 보장되어야 하는 실시간 시스템을 운영체제

  - 특수한 목적을 가진 시스템에서 사용된다[정확도가 중요한 장비에서 사용된다.

    - 원자로/공장 제어, 미사일 제어, 반도체 장비, 로보트 제어 등등

  - 실시간 시스템의 개념 확장

    - Hard realtime system [경성 실시간 시스템]

      ㄴ 데드라인을 정확히 지키지 않으면 치명적인 결과를 도래하는 것:

      반도체, 미사일 등등

    - Soft realtime system [연성 실시간 시스템]

      ㄴ 데드라인은 존재하나 지키지 않아도 비교적 치명적이지 않은 결과를 도래하는 것:

      영화, 넷플릭스 등등

  Q1 컴퓨터에서 영화를 볼 수 있으니 그럼 우리가 사용하는 OS 또한 실시간 처리 방식의 운영체제라고 봐야 하나?

  A: No. 우리 컴퓨터 같은 경우 매우 많은 프로그램을 띄워놓을 경우 영화의 데드라인을 지키지 못하고 영화가 끊길 것이다. 이는 우리 컴퓨터가 실시간 처리방식 운영체제가 아니라는 것을 뜻한다. 그러나 점점 스마트폰, 컴퓨터에게 실시간 시스템을 요구하는 프로그램이 늘어나기 때문에 어떻게 이런 실시간 처리방식을 요구하 애플리케이션을 지원 해 주는 볌용 운영체제를 구축해야 하는지 연구 중에 있다.



현재 배울 운영체제는 `다중 작업 `, `다중 사용자`를 지원하고 `시분할` 방식으로 운영되고 있다.



### 몇 가지 용어

---

#### Multitasking

- task가 동시에 돌아간다고 볼 수도 있지만 엄밀히 말하면 CPU에서는 매 순간 하나의 프로그램만 돌아가고 있다. 이것을 짧은 시간으로 분할을 해서 할당을 해서 동시에 실행되는 것 처럼 보이이고 이 작업을 우리는 multitasking이라고 부른다.

#### Multiprogramming

​	메모리에 여러 프로그램이 동시에 올라가는 것을 뜻한다. 특별히 메모리 측면을 강조한 방식을 멀티프로그래밍이라고 설명한다.

#### Time sharing

동시에 실행하는 것을 Time sharing이라 부른다. CPU를 더 강조한 표현. CPU를 조금씩 쪼개서 여러 프로그램에게 할당을 하기 때문에 Time sharing도 multitasking과 유사한 용어지만 CPU를 더 강조한 용어이다.

#### Multiprocess

실행중인 프로그램이 동시에 실행된다는 의미



#### Multiprocessor

하나의 컴퓨터에 CPU processor가 여러 개 붙어 있음을 의미

한 순간에 여러 작업이 실행될 수 있다 [여러개의 CPU를 보유하고 있기 때문이다]



### 운영 체제의 Examples

---

#### 유닉스

만들어진 동기: 

*대형 컴퓨터를 위해 만들어진 운영체제

* 여러사람들이 서버에 접속해서 사용하는 서버를 위한 운영체제

- 코드커널이  대부분을 C언어로 작성
  - 기계어와 가까우면서도 기존의 고급언어와 달리 사용자가 이해하기 수월한 언어
- 높은 이식성
  - Portable하다. 즉 하나의 컴퓨터에서 돌아가는 Unix를 다른 컴퓨터에 이식하기가 쉽다. 이 역시 C언어로 만들어져 있기에 다른 컴퓨터에서 그냥 컴파일하면 되기에 가능하다.
- 최소한의 커널 구조
  - 커널의 크기가 매우 작게 설계되어있다. 왠만한 기능을 빼고 핵심적인 기능만 넣어놓았기 때문에 아래와 같은 장점이 있다.
- 복잡한 시스템에 맞게 확장 용이
- 소스 코드 공개
- 프로그램 개발에 용이
- 다양한 버전
  - system V, FreeBSD, Sun OS, Solaris
  - Linux
    - 공개 소프트웨어 정신에 기반해 만들어진 운영체제
    - 누구나 사용할 수 있다.
    - 대형 컴퓨터 뿐 아니라 개인용 PC에 설치해도 좋은 특징을 가지고 있다.

#### DPS [Disk Operating System]

- MS사 에서 1981년 IBC-PC를 위해 개발

- 단일 사용자용 운영체제
- 메모리 관리 능력의 한계 [주 기억 장치: 640KB가 limit]

#### MS Windows

- MS사의 다중 작업용 GUI 기반 운영 체제
- Plug and Play, 네트워크 환경 강화
- DOS용 응용 프로그램과 호환성 제공
- 불안정성 [초기 윈도우의 불안정성]
- 풍부한 지원 소프트웨어

#### Handheld device[소형 Device]를 위한 OS

- PalmOS, Pocket PC (WinCE), Tiny OS



***운영 체제는 점점 여러가지 형태로 변화하고 있다.***



### 운영 체제의 구조

---

[CPU, Memory, HardDisk, I/O device]

위 장치들을 관리하는 것이 운영체제의 역할

**CPU 스케줄링**: 누구한테 CPU를 줄까?

은행의 경우 고객이 가면 번호표를 뽑고 먼저 온 순서대로 처리를 해준다.

그런데 매우 복잡한 업무를 봐야하는 고객이 있을 경우 은행원이 한 명이면 그 한명 때문에 뒤에 온 고객들은 기다려야 한다. 은행원이 많으면 이런 불상사는 겪지 않아도 된다. 

물론 CPU의 속도는 매우 빠르다 그럼에도 CPU가 하나일 때 위와 같은 불상사를 겪을 수 있기 때문에 CPU 스케쥴링은 짧은 시간 간격으로 메모리를 줬다 뺏는 것을 반복적으로 한다.

물론 이론적으로 CPU를 짧게 필요로 하는 프로그램을 우선적으로 처리하는 것이 맞다. 이를 통해 평균적인 속도가 빨라지기 때문이다. 해당 이론에서 출발은 **하지만** 이 방법 보다는 여러 프로그램을 돌아가며 메모리를 짧게 짧게 할당해서 전체 프로그램을 돌리는 것이 더 효율적일 것 같다는 것에 입각해서 `Memory sharing` 방향으로 관리하기 시작하였다.



**메모리 관리**: 한정된 메모리를 어떻게 쪼개어 쓰지?

- 메모리를 얼만큼 할당해야 하는가에 대한 문제...

메모리를 분배하는 이슈에서는 프로그램이 많아지면 각각이 갖는 메모리 공간이 작아지기 때문에 성능이 하락된다. 그래서 메모리에 동시에 많은 프로그램을 올려 놓기 보다는 CPU가 원활하게 운영되기 위해서 필요한 양만큼 메모리를 할당하는 방식: Working set[메모리에 한꺼번에 올라가 있어야 하는 페이지들의 집합]

그때 그때 특정 프로그램에 메모리를 몰아주면 프로그램 속도를 최적화 시킬 수 있다.

- 어떤 프로그램을 메모리로 부터 쫒아내야 하는가에 대한 문제...

CPU에서 추후에 사용할 가능성이 낮은 것들을 쫒아내야 하는데 미래에 무엇을 사용할지 컴퓨터는 알 수 없다. 따라서 미래를 예측해야 한다.

컴퓨터는 미래를 과거데이터를 토대로 미래를 예측한다. 최근에 많이 사용된 프로그램을 추후에도 많이 사용하겠구나 예측해서 메모리에 프로그램을 남겨두고 그 외 프로그램은 Disk로 쫒아낸다. [Working Set에 올리는 작업]



**파일관리**: 디스크에 파일을 어떻게 보관하지?

디스크라는 것은 head가 움직이면서 데이터를 쓰고 읽기 때문에 head에 가까이 들어온 요청이 있다면 나중에 요청이 들어온 것임에도 먼저 해당 요청을 듣는다. 이렇게 함으로써 디스크의 움직임을 최소화 할 수 있다.

엘레베이터 예시: 1층 사용자 버튼, 100층 사용자 버튼, 2층 사용자 버튼 이 순서대로 눌렀다 가정했을 때 먼저 들어온 순서대로 1층 갔다 100층 갔다 이렇게 작동하지 않고 1층에서 손님을 받고 올라가면서 2층에서 손님을 받는다.

디스크는 head 자체가 움직이면서 사용자의 입력을 처리해야하기 때문에 움직임을 최소화 하면서 일을 빨리 처리할 수 있는 방식으로 작동한다.



**입출력 관리**: 각기 다른 입출력장치와 컴퓨터 간에 어떻게 정보를 주고 받게 하지?

I/O device 장치는 비교적 느리다 이렇게 느린장치와 정보를 어떻게 주고 받나?

interrupt에 기반하여 정보를 처리한다. 

CPU는 평소에 I/O 장치에 관여하지 않는다. 그러다 I/O 장치가 interrupt를 걸 때 CPU는 다음 작업 하기 전에 io 장치를 처리해주므로서 최대한 CPU가 방해받지 않게 한다.

프로세스 관리: 

- 프로세스의 생성과 삭제
- 자원 할당 및 반환
- 프로세스 간 협력

그외

- 보호 시스템
- 네트워킹
- 명어 해석기

#### *인간의 신체가 뇌의 통제를 받듯 컴퓨터 하드웨어는 운영체제의 통제를 받으며 그 운영체제는 사람이 프로그래밍하는 것이다.*

