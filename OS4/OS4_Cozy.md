**프로세스와 관련한 시스템 콜**

\- fork()

\- exec()

\- wait()

\- abort()



### **프로세스 생성(Process Creation)**

부모 프로세스가 자식 프로세스를 생성한다 **(생성방법; 복제)**

부모 프로세스 하나가 자식 여러개를 생성할 수 있기 때문에 트리(계층 구조) 형성

 

프로세스는 자원을 필요로 함

\- 운영체제로부터 받는다

\- 부모와 공유한다 : 경우에 따라 자식 프로세스와 자원을 공유하기도 하지만, 자식 또한 별도의 프로세스이기 때문에 서로 많은 메모리 공유와, CPU를 할당 받기 위해 경쟁한다.

\- 자식이 종료될 때까지 부모가 기다린다.(wait system call) 

 

**주소 공간(Address space)**

기존의 무언가가 있으면 작업이 편하니까 필요한 것만 덮어씌우는 방식을 채택한다. 

ex. 과목마다 보고서 쓸 때 만들어 놓았던 템플릿 생성 후 사용하기 

 

\1.  부모의 주소 공간을 자식이 복사한다. (PCB, 자원도 다 복제한다) - **fork()** system call 

\2. 복제한 곳에 새로운 프로그램을 올린다. - **exec()** system call

 

**Copy - on - write (COW) :**

write(내용이 바뀔 때)가 발생하기 전까지는 부모의 코드 데이터 스택과 공유되지 않는 부분만 copy한다.

 

부모 프로세스가 fork()한 부분부터 자식 프로세스도 수행하는 이유 **->** 부모 프로세스의 context(부모의 PC)를 복제하기 때문 그대로 복사하기 때문에 main 함수부터 실행하지 않고, **fork()**한 부분부터 수행

 

운영체제가 fork()하면 그 순간부터 새로운 프로세스가 생성된다. 

자식(fork 결과 값이 0)과 부모(fork 결과 값이 양수)를 분류하는 법 **->** fork의 retun value가 다름

 

**exec() system call** : 어떤 프로그램을 완전히 덮어 씌워서 새로운 프로그램으로 태어나게 해주는 것(이전으로 복귀할 수 없다)

fork()와 달리 시작 부분부터 수행하게 됨.

 

```
int main() {
	printf("1");
	execlp("echo", "echo", "3", (char *) 0);
	printf("2");
}
```

\>> 1, 3 출력 후, 프로그램 종료

 

 

### **wait() system call**

부모프로세스는 wait()상태에서 blocked 상태가 되고, CPU를 받을 때 까지 기다린다.

자식 프로세스가 종료될 때까지 기다림

 

###  

### **프로세스 종료(Process Termination)** 

프로세스가 마지막 statement(명령) 을 수행한 후, 운영체제에게 스스로 이를 알려준다. (자발적 종료 - **exit**)

\- 자식이 부모에게 output data를 보낸다**(via wait)**. 프로세스는 자식이 먼저 종료 후, 부모가 종료한다.

\- 프로세스의 각종 자원들이 운영체제에게 반납된다.

 

 

부모 프로세스가 자식의 수행을 강제 종료시킴 (비자발적 종료 - **abort**)

\- 자식이 할당 자원의 한계치를 넘어선 경우

\- 일을 시키기 위한 자식을 만든것인데, 더이상 자식에게 할당할 task 가 없을 경우

\- 부모 프로세스가 종료(exit)하는 경우; 운영체제는 부모 프로세스가 종료하기 전 자식들이 먼저 종료된다. (단계적 종료)

\- 사람이 키보드로 kill, break(cmd+delete) 한 경우 

 

 

### **프로세스 간 협력**

1. **독립적 프로세스(Independent process)**

: 원칙적으로는 각자의 주소 공간을 가지고 수행되기 때문에, 하나의 프로세스는 다른 프로세스의 수행에 영향을 미치지 못함

 

2. **협력 프로세스(Cooperating process)**

프로세스 협력 메커니즘을 통해 하나의 프로세스가 다른 프로세스의 수행에 영향을 미칠 수 있음.

 

