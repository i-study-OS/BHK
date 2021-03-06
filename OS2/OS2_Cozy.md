#### Chapter 2. 시스템 구조 & 프로그램 실행

컴퓨터의 하드웨어 적인 것을 설명하는 시간을 가진다. 

시스템 구조:  cpu,  메모리로 구성된 컴퓨터와,  I/O device





##### Memory: CPU의 작업공간

cpu는 메모리에서 클럭단위로 기계어를 읽어서 실행한다.

cpu안에는 메모리보다 빠르면서 정보를 저장하는 register가 있음.

<u>프로그램이 직접적으로 I/O 작업은 불가능하므로 I/O이 필요할 때에는 cpu를 OS에 넘겨주고, OS가 처리할 수 있도록 해야한다.</u>



하드디스크는 I/O 디바이스로서의 역할도 동시에 수행하는 장치

디스크의 내부를 통제하는 것은 cpu의 역할이 아닌 device controller가 한다. 각각의 컨트롤러들이 각 I/O 디바이스별로 있어서 각 디바이스들을 담당한다.



각 Device controller 도 각각의 작업 공간이 있는데 이것을 로컬 버퍼라고 한다.



cpu와 I/O 디바이스의 속도 차이가 많이난다. 

디스크를 cpu가 직접 관장하지 않고 디바이스 컨트롤러들이 역할을 담당한다.

cpu는 매 클럭마다 인스트럭쳐를 읽어서 실행하는 역할만을 담당한다.

cpu안에는 메모리보다 더 빠르면서 메모리를 저장할 수 있는 공간인 '레지스터'가 있다. 

cpu안의 Mod bit은 지금 cpu에서 실행되는 것이 운영체제인지 사용자 프로그램인지 구분해주는 것.

interrupt line: cpu는 항상 메모리에 있는 인스트럭션만 수행한다. 정확하게 설명하면 인스트럭션 하나 실행되고 나면 다음 번 실행되어야 하는 기계어의 주소값이 증가하고 그럼 다음 사이클에서는 그 다음 인스트럭션 수행하고 이런 역할만 반복하여 수행한다. ex. 디스크에서 무언가를 읽어와야한다던지, 읽어오라고 요청했는데 이미 그 작업을 끝냈따던지 그런것을 전달하기 위해서 interrupt line이 존재한다.



cpu는 메모리와만 일한다.

c 프로그램이 컴파일 되어 A라는 프로그램으로 실행한다면 계속 실행하다가 키보드 입력도 받을 수 있고, 디스크에서 어떤 파일을 읽어와서 다음 인스트럭션을 실행하는 그런 경우도 있을 수 있다(메모리랑만 일할 수는 없다. 키보드 입력이나 화면 출력 등을 접근하는 인스트럭션 

cpu는 메모리 인스트럭션과만 실행하도록 되어있다. 

그러다가 disk에서 읽어오라는 요청은 어떻게 읽어오는가 ? -> 직접 디스크를 접근하는 것이 아닌 디스크 컨트롤러에게 특정 데이터를 읽어오라고 cpu가 일을 시킨다.일을 시켜놓으면 디스크는 요청한 데이터를 컨트롤러의 지시를 받아서 읽어온다. 그 작업은 굉장히 오래걸리며 디스크는 시키는 일을 하면서 자신의 로컬 버퍼에 집어넣게 된다. 그러다가 빠른 cpu가 놀고 있으면 낭비되기 때문에 I/O를 하게 되면 cpu가 직접 하는것이 아닌 컨트롤러에게 시키고 cpu는 그 시간에 놀지 않고 메모리에게 접근하면서 본인이 할 수 있는 일을 수행한다. ) 당장 cpu만 주면 메모리하고만 접근하면서 인스트럭션을 실행하려고 하면 그 프로그램에게만 cpu를 주면 된다.



cpu는 빠르면서 쉬지 않고 일을 한다. 여러 가지 수행이 있을 때 cpu는 여러 작업을 빠르게 왔다 갔다 하면서 응답하면서 일을 한다. 

