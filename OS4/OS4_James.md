<img width="1981" alt="Screen Shot 2021-04-03 at 11 53 32 AM" src="https://user-images.githubusercontent.com/69072471/113498932-6cc78400-954c-11eb-89d1-5d8000335a6c.png">

- 최초의 프로세스는 운영체제에 의해 생성된다.

- 그 이후에는 이미 존재하는 프로세스가 다른 프로세스를 복제 생성한다.

  **부모 프로세스가 자식 프로세스를 생성한다**

  

### 프로세스가 수행되는 모델

---

- 부모와 자식이 공존하며 수행되는 모델(부모와 자식의 경쟁관계)
- 자식이 종료(terminate)될 때까지 부모가 기다리는 `(wait) 모델`



<img width="1962" alt="Screen Shot 2021-04-03 at 11 58 33 AM" src="https://user-images.githubusercontent.com/69072471/113498934-7224ce80-954c-11eb-8882-c1465451c067.png">

- 프로세스 세계에서는 자식이 먼저 죽고, 이에 대한 처리는 부모 프로세스가 담당하게 된다.



<img width="1982" alt="Screen Shot 2021-04-04 at 3 18 25 AM" src="https://user-images.githubusercontent.com/69072471/113498941-76e98280-954c-11eb-9d53-1ff85b0d4457.png">



<img width="1965" alt="Screen Shot 2021-04-03 at 12 09 25 PM" src="https://user-images.githubusercontent.com/69072471/113498935-73ee9200-954c-11eb-81cf-b82efa57595e.png">

- 자식 프로세스는 부모의 fork system call에 의해 생성된다.

- 자식 프로세스는 부모와는 별도의 주소 공간을 가지게 된다.
- fork() 시스템 콜을 통해 부모의 PID 또는 Process ID를 제외한 모든 정보(운영체제 커널 내의 정보와 주소공간의 정보)를 모두 그대로 복사한다.

- fork()를 하게 되면 자녀 프로세스는 부모와 똑같은 코드를 갖고 있으나 main함수 처음부터 실행하지 않는다. 자녀 클래스는 fork()된 이후 시점부터 다음 코드를 실행한다. [무한 포크 방지]



#### Copy-on-write (COW)기법

write가 발생했을 때 copy를 하겠다. write 즉 내용이 바뀔 때 새로운 것을 만들고 그 전까지는 부모데이터를 공유한다. 그런데 내용이 수정되면 그 때 부모 프로세스의 code, data, stack을 copy해서 자신의 고유 데이터로 만든다. 메모리를 잘게 쪼개서 필요한 부분만 독립적으로 갖게 된다.



#### fork() system call의 메커니즘

- 운영체제 fork() 시스템 콜 제공
- 프로세스 fork() 시스템 콜 → CPU의 제어권이 운영체제 커널에게 넘어감
- 커널은 시스템콜을 한 프로세스를 복사함 → 자식 프로세스 생성



#### 자식 프로세스의 특징

- 부모 프로세스와 모든 문맥을 동일하게 가지고 있다
  - 주소공간
  - 프로그램 카운터
  - 레지스터 상태
  - PCB
  - 커널 스택
- 아들 또는 딸과 같은 개념이 아니라 자기자신을 원본이라 생각하는 복제인간이라 보면 된다.
- Fork() 함수의 결괏값으로 복제본은 0을 갖게된다 (원본은 양수)



<img width="1964" alt="Screen Shot 2021-04-04 at 3 05 08 AM" src="https://user-images.githubusercontent.com/69072471/113498936-751fbf00-954c-11eb-93ac-f8f306c6fdfb.png">

부모 프로세스와 자식 프로세스는 동일한 코드의 내용을 갖게 되는데 자식 프로세스에게 부모와 다른 독자적인 프로그램을 수행하도록 시킬 수 있는 메커니즘이 필요하고 이는 `exec()` 시스템콜에 의해 가능해 진다.

`exec()`시스템콜을 사용하면 프로세스가 지금까지 수행했던 상태를 다 잊어버리고 그 주소 공간을 완전히 새로운 프로그램으로 덮어씌운 후 새로운 프로그램의 첫 부분부터 다시 실행을 시작하도록 하는 시스템 콜이다.



`exec()` 시스템콜 또한 사용자 프로세스가 직접 수행할 수 없고 운영체제에 요청하는 시스템 콜을 통해서만 수행이 가능하다.