3. **프로세스 간 협력 메커니즘(IPC: Interprocess Communication)**: 프로세스간 정보를 주고 받을 수 있는 방법

1) 메시지를 전달하는 방법 

message passing: 커널이 메신저 역할을 함.프로세스 사이에 공유변수(shared variable)를 일체 사용하지 않고 통신하는 시스템

  \- 상대 프로세스 이름을 명시 O: Direct Communication

  \- 상대 프로세스 이름 명시 x: Indirect Communication,(Mailbox에 명시만 할 것인가)

 

2) 주소공간을 공유하는 방법

shared memory: 독자적인 주소공간을 갖고 있기 때문에, 일부 주소공간을 두 프로세스가 공유하는 경우가 있다. 

 process A 와 Process B 사이에 일부 메모리 공간을 공유. 커널에게 shared memory를 쓴다고 매핑 해둔 후, 사용자 프로세스끼리 서로 공유하도록 한다. 

------

 

✅ **thread:** thread는 하나의 프로세스이므로 프로세스 간의 협력은 X, 동일한 process를 구성하는 Thread 간에는 주소 공간을 공유하므로 협력이 가능, **프로세스간 x 쓰레드간의 협력 o**


### **CPU Scheduling**



![img](https://blog.kakaocdn.net/dn/c5fv7i/btq1QnVAlTz/v8qSGzoCC0VFhyToq3M4K1/img.png)

CPU and I/O Bursts



프로그램은 CPU burst와 I/O burst의 연속이다.

I/O burst 가 계속 끼어들수록 CPU burst 가 짧아진다.

 

 



![img](https://blog.kakaocdn.net/dn/bkhpyN/btq1XLUukaf/h8mKLDKY7nBtweZme4GEI0/img.png)

CPU-burst Time의 분포



컴퓨터 안에는 CPU bound job, I/O bound job이 섞여있기 때문에 CPU Scheduling이 필요하다.

대부분의 CPU는 CPU bound job이 많이 쓰고, I/O bound job은 자주 끼어 들지만 많이 쓰이진 않는다. 

 

**CPU Scheduling이 필요한 이유**

: I/O bound job은 주로 사람과 작용하는 Interactive 한 job이다. CPU를 공평하게 주는 것은 좋지만, 사용자에게 우선적으로 주어야 효율성이 높기 때문

 

 

### 프로세스의 특성 분류

1) I/O-bound process

\- 사람과의 Interaction

\- CPU를 잡고 계산하는 시간보다 I/O에 많은 시간이 필요한 job

\- many short CPU bursts

 

2) CPU-bound process

\- 계산 위주의 job

\- few very long CPU bursts

\- 연속적으로 쓰면 시간이 길어진다. 

 

 

### CPU Scheduler & Dispatcher

**1) CPU Scheduler** 

누구한테 CPU를 줄지 결정할지의 운영체제 커널코드

Ready 상태의 프로세스 중에서 이번에 CPU를 줄 프로세스를 고른다.

 

**2) Dispatcher**

CPU의 제어권을 CPU scheduler에 의해 선택된 프로세스에 넘기는 역할을 담당하는 운영체제 커널코드

이 과정을 context switch(문맥교환) 이라고 한다.

 

 

Scheduling이 필요한 경우

1. Running -> Blocked (I/O 요청하는 시스템 콜, 너무 오래걸릴 때 더이상 작업할 수 없는 경우) - nonpreemptive

2. Running -> Ready(할당시간 만료하는 time interrupt 발생하여 다른 프로세스에 넘겨주는 경우) - preemptive

3. Blocked -> Ready (오래걸리는 작업이 끝났을 경우, interrupt 걸어 다시 권한을 부여하는 경우) - preemptive

4. Terminate (프로세스가 종료되서 더이상 할게 없어서 새로운 프로세스에게 넘겨주어야 할 때) - nonpreemptive

 

 

정확하게 알아둘 용어 

nonpreemptive : 강제로 빼앗지 않고 자진 반납

preemptive: 강제로 빼앗음
