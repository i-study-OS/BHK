# 프로세스의 개념

프로세스는 프로그램이 실행 중인 상태

## 프로세서의 문맥

특정 시점을 놓고 봤을 때 어디까지 수행했는지

현대 컴퓨터의 cpu는 시분할 방식으로 동작하기 때문에 각 프로세스의 문맥을 알고 있어야 프로세스가 cpu 자원을 얻었을 때 하던 일을 다시 실행 할 수 있다.

# 프로세스의 상태

우리는 cpu가 하나인 컴터만 고려한다.

## Running

- 프로세스가 cpu를 잡아서 수행중인 상태

## Ready

- cpu를 기다리는 상태
- 다른 조건은 다 만족이 된 상태

## Blocked(wait, sleep)

- 오래 걸리는 일을 하고 있는 프로세스의 상태
- 그래서 cpu를 잡아도 일을 하지 못하는 상태
- 예로 IO를 하고 있는 프로세스

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/53a2f215-3dd2-4d71-a2d8-1f59ab72d64a/_2021-03-27__5.35.11.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/53a2f215-3dd2-4d71-a2d8-1f59ab72d64a/_2021-03-27__5.35.11.png)

- 마치 놀이동산에서 줄서는 것과 비슷하다
    - cpu는 빠르고 타는 시간이 짧은 놀이기구
    - 그에 비해 io 장치들은 cpu 보다 백만배 느림.

# Process Control Block

운영체제가 각 프로세스를 관리하기 위해 정보들을 저장하는 곳

담긴 정보들

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/12ebd845-5113-4e71-ae49-8510bc2917b7/_2021-03-29__10.04.40.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/12ebd845-5113-4e71-ae49-8510bc2917b7/_2021-03-29__10.04.40.png)

# 문맥 교환(context switch)

- cpu를 한 프로세스에서 다른 프로세스로 넘겨주는 과정
- cpu를 뺏기는 프로세스는 cpu의 상태를 자기의 pcb에 저장
- cpu를 새롭게 얻는 프로세스는 그 프로세스의 상태를 pcb에서 읽어와서 수행

> 시스템 콜이나 인터럽트가 되면 cpu를 운영체제에게 넘겨주는데 이 행위는 문맥 교환은 아니다

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d016f2ac-55fd-40b0-bb79-2559d193963c/_2021-03-27__5.55.01.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d016f2ac-55fd-40b0-bb79-2559d193963c/_2021-03-27__5.55.01.png)

 

# 프로세스를 스케줄링하기 위한 큐

### ready Queue

cpu를 기다리는 프로세스들을 담아두는 큐

### Device Queues

io 장치들에 처리를 기다리는 프로세스를 담아두는 큐

### Job Queue

현재 시스템 내에 있는 모든 프로세스의 집합

레디큐와 디바이스의 큐에 있는 프로세스들은 모두 잡 큐에 있음

# 스케줄러

순서를 정해주는 애(우리가 하루의 스케줄을 스케줄러에 적듯이)

- 각각의 자원 별로 할일을 스케줄 해주는 것

## Short-term scheduler(단기 스케줄러, cpu 스케줄러)

- 다음 번에 어떤 프로세스에게 cpu를 줄지 스케줄 해주는 애

## Long-term scheduler(장기 스케줄러,  잡 스케줄러)

- 어떤 프로세스가 생성(new 상태)에 있는데 메모리를 줄지 안 줄지 스케줄 하는 애
- 여러 프로그램이 메모리에 올라가는 것을 `degree of multiprogramming` 이라고 함. → 이를 제어 함.
- 근데 시분할 시스템에는 보통 장기 스케줄러가 없음..
    - 프로세스가 생성되면 바로 레디 상태로 넘어감.

## Medium-term scheduler(중기 스케줄러, 스와퍼)

- 너무 많은 프로세스가 메모리에 올라가면 몇개는 쫒아내는 애
- 얘가 `degree of multiprogramming` 을 제어

> 현대 컴터 시스템에서는 중기 스케줄로 인해서 `Suspended` 상태도 있음.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5b414ad1-6e39-4af0-9fd9-03c77241b91f/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5b414ad1-6e39-4af0-9fd9-03c77241b91f/Untitled.png)

## Suspended(stopped)

- 외부에서 강제로 정지 시킨 상태
- 메모리에서 프로세스가 다 쫓겨나서 swap out 된 상태

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8b84a6d2-6fee-4326-8444-deb46ba8c0ca/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8b84a6d2-6fee-4326-8444-deb46ba8c0ca/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/419bcdf1-1189-41d9-b47b-40385e4caeb3/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/419bcdf1-1189-41d9-b47b-40385e4caeb3/Untitled.png)

- running(user mode) → 사용자의 코드를 실행하고 있는 상태
- running(monitor mode(커널 모드)) → 운영체제 코드를 실행하고 있는 상태
- active → 메모리에 올라가 있는 상태
- inactive → 메모리에서 내려가 있는 상테

# 쓰레드

- 다른 말로 lightweight process
- 프로세스 내부에 cpu 실행 단위가 여러개가 있는 경우
- cpu를 수행하는 단위

### 각 쓰레드가 가지고 있는 것들

- Program Counter
- register set
- stack space

### 쓰레드가 공유하는 것들

- code section
- data section
- OS resources

## 쓰레드의 장점

- 쓰레드 하나가 블럭 상태 일때 다른 쓰레드가 cpu를 잡고 러닝 상태가 되어서 응답시간을 빠르게 할 수 있다.(응답성)
    - 예) 서버에서 페이지를 불러 올 때 여러 쓰레드를 둬서 텍스트는 먼저 읽어 오는데로 화면에 보여주고 그동안 다른 쓰레드들은 사진이나 영상을 가져오는..
- 프로세스를 여러개 만들면 차지하는 메모리가 많아지지만 같은 일을 하는 경우에 쓰레드만 늘리면 메모리의 효율과 성능 높일 수 있다.(자원의 공유)
- 프로세스를 만드는 것와 쓰레드를 만드는 것, 프로세스를 전환하는 것과 쓰레드를 전환하는 것은 오버헤드(시간)가 많이 차이가 난다(경제성)
- cpu가 여러개 일때 쓰레드가 병렬적으로 일을 할 수 있다.

## 쓰레드의 구현 방식

### 커널 쓰레드

- 커널이 알고 있는 쓰레드

### 유저쓰레드

- 라이브러리로 프로세스 안에서 만들어지는 쓰레드
- 커널이 모름(하나의 프로세스로 인식)
- C언어 Pthreads
