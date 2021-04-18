# Process Synchronization

---

데이터는 어떻게 접근하는가?

<img width="1969" alt="Screen Shot 2021-04-11 at 6 01 12 PM" src="https://user-images.githubusercontent.com/69072471/114298140-e84aa780-9aef-11eb-8682-03039117c680.png">

Memory에  저장된 데이터를  CPU,로 불러와 연산을 진행 한 뒤 뒤 원래 위치에 반영하는 방식으로 **데이터를 접근**한다.

이 작업 중 synchronization(동기화) 문제가 발생할 수 있다.

데이터를 읽어온 뒤 연산을 한 뒤 저장하는 방식에서는 누가 먼저 읽어왔느냐 에 따라 다른 결과를 야기할 수 있기 때문이다.

![Screen Shot 2021-04-11 at 6.06.28 PM](/Users/hwang-in-u/Library/Application Support/typora-user-images/Screen Shot 2021-04-11 at 6.06.28 PM.png)

여러 주체가 하나의 데이터를 접근하려 할하는 것을 Race condition(경쟁 상태)라 부르고 이를 조율 해 주는 장치가 필요하다.

문제가 발생할 수 있는 상황:

- CPU가 여러개 있는 시스템에서 여러 CPU가 메모리에서 데이터를 접근할 때 
- 공유 메모리를 사용하는 프로세스간의 경쟁 시
- 커널 내부 데이터를 접근하는 루틴들 간의 경쟁



<img width="1980" alt="Screen Shot 2021-04-11 at 6 40 15 PM" src="https://user-images.githubusercontent.com/69072471/114299282-5cd41500-9af5-11eb-86be-411d85e91ec8.png">



<img width="1987" alt="Screen Shot 2021-04-11 at 6 13 19 PM" src="https://user-images.githubusercontent.com/69072471/114298480-99057680-9af1-11eb-950e-0b41babfe081.png">

Case 1

<img width="1982" alt="Screen Shot 2021-04-11 at 6 14 31 PM" src="https://user-images.githubusercontent.com/69072471/114298523-c3efca80-9af1-11eb-87ab-7aab2f8444f3.png">

위 예시에 대한 자세한 설명:

커널이 CPU에서 실행을 하면서 count라는 고급언어의 변수값을 1 증가시키고 있다. 이 고급언어는 load, increase, store와 같은 여러 개의 instruction으로 나뉘어져서 실행 되는데 이 변수를 CPU로 load한 상태에서 interrupt가 들어왔을 경우에 이 작업을 잠시 멈추고 interrupt처리 루틴으로 넘어가게 된다면

interrupt또한 커널의 코드이기에 interrupt handler가 처리되는 도중에 커널의 데이터인 count라는 것을 값에서 1을 뺄 수도 있다. 위 예시에서는 변수의 값을 1감소 뒤에 다시 원래 상태로 돌아가게 된다. interrupt가 들어오기 전에 context를 저장할 때 load 까지 한 부분으로 다시 돌아가 그냥 단순히 해당 변수를 increase한 뒤 memory에 저장을 한다.

그런데 이 count변수를 1 증가 시킨 뒤 interrupt처리에서 다시 해당 변수 값을 1 감소 시켜주었으니 상태의 변화가 없어야 하는데. 결과적으로는 1 감소 시키는 interrupt는 context에 저장이 되어있지 않으니 1증가 한 변수 값만 저장하게 된다.

커널에서는 위와 같은 문제가 생길 수 있다.

이 문제를 해결하기 위해서:

커널에서 중요한 정보를 연산하는 경우count++ 가 끝나기 전 까지는 interrupt처리를 하지 않는다. 즉 interrupt를 `disable` 시켰다가 count연산이 끝난 뒤 interrupt handler에게 넘겨주는 방식으로 해결하고 있다.



Case 2

<img width="1980" alt="Screen Shot 2021-04-11 at 6 25 37 PM" src="https://user-images.githubusercontent.com/69072471/114298852-517fea00-9af3-11eb-81c4-8e4d937fa491.png">

프로세스는 본인의 코드를 실행하는 것 뿐 아니라 자기가 접근할 수 없는 부분을 처리하기 위해서 System call을 통해서 운영체제에게 요청을 하는 경우가 있다. 그래서 프로그램은 여러 모드를 번갈아가면서 사용한다.

<img width="1979" alt="Screen Shot 2021-04-11 at 6 30 23 PM" src="https://user-images.githubusercontent.com/69072471/114298986-fb5f7680-9af3-11eb-9e75-90cec04b4c34.png">

