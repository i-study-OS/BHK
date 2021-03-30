#### **프로세스**

실행중인 프로그램. PCB는 커널의 프로세스마다 하나씩 갖고 있다. 

![img](https://blog.kakaocdn.net/dn/OqWzJ/btq1dyJMpZL/do75SHiF55DotZppsGPT9K/img.png){ width=50% height=50%}





 

 

#### **프로세스 문맥(context)**

중간 시점을 잘랐을 때 특정 시점이 어디까지 수행했음을 정확하게 나타내기 위하여 사용.

PC가 어디부분까지 실행되었는지, 메모리에 어떤 내용을 어디까지 쌓아놓고 있는지, 현재 변수의 값은 얼마정도이며

레지스터에 어떤 값을 넣어두고 어떤 instruction까지 실행했는지 등

 

모든 요소를 파악하여야 프로세스를 파악할 수 있기 때문에 해당 요소들을 **프로세스 문맥** 이라고 한다.

 

 

#### **문맥 교환 (Context Switch)** 



![img](https://blog.kakaocdn.net/dn/cqACqR/btq1dyJKt0V/AG8kdBd871UU9ReP9eJvbK/img.png){ width=50% height=50% }

![img](https://blog.kakaocdn.net/dn/tWkPm/btq1nHynxrJ/oaDIfRjdH44yd20SAlShXK/img.png){ width=50% height=50% }



(1) 문맥 교환이 일어나지 않는 경우 A -> A 

(2) 커널 모드로 들어갔다가 문맥교환이 일어나는 것 A -> B

: timer interrupt가 들어오면 특정 프로세스가 끝난 후에 넘겨주는 것이기 때문에 또 다른 프로세스에게 넘겨주는 것이므로 문맥교환이 일어남. I/O 와 같은 오래 걸리는 작업이 있을 때에는 당장 상태가 blocked가 되기 때문에 또 다른 CPU로 넘겨주게 된다. 

####  

#### **커널**

시스템의 모든것을 완전히 통제하는 운영체제의 핵심이 되는 컴퓨터 프로그램.

 

**커널의 역할** 

1) **보안**: 컴퓨터 하드웨어와 프로세스의 보안을 책임진다

2) **자원 관리**: 한정된 시스템 자원을 효율적으로 관리하여, 프로그램의 실행을 원활하게 한다.

특히 프로세스에 처리기를 할당하는 것을 스케줄링이라고 한다. 

3) **추상화**: 같은 종류의 부품에 다양한 하드웨어 설계를 할 수 있기 때문에 운영체제의 복잡한 내부를 감추고 일관성있는 인터페이스를 제공하기 위해 몇가지의 **하드웨어 추상화(같은 종류의 장비에 대한 공통 명령어의 집합)**들로 구현한다.

 

####  

#### **Process Controller Block (PCB)** 



![img](https://blog.kakaocdn.net/dn/bnTmzK/btq1bgv0hRu/taw79p4LLuCBrbwBYgkqN0/img.png){ width=50% height=50% }
PCB 하나당 갖고 있는 정보



프로세스가 하나 실행될 때마다 CPU를 얼마정도 주어야 할지, 메모리를 어느정도 주어야 할지 등

특정 프로세스를 관리할 필요가 있는 정보를 포함하는, **운영체제 커널의 자료구조**.

-> 운영체제가 프로세스 스케줄링을 위해 프로세스에 관한 모든 정보를 가지고 있는 DB를 PCB라고 함

각 프로세스가 생성될 때마다 고유의 PCB가 생성되고, 프로세스가 완료되면 PCB는 제거됨

Priorty: 우선순위가 높은 프로세스한테 다음번 CPU를 넘겨주는 것

 

(1) OS가 관리상 사용하는 정보: 운영체제가 프로세스를 관리하기 위한 정보

(2) CPU 수행 관련 하드웨어 값: 문맥을 표시하기 위한 정보, 어떤 register 값들을 넣어서 실행하고 있는가

(3) 메모리 관련:  메모리의 어디에 위치해어있는가 

(4) 파일 관련: 프로세스가 사용(Open)하고 있는 파일이 어떤 것인가 ? 

 

#### **프로세스의 상태 (Process State)** 

기준 : 하나의 CPU



![img](https://blog.kakaocdn.net/dn/QHXnV/btq1laAX06O/DKkgWoK6n2erXDokezPqS0/img.png){ width=50% height=50% }프로세스 상태도



1) Running : CPU를 잡고 instruction을 수행중인 상태 

2) Ready: CPU를 기다리는 상태(메모리 등 다른 모든 준비는 완료되어고, CPU만 올리면 됨) , 이 상태에 있는 프로세스들이 이곳 저곳 옮겨지면서 time - sharing을 당하고 있다.