for문이나 while 중간에 I/O작업 없이 온전히 cpu만 일을 한다면 
-> cpu가 다른 프로그램에게 넘어가지 못해서 time sharing을 구현하지 못한다. 그래서 컴퓨터 안에 Timer라는 하드웨어가 있따.



##### Timer (특정 프로그램이 cpu를 독점하는 것을 막기 위함)

- OS가 cpu를 갖고 있다가 사용자 프로그램이 여러개 실행될때 cpu에 넘겨주면서 timer를 준다.

- 사용자 프로그램은 할당된 time을 가지고 프로그램에게 cpu를 넘겨주면 각자는 자신의 기계어를 수행하다가 시간이 지나면 timer가 cpu에게 세팅 시간이 끝났다고 **interrupt**를 준다 -> 정해진 시간이 흐른 뒤 OS에게 제어권이 넘어가도록 인터럽트를 발생

- cpu는 하나의 instruction이 끝나고 나면 interrupt line을 체크하고, 체크된 작업이 없으면 다시 자기 일을 하고 있다면 수행해주고 하는 작업을 반복한다. 

- 운영체제가 사용자 프로그램에 cpu를 줄 때는 자유롭게 주지만 쉽게 뺏을 수는 없다. 

- 무한 루프가 아닌 종료가 있는 프로그램에선 다 쓰고 나면 cpu에게 다시 돌려준다.





**Mode bit** :

보안을 목적으로 instruction set을 나누어 놓았다. 

- 0 : 모니터 모드(커널 모드), 운영체제가 cpu에서 수행중일 때 모든 실행이 가능함

  주로 interrupt나 Exception 발생 시 하드웨어가 modebit을 0으로 바꾼다.

- 1: 사용자 프로그램에 cpu를 넘기기 전에 1로 셋팅한다.

  interrupt가 되면 cpu 제어권이 운영체제에게 넘어가게 되면서 mode bit은 0이 된다.



##### I/O Device controller

- 해당 I/O 장치 유형을 관리하는 일종의 작은 cpu

- I/O는 실제 device와 local buffer 사이에서 일어난다. 

- device controller는 I/O가 끝났을 경우 interrupt로 cpu에게 사실을 알림.
- 메모리를 원칙적으로는 cpu에서만 접근하고, I/O device는 자기 자신의 로컬 버퍼가 존재하기 때문에 버퍼가 쌓이면 cpu가 그에 대한 내용을 읽어서 자기 자신의 메모리 영역에 복사한다. -> 이게 DMA인가 ?

메모리도 하나의 device이므로 메모리 컨트롤러가 있다. 

##### 용어 정리 :

- device driver(장치 구동기) : OS 코드 중 각 장치별 처리 루틴 (S/W)

- device controller(장치제어기): 각 장치를 통제하는 일종의 작은 CPU (H/W)

  

##### DMA(Direct Memory Access), 직접 메모리를 접근하는 컨트롤러

​	DMA 장치가 필요한 이유:  I/O 장치가 너무 많은 인터럽트를 걸면 cpu가 방해가 되버릴 수 있으므로 중간중간 작업이 들어왔을 경우 cpu한테 interrupt를 걸지않고 cpu는 본인의 일을 하면서 로컬 버퍼의 작업이 끝나고 나면 DMA가 직접 메모리에 복사하는 일까지해준다. 작업이 끝나면 한번만 cpu에게 인터럽트를 걸어 <u>cpu가 인터럽트 당하는 빈도를 줄이기 위해</u>

메모리를 cpu 도 접근할 수 있고, DMA도 접근할 수 있게 함. <u>둘이서 특정 영역 동시에 접근하면 문제가 될 수 있기 때문</u>에 **memory controller**가 문제를 중지하는 역할을 한다. 누가 먼저 접근할지 교통정리를 하는 역할.





   

##### system call: 사용자 프로그램이 운영체제의 커널에 작업을 요청하는 것. 

- 디바이스 컨트롤러들이 cpu에 인터럽트를 거는 것처럼 system call을 한다. 