Case 1과 비슷한 상황이다. Usermode에서 Kernelmode로 전환이 되면서 1증가시키는 도중 할당시간이 끝나서 다른 프로세스에게 넘어 간 뒤 다시 1을 증가시키는 도중 할당시간이 끝나서 원래의 프로그램으로 돌아가게 되면 우리의 생각으로는 총 2가 증가 되어야 하는데 증가되기 전에 count값을 가진 context를 가지고 저장을 했기 때문에 synchronize문제가 생겨난다.

이를 해결하기 위해서 프로세스가 커너모드에 있을 때는 할당시간이 끝나도 CPU를 뺏기지 않도록 한다. 커널모드가 끝나고 유저모드로 돌아갈 때 CPU를 뺏는다. 할당시간이 완벽히 지켜지지 않지만 time sharing 시스템에서는 이런 문제가 시스템에 큰 문제로 이어지지 않는다.



Case 3: CPU가 여러개 있는 상황

<img width="1969" alt="Screen Shot 2021-04-11 at 6 34 21 PM" src="https://user-images.githubusercontent.com/69072471/114299099-89d3f800-9af4-11eb-8cca-5b2234025cd5.png">

- interrupt를 잠시 block한다고 해결되지 않음. 하나의 CPU가 다른 프로세스로 넘어가면서 생기는 문제를 방지하는 방법인데 멀티프로세서인 경우  작업 주체가 여럿이 있으니까 생기는 문제이기에

RaceCondition 방지 방법:

1. 커널을 접근할 때 커널 전체에 lock을 걸어야 한다. 특정 데이터를 들고가기 전에 해당 데이터에 lock을 걸어서 다른 CPU가 접근하지 못하게 한 뒤 연산작업이 마친 뒤에 lock을 풀어주는 방법
2. 운영체제 커널의 데이터에 여러 CPU가 접근하며 생기는 문제니까 커널 내부에 있는 각각의 데이터에 매 순간 한 CPU만 접근할 수 있도록 lock을 걸어주는 방법



**여러 프로세스가 shared data를 동시에 사용할 경우 생길 수 있는 문제**

<img width="1973" alt="Screen Shot 2021-04-11 at 6 46 48 PM" src="https://user-images.githubusercontent.com/69072471/114299452-47131f80-9af6-11eb-9e92-82201399f85a.png">

<img width="1956" alt="Screen Shot 2021-04-11 at 6 48 34 PM" src="https://user-images.githubusercontent.com/69072471/114299499-86417080-9af6-11eb-9974-d1c1311bbe8f.png">

<img width="1975" alt="Screen Shot 2021-04-11 at 6 49 16 PM" src="https://user-images.githubusercontent.com/69072471/114299514-9eb18b00-9af6-11eb-8f73-03abc47bcc1c.png">

공유 데이터를 접근하는 코드를 `Critical section`이라 부른다.

하나의 프로세스가 ciritical section에 있을 때 다른 프로세스는 critical section에 들어갈 수 없어야 한다.



이를 위한 해결법:

<img width="1978" alt="Screen Shot 2021-04-11 at 6 51 40 PM" src="https://user-images.githubusercontent.com/69072471/114299559-f51ec980-9af6-11eb-9a44-6562094c01d8.png">

<img width="1970" alt="Screen Shot 2021-04-11 at 6 52 16 PM" src="https://user-images.githubusercontent.com/69072471/114299573-09fb5d00-9af7-11eb-8597-749d95e8b8a9.png">

코드는 두 가지로 분류 될 수 있다:

- Critical Section(공유 데이터를 접근하는 코드)
- Non-critical Section(접근하지 않는 코드)

앞 뒤로 여러 프로세스가 critical section으로 접근하지 못하게 하는 코드를 적어 줌으로써 해결 할 수 있다.

<img width="1975" alt="Screen Shot 2021-04-11 at 6 53 46 PM" src="https://user-images.githubusercontent.com/69072471/114299612-3fa04600-9af7-11eb-8080-751df4efc03d.png">

- Mutual Exclusion 조건 만족
- Progress 조건 불만족: 아무도 critical section에 있지 않은 상태에서 critical section에 들어가게 해주어야 하는데 그러지 못하는 문제 발생:
  - 무조건 상대방이 turn을 바꿔주도록 코드가 설계되어있다.



<img width="471" alt="Screen Shot 2021-04-17 at 8 12 02 PM" src="https://user-images.githubusercontent.com/69072471/115111020-33b1f980-9fb9-11eb-9a44-615e964b62ed.png">