#### 새로운 프로그램 수행 메커니즘

- fork()를 통해 운영체제 커널이 프로세스를 복제한다.
- exec()를 통해 운영체제 커널이 새로운 프로그램을 자식 프로세스의 주소 공간에 덮어 씌운다.





<img width="1950" alt="Screen Shot 2021-04-04 at 3 11 39 AM" src="https://user-images.githubusercontent.com/69072471/113498939-75b85580-954c-11eb-989e-7b64e794ec9e.png">



<img width="1964" alt="Screen Shot 2021-04-04 at 3 15 33 AM" src="https://user-images.githubusercontent.com/69072471/113498940-7650ec00-954c-11eb-8749-1534f1d88876.png">



기본적으로 프로세스는 독립적으로 수행된다.

<img width="1959" alt="Screen Shot 2021-04-04 at 3 19 13 AM" src="https://user-images.githubusercontent.com/69072471/113498942-77821900-954c-11eb-87f1-270f09b63cad.png">

그런데 경우에 따라서는 독립적인 프로세스들이 협력할 때 업무의 효율성이 증진될 수 있다. 부분적인 처리 결과나 정보를 공유할 수 있고, 처리 속도가 향상될 수 있는 등 여러가지 측면에서 효과적일 수 있기 때문이다.



이를 인지하고 운영체제는 프로세스간의 협력 메커니즘[IPC]을 제공해 하나의 프로세스가 다른 프로세스의 수행에 영향을 미칠 수 있게 한다.

IPC의 대표적인 방법으로 메시지 전달(Message Passing) 그리고 공유메모리(Shared Memory)방식이 있다.

### 메시지 전달 방식

<img width="1966" alt="Screen Shot 2021-04-04 at 3 21 10 AM" src="https://user-images.githubusercontent.com/69072471/113498944-781aaf80-954c-11eb-9477-ef08a874d84a.png">

- 공유데이터를 일체 사용하지 않는다.
- 커널을 통해 메시지 전달이 가능해 진다[특권명령]
  - 메시지 통신을 하는 시스템은 커널에 의해 send와 receive message라는 두 가지 연산을 제공받게 된다.
  - 이 연산을 통해 프로세스는 전달할 메시지를 운영체제에게 시스템 콜 방식으로 요청해 전달할 수 있게 된다.
  - 프로세스들은 커뮤니케이션 링크를 생성한 후 send()와 receive()를 이용해 메시지를 주고받게 된다.

#### 메시지 전달 통신 방식

내부 구현은 커널의 중재에 의해 동일한 방식으로 이뤄지나 연산 인터페이스의 존재한다.

- 직접통신(Direct Communication)
  - 프로세스의 이름을 명시적으로 표시한다.
- 간접통신(Indirect communication)
  - 메시지를 메일박스(mail box) 또는 포터(port)로 부터 전달 받는다.
  - 각 메일박스는 고유의 ID가 있고 간접통신에서 사용되는 커뮤니케이션 링크는 프로세스간에 메일박스를 공유하는 경우에만 생성된다.



### 공유 메모리 방식

<img width="1944" alt="Screen Shot 2021-04-04 at 3 22 35 AM" src="https://user-images.githubusercontent.com/69072471/113498946-78b34600-954c-11eb-97eb-63dd720507f0.png">

- 공유메모리 방식에서는 프로세스들이 주소 공간의 일부를 공유한다.

*운영체제가 공유메모리를 사용하는 시스템 콜을 지원하기에 이렇게 주소 공간 일부를 공유하는 것이 가능해 진다.*



- 공유 메모리 영역은 여러 프로세스가 읽고 쓰는 것이 가능 해 진다.



#### 공유 메모리 방식의 특징

shared memory는 프로세스끼리 직접적으로 메모리를 공유할 수 있는 것이 아니라 운영체제 kernel에게 메모리를 쓰기위해 시스템 콜을 하고 물리적 메모리에 매핑을 하면 share가 가능해 지고 이 영역은 공유가 가능 해 진다. 공유 공간이 생기는 순간 운영체제의 간섭 없이 메모리를 공유하며 서로에게 메세지를 보낼 수 있게 된다. 따라서 서로의 데이터를 간섭하는 문제가 생길 수 있고 이에 대해 커널이 책임지지 않는다. 따라서 메모리를 공유하는 프로세스는 모두 신뢰할 수 있는 프로세스여야 한다.