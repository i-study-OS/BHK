# 운영체제의 정의
<br/>

<img width="450" alt="991DB74C5ADAD4BA1A" src="https://user-images.githubusercontent.com/67148595/111065885-776b9c00-84ff-11eb-9d8a-bce5ae1360ba.png">


- ### 좁은 의미의 운영체제

  일반적으로 커널만을 의미, 전공자가 말하는 운영체제는 보통 좁은 의미의 운영체제

  

- ### 넓은 의미의 운영체제

  커널뿐만 아닌 여러 주변 시스템을 다루는 시스템 유틸리티까지도 포함한 개념

  ex) 윈도우를 설치하면 커널만 설치되는게 아니라 다른 부가적인 시스템이 설치됨 

  이렇게 항상 메모리에 상주하지는 않지만 최소로 필요한 유틸리티를 포함하는 개념
  
  
  
  <br/>
  
  ---

# 운영체제가 하는 일
<br/>
<img width="450" alt="9996723F5AD2EDD424" src="https://user-images.githubusercontent.com/67148595/111065892-85212180-84ff-11eb-9648-7f33acfb002c.png">


- ### 컴퓨터 시스템 사용환경 편의 제공
  
  - 서로 다른 시스템을 동시에 실행할 수 있도록 해줌 (메모리 관리, 프로그램 충돌 조율)
  - 하드웨어 자원을 어떻게 분배해서 쓰는지 사용자가 몰라도 이용할 수 있게 해줌

<img width="450" alt="99A4FD4D5ADAD58818" src="https://user-images.githubusercontent.com/67148595/111066010-2c9e5400-8500-11eb-9f17-b7d53f99ba53.png">

- ### 자원의 효율적 관리
  
  - 운영체제의 목표 : 주어진 자원으로 최대한의 성능을 내는 것
  - 하드웨어를 효율적으로 관리해줌
  - CPU, 메모리, I/O 장치 등을 효율적으로 관리
  - 효율성만 강조하면 특정 프로그램 혹은 특정  사용자가 차별받는 상황이 생길 수도 있음. 이를 고려하여 형평성 있는 자원 분배 또한 중요
  - 프로세스, 파일, 메시지와 같은 소프트웨어 자원도 관리함
- ### 사용자나 운영체제 자신의 보호

<br/>

---


# 운영 체제의 분류
<br/>

### 동시 작업 가능 여부

<img width="450" alt="99723E4E5ADAD7B917" src="https://user-images.githubusercontent.com/67148595/111065910-98cc8800-84ff-11eb-93f0-b278f58e52b9.png">

  - 단일 작업 (ex. MS-DOS)

    특수한 경우를 제외하곤 현대에 와서는 거의 쓰지 않음. 설계하기 쉽다는 장점이 있음

  - 다중 작업

    UNIX, MS Windows 등의 환경
    

<br/>    

### 사용자의 수

<img width="450" alt="99ED994B5ADAD8711A" src="https://user-images.githubusercontent.com/67148595/111065942-c0235500-84ff-11eb-88b9-2e8ee62d8fa9.png">


  - 단일 사용자만 지원

    MS-DOS, MS Windows는 기본적으로 단일 사용자만 지원

    (Personal Computer에서도 계정을 여러개 만들 수 있어서 경계가 모호해짐)

  - 다중 사용자도 지원

    UNIX, NT server, Linux server 등에서는 여러 계정이 동시에 접근하는 것을 지원

<br/>    

### 처리 방식

<img width="450" alt="99C62D4F5ADAD95327" src="https://user-images.githubusercontent.com/67148595/111065953-d4ffe880-84ff-11eb-9e9f-670b974c671d.png">


  - #### 일괄 처리

    현재는 사용하지 않는 방식

    작업을 요청하면 일정량의 작업을 모아 한꺼번에 처리해주는 처리 방식.

    interactive하지 않았기 때문에 오류 없는 정확한 프로그래밍이 중요했었음

<img width="450" alt="99F8E4455ADAD9A42A" src="https://user-images.githubusercontent.com/67148595/111065948-ca455380-84ff-11eb-8adf-7e23bcb289cc.png">

<br/>

  - #### 시분할

    각각의 사용자는 매우 짧은 응답시간을 통해 전달되는 정보를 통해 interactive한 환경을 제공받음.

    다만 시간의 제약조건(데드라인)이 없기에 사용자가 많아지면 느려지기도 함

    주어진 자원을 최대한 활용하는 것이 목적