flag를 통해 자신이 critical section에 들어갈 것인지 말것인지 의중을 표현 가능. 다른 프로세스 flag표시가 true일 경우 다른 프로세스가 critical section을 들어간다고 인지하고 그 프로세스가 critical section에서 나오기 전까지 기다린다.

프로세스가 critical section을 빠져나오면 자신의 flag를 false로 바꿔준다.

- mutual exclusion 조건 만족
- Progress 조건 불만족: 프로세스가 flag를 true로 바꿨을 때 CPU를 뺏긴 후 다른 프로세스 또한 flag를 true로 바꿨을 때는 critical section에 아무도 없는데 flag가 true인 경우 때문에 아무 프로세스도 못 들어가는 문제 발생



<img width="471" alt="Screen Shot 2021-04-17 at 8 18 26 PM" src="https://user-images.githubusercontent.com/69072471/115111195-16315f80-9fba-11eb-80bf-d6518bcff02d.png">

Spin lock: while문을 돌면서 상대가 못 들어오게 막는 방법. 비효율적인 측면 존재



하드웨어적으로 critical section문제를 해결하는 방법

<img width="464" alt="Screen Shot 2021-04-17 at 8 25 42 PM" src="https://user-images.githubusercontent.com/69072471/115111384-1aaa4800-9fbb-11eb-9d79-f6ded6f651ab.png">

데이터를 읽고 쓰는 것을 하나의 instruction으로 처리할 수 있으면 소프트웨어적으로 복잡하게 해결하지 않고 문제 해결 가능. 



<img width="460" alt="Screen Shot 2021-04-17 at 9 08 45 PM" src="https://user-images.githubusercontent.com/69072471/115112576-1b45dd00-9fc1-11eb-83e2-862f439c4f95.png">



P연산: Semaphore 변수 값, 또는 어떤 공유 데이터를 획득하는 과정

V연산: 변수 값 또는 데이터를 사용한 뒤 반납하는 과정

Semaphore변수는 정수값을 가질 수 있고 자원의 갯수라 봐도 된다. 변수가 5면 자원이 5개 있는 것이다. 

P연산을 하면 해당 자원을 하나 가져가서 총 자원이 4개가 된다.

 자원의 갯수가 4개인 상태에서 V연산을 하면 총 자원이 5개가 된다.



Critical Section에 위에 언급햇던 Semaphore를 적용시켜보면...

semaphore의 변수 또는 semaphore mutual exclusion or mutex 변수를 1로 놓고 Critical Section에 들어갈 때는 P연산을 나올 때는 V연산을 해주면 Critical Section문제가 해결이 된다.

앞에서 처럼 프로그래머가 모든 코드를 작성하는 것이 아니라 구현하는 시스템에서 추상화 자료형 형태로 제공을 해 주면 프로그래머는 Semaphore를 통해서 프로그램을 작성하면 훨씬 간단한 프로그램을 작성할 수 있다.

<img width="464" alt="Screen Shot 2021-04-17 at 10 20 24 PM" src="https://user-images.githubusercontent.com/69072471/115114585-1d149e00-9fcb-11eb-8888-72717f26f409.png">

<img width="467" alt="Screen Shot 2021-04-17 at 10 27 48 PM" src="https://user-images.githubusercontent.com/69072471/115114813-26523a80-9fcc-11eb-9933-639f5ef657c7.png">

<img width="467" alt="Screen Shot 2021-04-17 at 10 29 31 PM" src="https://user-images.githubusercontent.com/69072471/115114864-69141280-9fcc-11eb-9699-b36aebea853c.png">

<img width="465" alt="Screen Shot 2021-04-17 at 10 38 37 PM" src="https://user-images.githubusercontent.com/69072471/115115148-a9c05b80-9fcd-11eb-87dc-471d9d4a5015.png">

일반적으로 block/wakeup이 낫다 하지만 Critical section 짧은 경우 busy/wakeup이 나을 수 있다.



<img width="466" alt="Screen Shot 2021-04-17 at 10 41 24 PM" src="https://user-images.githubusercontent.com/69072471/115115202-0c195c00-9fce-11eb-8347-064f2ad0ffc6.png">



Semaphore를 쓸 때 주의할 점:

<img width="468" alt="Screen Shot 2021-04-17 at 10 45 38 PM" src="https://user-images.githubusercontent.com/69072471/115115296-a5487280-9fce-11eb-95f4-2b67b76eb974.png">



Starvation Problem:

<img width="465" alt="Screen Shot 2021-04-17 at 10 51 24 PM" src="https://user-images.githubusercontent.com/69072471/115115436-7252ae80-9fcf-11eb-9364-23d49b21b3e7.png">