3) Blocked(wait, sleep) : CPU를 주어도 당장 instruction을 수행할 수 없는 상태 Ex. 디스크에서 file을 읽어와야 하는 경우

 

\+ New: 프로세스가 지금 막 생성중인 상태

\+ Terminated: 이미 종료되면 수행(execution)이 끝나서 프로세스가 아니지만,  끝나도 정리하는 작업이 남아있는 기간을 말함

 

 

 



![img](https://blog.kakaocdn.net/dn/bGiw1c/btq1jlbF8eO/uIGRJ3WHhB1MoCrC3fGkJK/img.png){ width=50% height=50% }프로세스의 상태



**해당 과정이 필요한 이유** : CPU의 속도가 월등히 빠르기 때문에 모든 자원들이 놀지 않고 원활하게 일할 수 있는 매커니즘의 필요성

 

1) Ready Queue에 있던 하나의 프로세스가 CPU에서 running 하다가 timer interrupt 가 들어오면 Ready Queue의 가장 뒤로 간다.

2) 1)번에서 수행하던 프로세스의 다음 순서 프로세스가 CPU에서 running이 진행되어진다. 

3) 1),2) 번이 반복되면서 CPU 안에서 running 되어진다.

4) Disk에서 무언가를 읽어와야 하는 상황이 왔을 때, Running이던 CPU의 상태가 blocked로 바뀌면서 디스크의 서비스를 받는 곳(Disk I/O queue)에 프로세스가 가서 줄을 서게 된다. 

5) Disk에서는 요청이 들어온 프로세스들을 Disk Controller의 지휘아래 순서대로 진행을 할 것이다.

6) 만약 4)번에서 줄을 섰던 프로세스의 요청 작업이 끝이 나면, Disk Controller가 CPU에게 interrupt를 건다. (4)번에서 요청한 I/O 작업이 끝났습니다)

7) CPU는 무언가 하고 있던 프로세스 작업이 있었지만, interrupt가 들어왔기 때문에 하던 작업을 잠시 멈추고, CPU 제어권이 **운영체제의 커널**로 넘어가게 되어 CPU를 얻을 수 있는 자격을 갖게 된다. 

8) 다시 Ready queue안으로 들어간 프로세스의 상태가 바뀌어 줄을 서게 된다

 

**운영체제의 커널이 하는 역할** 

1.  4)번 작업인 I/O을 요청했던 프로세스의 작업이 끝났으면 그 프로세스의 메모리 영역에 해당하는 데이터를 넘겨주는 역할
2. Disk I/O queue 상태를 blocked에서 Ready로 바꾸어 CPU를 얻을 수 있는 자격을 준다.

 

**공유데이터**

여러 프로세스가 동시에 접근하면(하나의 프로세스가 접근하는 도중에 다른 프로세스가 공유 데이터에 접근하게 되는 상황) 다른 프로세스가 이 데이터를 접근하러 왔을 때 접근을 막아주어야 하는데, 이미 데이터를 쓰고 있는 프로세스가 데이터를 내놓지 않을 경우(오래 기다리는 작업이 필요한 경우) 프로세스는 blocked 상태가 되고, 공유데이터 탓인지 키보드 탓인지 종류에 따라 해당하는 resouce Queue에 줄서 있게 되고, 그 작업이 완료가 되면 Ready Queue에 넘어와서 CPU를 얻을 수 있는 것.

 