<img width="450" alt="99F1A6435ADADAD536" src="https://user-images.githubusercontent.com/67148595/111065949-cdd8da80-84ff-11eb-8425-05079bbb2029.png">

<br/>

  - #### 실시간

    정해진 시간안에 일이 반드시 종료됨

    미사일 제어, 반도체 공장 제어 등 미세한 시간차도 유의미한 상황에서 사용됨

<br/>  

### ⭐️ Soft realtime system ⭐️

실시간 시스템 개념의 확장

데드라인을 정확히 지키지 않아도 됨

<br/>

---


# 유사한 용어 정리
<br/>

<img width="450" alt="99A6D3445ADADECF09" src="https://user-images.githubusercontent.com/67148595/111065968-e77a2200-84ff-11eb-8cdc-647be38a38c3.png">

### Multitasking

일반적으로 사용되는 용어

컴퓨터에는 여러 task가 돌아가고 있지만 CPU에서는 매 순간 하나의 작업만이 실행되고 있음

매우 짧은 시간 간격에 하나의 작업을 할당하는 행동을 반복하여 동시에 실행되는 것처럼 보이게 해줌

### Multiprogramming

메모리에 여러 프로그램이 동시에 올라가는 방식

### Time sharing

시간을 분할해서 쓰는 방식 (CPU를 강조한 용어)

### Multiprocess

실행중인 프로그램이 동시에 실행된다는 의미의 용어



### ❗️주의할 용어 : Multiprocessor ❗️

CPU가 여러개 있는 컴퓨터를 의미

한 순간에 여러 작업이 실행될 수 있음 (CPU가 여러개이기 때문)

위 4개의 용어와는 하드웨어적으로 다른 시스템! (고성능 컴퓨팅에서 사용됨)


<br/>

---


# 운영 체제의 예
<br/>

<img width="450" alt="99C2C2345ADAE88419" src="https://user-images.githubusercontent.com/67148595/111066031-48a1f580-8500-11eb-841a-dd855e150980.png">

### 유닉스

- Multitasking 지원

- 유닉스를 만들기 위해 C가 탄생

- 초창기에는 오픈 소스였음 (유닉스에서 파생된 리눅스는 현재도 오픈소스)

- 특정 컴퓨터의 기계어에만 국한되게 만들지 않고 C로 만들어졌기에 이식성과 확장성이 좋음

- 커널의 크기를 최소한으로 만들어 메모리를 최대한 적게 사용

  

  <img width="450" alt="99BFBB335ADAE8930D" src="https://user-images.githubusercontent.com/67148595/111065972-ee089980-84ff-11eb-9ddf-81db4ef8d287.png">

### 윈도우

- Personal Computer을 위해 만들어짐
- 단일 사용자, 단일 작업만 지원하는 DOS에서 시작됨
- DOS의 경우 하드웨어의 발전속도를 예측하지 못하고 만들어져서 현재는 버려짐
- 현재는 DOS 위에서 윈도우즈를 실행하는 환경에서 벗어나 윈도우즈가 독자적인 운영체제가 되었음


<br/>

---

# 운영 체제의 구조
<br/>
<img width="450" alt="998203355ADAEEC315" src="https://user-images.githubusercontent.com/67148595/111065977-f365e400-84ff-11eb-9631-9216ee7ca0cf.png">

### CPU 스케쥴링

- CPU는 매우 빠른 장치
- 우선 요청순 처리방식이 아닌 task간의 시간 차이를 완충하면서 주어진 자원의 특성을 최대한 끌어올리는 방식
- 어떻게하면 매우 빠른 CPU의 성능을 최대한으로 끌어올릴 수 있을까? 를 해결해주는 것이 CPU 스케쥴링

### 디스크 스케쥴링

- 디스크 헤드자체가 움직이면서 일을 처리해야 함
- 어떻게하면 디스크 헤드의 움직임을 최소화 할 수 있을지가 관건

### I/O device의 관리

- 장치가 CPU에 비해 매우 느리기 때문에 CPU와의 정보교환을 어떻게 할지가 관건
- interrupt 방식에 기반하여 관리하고 있음

<br/>

---

# 수강 태도
<img width="450" alt="99CEE1355ADAEEFA10" src="https://user-images.githubusercontent.com/67148595/111066067-75560d00-8500-11eb-88a2-7b9049031ad7.png">

- 자신이 운영체제가 되어 효율적인 행동이 무엇일지 생각해보기 (대부분의 알고리즘은 OS 자체의 내용)