- 프로그램을 실행하다가 I/O를 해야한다면, 운영체제에 해당하는 주소로 넘어가야 하는데 Mod bit이 1일 경우에는 접근을 하지 못한다. cpu는 다음 인스트럭션이 아닌 인터럽트가 왔으므로 mod bit이 0이 되면서 운영체제에게 cpu권한이 넘어가기 때문에 디스크 컨트롤러에게 읽어오라고 명령할 수 있다. 



**인터럽트** : 인터럽트 당한 시점의 레지스터와 program counter 를 save 한 후 cpu의 제어를 인터럽트 처리 루틴에 넘긴다.

**interrupt** - 하드웨어 인터럽트: 하드웨어가 발생시킨 인터럽트 

**trap**- 소프트웨어 인터럽트: 사용자 프로그램이 돌아가다가 내가 직접 접근을 못하고 운영체제한테 대신 해달라고 요청을 해야하는 상황에서 interrupt line을 세팅해서 OS한테 cpu를 넘어가게 하는 것

- Exception: 프로그램이 오류를 범한 경우
- System call: 프로그램이 커널 함수를 호출하는 경우



OS는 올바른 요청인지 확인후 I/O 수행을 하며 I/O를  완료했을 경우 제어권을 시스템콜 다음 명령으로 옮긴다.

I/O 요청시에는 소프트웨어 인터럽트로 쓰고 요청 후 받을 때에는 하드웨어 인터럽트를 사용한다.

<u>운영체제는 cpu를 사용할 일이 없고, 인터럽트가 들어올 때만 cpu가 운영체제한테 넘어가는 것. 그렇지 않을 때는 운영체제는 항상 사용자 프로그램이 쓰고 있다.</u> 

시스템 콜: 사용자 프로그램이 운영체제한테 무언가를 부탁할 때 사용하는 방법

즉, 사용자 프로그램이 운영체제 코드를 직접 수행하는 것이 불가능하기 때문에 사용자 프로그램이 호출하고자 할 때는 사용자 프로그램이 interrupt line을 세팅하고 cpu제어권이 운영체제한테 넘어가게 된다.  



**인터럽트 관련 용어** 

인터럽트 벡터: 해당 인터럽트의 종류가 여러가지 있고, <u>각각의 처리 루틴 주소</u>를 가지고 있음

인터럽트 처리 루틴(= interrupt service routine, 인터럽트 핸들러): 해당 인터럽트를 처리하는 커널 함수. <u>실제로 인터럽트를 처리, 해야하는일 처리하는 곳.</u>

cpu는 매번 PC가 가르키는 곳의 작업을 수행하는데, 인터럽트가 되면 cpu 제어권이 운영체제에 넘어간다.

cpu의 mode bit이 0이면 cpu가 수행할 수 있는 모든 기계어 집합을 다 실행 가능, 1일 경우에는 한정된 기계어들만 실행할 수 있다.

운영체제가 cpu를 갖고 있을 때는 0이기 때문에 모든 기계어 집합 다 실행가능.



#### 동기식 입출력과 비동기식 입출력 

**동기식 입출력(synchronous I/O)**

: I/O 요청 후 입출력 작업이 끝난 후에야 사용자 프로그램이 다음 일을 할 수 있는 것.

 정말 write가 잘 되었는지 확인 후에 넘어가야 하는 작업일 때. 

I/O가 끝났다는 interrupt가 끝난 후에 그 다음 작업을 진행해야 하는 것.

**구현방법** 

1. I/O 가 끝날 때까지 CPU를 낭비시키고, 매 시점 하나의 I/O만 일어날 수 있음.

2. **I/O 가 완료될 때까지 해당 프로그램에게서 CPU를 빼앗거나 I/O 처리를 기다리는 줄에 그 프로그램을 줄 세움**, 다른 프로그램에게 CPU를 줌  -> 이 방식은 cpu가 놀지 않고 일을 하면서 효율적으로 사용하기 때문에 이 방식을 선호한다. 



##### 비동기식 입출력(asynchronous I/O)

: I/O가 시작된 후 바로 cpu를 가져와서 바로 다음 일을 하는 것.  요청 결과를 보지 않고, 읽어온 데이터와 상관 없이 그 동안에 다른 작업들을 할 수 있도록 프로그램을 짜는 것. write는 async가 더 자연스럽다. storage에 데이터를 쓰라고 하고 결과 나오지 않고 다른 작업을 수행 하는 것.