놀이동산에 기구를 타러 다니는 상황과 비슷하다.

 



![img](https://blog.kakaocdn.net/dn/b8VeOG/btq1beydw84/CQfFr954GekpNc0PrFM6k0/img.png){ width=50% height=50% }

![img](https://blog.kakaocdn.net/dn/JBr2i/btq1ovYLVKC/xArIPZUD5unLRk7sU3bO81/img.png){ width=50% height=50% }PCB의 구성요소 Pointer가 있어 줄줄이 연결 시키는 상황



 

### **스케줄러 ( Scheduler)**

**1)  Long - term scheduler(장기 스케줄러 or job scheduler)**

\- 시작 프로세스 중 어떤 것들을 ready queue로 보낼지 결정

\- 프로세스가 시작 (new -> ready) 로 넘어갈 때,

메모리에 올라가는 작업(admitted)을 허락하게 되면

비로소 ready 상태가 되며 CPU를 얻을 수 있게 되는데, 어떤 프로세스에 메모리를 줄지 안줄지를 결정하는 것

(degree of Multiprogramming을 제어; 메모리에 몇개의 프로세스의 수를 올려놓아야 할지 조절(제어)하는 역할)

\- 프로세스에 메로리(및 각종자원)을 주는 문제를 다룸

\- time sharing system에는 보통 장기 스케줄러가 없음 **(무조건 ready)**

 

**2) Short-term scheduler(단기 스케줄러 or CPU scheduler)**

\- 짧은 시간단위로 스케줄이 일어남

\- millisecond단위로 굉장히 빠르게 일어남

\- 어떤 프로세스를 다음번에 running 시킬지 결정

\- 프로세스에 CPU를 주는 문제

 

 

**3) Medium-term Scheduler(중기 스케줄러 or Swapper)**

\- 여유 공간 마련을 위해 프로세스를 통째로 메모리에서 디스크로 쫓아냄

\- 프로세스에게서 memory를 뺏는 문제(메모리에 올라가있는 프로세스를 조절하는 역할, 현재 우리가 사용하고 있는 시스템)

\- 일단 프로세스를 다 올려놓은 후 올라가있는 프로세스를 조절하는 역할

\- degree of Multiprogramming을 제어

\- Suspended 상태에서 사람(user)이 프로세스를 재개시켜주어야 프로세스의 상태로 올라갈 수 있다. 

 



![img](https://blog.kakaocdn.net/dn/AYNgN/btq1bgv5b3l/UlAcaEv0uXBbIrKqxOKjek/img.png){ width=50% height=50%}

CPU 관점. 중기 스케줄러 때문에 추가된 Suspended(stopped) 상태



Suspended(stopped): CPU 뿐만 아니라 외부에서 프로세스를 강제로 정지시켜놓은 상태. 

하던 일을 멈추고 정지가 되며 메모리에서 다 쫓겨나서 통째로 디스크로 swap out 되어진다. 

 

Blocked: 자신이 요청한(자의) -> Ready

Suspended: 외부에서 정지(타의) -> Active

 

 




<img src="https://blog.kakaocdn.net/dn/7CVhw/btq1g9Qf6Ez/tza7owJaMfS3K4iD8HGHX0/img.png" style="height:200px" />

사용자 프로그램의 상태도



inactive 상태 : 외부적 요인으로 프로세스가 얼어붙어 정지되어있는 상태. suspended 상태는 기본적으로 메모리를 완전히 잃어버리는 상태이기 때문에 CPU관점에서는 아무 일도 할 수 없지만, I/O 작업 같은게 진행중이었다면 Suspend Ready로 넘어갈 수는 있다. 

 

active 상태 : CPU를 얻었거나, 기다리거나 등 활성화 되어있는 상태

 

[참고자료]

[core.ewha.ac.kr/publicview/C0101020140318134023355997?vmode=f](https://core.ewha.ac.kr/publicview/C0101020140318134023355997?vmode=f)