-> 두 경우 모두 I/O 작업이 끝난 후 인터럽트로 cpu에 알린다.

<u>I/O 작업은 kernel을 거친다.</u>



##### DMA(Direct Memory Access): 메모리 접근 장치

cpu가 작은 일들을 포함하여 많은 인터럽트를 당하면 효율적으로 처리하지 못한다.

CPU 중재 없이 device controller가 device의 buffer storage의 내용을 메모리에 블록 단위로 직접 전송

바이트 단위가 아닌 **Block 단위**로 인터럽트를 발생시킴 -> cpu의 인터럽트 빈도가 줄어 효율적인 처리 가능 



#### 서로 다른 입출력 명령어 두가지

1. Special instruction: 메모리 접근을 하는 기계어, I/O를 하는 special 기계어가 있는 경우

2. Memory Mapped I/O: I/O 디바이스도 Device Address가 있어서 주소를 가진 메모리들에 접근할 수 있는 것 



#### 저장장치 계층 구조 

- primary(cpu가 직접 접근 가능): 레지스터, 캐시 메모리, 메인 메모리 (휘발성, 속도 빠름, 용량이 적기 때문에 비용 비쌈)

  요즘은 메인메모리도 비휘발성으로 만드는 반도체가 나오고 있다.

  캐시메모리는 메인메모리보다 용량이 작아 모든걸 담지는 못하고, 당장 필요한 것만 밑에서 위로 올려다가 쓴다. 

  caching: 빠른 매체로 정보를 읽어들여 쓰는 것, 한번 읽어들인 것은 다시 읽어드릴 때 빠르게 정보를 읽어들일 수 있다. (**재사용의 목적**)

- secondary(cpu가 직접 접근 불가능): 마그내틱 디스크, 옵티컬 디스크, 마그내틱 테이프 (비휘발성, 속도 느림, 비용 쌈)





#### 프로그램의 실행(메모리 load)

##### 파일형태의 실행파일을 실행하면 메모리에 올라가서 프로세스가 되어 실행을 한다.

가상메모리(virtual memory) : 물리적인 메모리에 올라가기 전에 한단계 거치는 곳



모든 프로그램은 독자적인 주소 공간을 갖고 있고, 이것을 물리적인 메모리 공간에 올려서 사용해야하는데 당장 필요한 부분만 프로그램 실행 시에 주소공간에 올라가있다가 그렇지 않은 것은 올리지 않아야 메모리 낭비가 되지 않는다. 

그 프로그램만의 독자적인 실행 공간은 **code, data, stack** 형태로 저장되어진다.

올리지 않는 것은 **swap area**에 들어가있는다.



**virtual memory**: 각 프로그램마다 독자적으로 갖고 있는 메모리의 주소 공간을 말한다.

**File system**: 전원 나가도 남아있어야 함

**Swap area**: 전원 나가면 의미없는 정보이므로 휘발되어도 됨. 메모리의 연장공간으로 사용됨 . 그럼 메모리는 휘발성인가 ? ? 





#### 커널 주소 공간의 내용

code: 커널 코드, 시스템콜 인터럽트 처리 코드, 자원 관리를 위한 코드, 편리한 서비스 제공을 위한 코드

data:  하드웨어를 관리할 때 종류마다 자료구조를 만들어서 관리하고 있다. PCB: 시스템안에 프로그램이 하나 돌아갈 때 그 프로그램을 관리하기 위한 운영체제의 커널, 프로세스마다 PCB가 하나씩 만들어져 관리하고 있다.

stack: process A의 커널 스택, process B의 커널 스택



#### 사용자 프로그램이 사용하는 함수 

###### 함수(function)

- **사용자 정의 함수**: 자신의 프로그램에서 정의한 함수 (code)

- **라이브러리 함수**: 

  자신의 프로그램에서 정의하지 않고 갖다 쓴 함수(code)

  자신의 프로그램의 실행 파일에 포함되어 있다

- **커널 함수**:

  운영체제안에서 정의된 함수

  커널 함수의 호출 = 시스템 콜

  

