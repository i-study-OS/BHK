System Structure & Program Execution

컴퓨터 시스템 구조를 설명하는 수업

<img width="821" alt="운영체제 2-1" src="https://user-images.githubusercontent.com/69072471/111486252-ad688480-877a-11eb-9b9c-863ef67353e1.png">

컴퓨터 시스템은

1. **CPU와**
2. **Memory로 구성된 Host컴퓨터**

**Memory**:

- CPU의 작업 공간.

  Memory controller: 메모리를 전담 담당하는 작은 CPU

- 원칙적으로 메모리는 CPU만 접근할 수 있다.

- Instruction

  - CPU가 어떤 작업을 해야할지 알려주는 Manual과 같다.
  - CPU는 이 메뉴얼에 따라서 작업을 처리한다.

1. **그리고 I/O Device들로 구성되어 있다.**

   - 마우스, 모니터, 프린트, 하드디스크 등

   - Device controller들은 I/O Device를 통해서 데이터를 읽고 쓰고 각각의 작업 공간인 Local Buffer에 데이터를 저장한다.
   - CPU에게 알려줄게 있을 경우 interrupt를 걸어서 CPU에게 알림을 보낸다.



<img width="800" alt="운영체제 2-2" src="https://user-images.githubusercontent.com/69072471/111486269-b0fc0b80-877a-11eb-826a-8fc3a52266bb.png">

**CPU**가 가지고 있는 것:

- **Register**: 메모리보다 빠르고 CPU속 정보를 저장하는 공간
- **Program Counter**: CPU에서 메모리의 instruction 주소값을 알려주는 장치



**CPU**의 역할 또는 운명 [Destiny]

- 매 순간 메모리에 올라와 있는 기계어를 처리한다(CPU Instruction, 0과 1의 조합으로 된 기계어 집합)

- CPU 속 program counter가 가리키고 있는 메모리 위치에서 instruction을 읽어오고 실행한다.

- instruction 실행 후 다음 instruction 실행 전에 interrupt가 들어왔는지 체크한다.

  - interrupt가 들어왔다면 하던 작업을 잠시 멈추고 CPU 제어권이 운영체제에게 넘어가게 된다. 운영체제는 interrupt마다 상황에 맞게 처리할 일이 커널 함수로 정의 되어 있다[인터럽트 백터 속 인터럽트 처리 루틴 또는 인터럽트 핸들러]
  - interrupt가 들어오지 않았다면 다음 instruction을 메모리로 부터 받아서 실행한다.  

- 오직 CPU만 메모리 접근도 할 수 있다.

  - 본인이 실행해야 할 instruction의 주소를 register에서 program counter가 instruction 주소값을 알려주면 해당 instruction을 실행하는 것 destiny...
  - Device driver를 통해서 I/O Device에게 명령을 내린다.

- CPU는 I/O device가 자신의 local buffer에 데이터를 저장하고 쌓인 데이터를  읽어서 Memory 속 자신의 작업영역으로 복사를 한다.

  🙋그럼 CPU는 너무 자주 interrupt 당하는거 아닐까? 거의 노예 수준으로 이리저리 불려다닐텐데...

  📚 키보드에서 호출, 모니터에서 호출... 정신없이 사는 CPU를 위해 `DMA controller`가 존재한다

  

  <img width="923" alt="Screen Shot 2021-03-20 at 8 01 11 PM" src="https://user-images.githubusercontent.com/69072471/111867410-0cb2d880-89b7-11eb-8ff6-1ebe96e23f61.png">

  **DMA controller**: Direct Memory Access

  - CPU 외 직접 메모리를 Access 할 수 있는 컨트롤러. DMA controller를 두게 되면 메모리가 CPU도 접근할 수 있고 DMA controller도 접근할 수 있게 된다.

  - CPU의 개입 없이 DMA controller가 직접 I/O device의 buffer storage의 데이터를 블록 단위로 메모리로 전송

    

    🙋 DMA controller와 CPU가 같이 메모리를 접근하면 문제가 생기지 않을까?

    📚 DMA는 그런 충돌을 방지하기 위해 조율을 하는 일종의 교통정리 역할을 도맡아서 한다.

    

    🙋 DMA의 존재이유?

    📚작은 일 하나하나 다 CPU가 interrupt되면 CPU가 효율적으로 작업을 못하니 이를 막기 위해 DMA controller는 중간 중간 I/O Device가 일을 끝내면 끝낸 작업을 메모리까지 복사 해 주는 작업을 한다. 이 작업이 끝나면 CPU에게 interrupt를 모아서 한 번만 걸어서 CPU가 방해 받는 횟수를 줄여준다.

  

  <img width="702" alt="운영체제 2-3" src="https://user-images.githubusercontent.com/69072471/111486276-b22d3880-877a-11eb-8b8e-b34c41200da3.png">

  **Mode bit**: CPU에서 실행되는 것이 운영체제인지 사용자 프로그램인지 구분 해 주는 하드웨어

  ---

  **사용자 프로그램에게 CPU를 넘겨 줄 때 Mode bit을 0 → 1로 바꿔서 넘겨준다**

  ---

  **운영체제가 사용자 프로그램으로부터 CPU를 돌려받으면 Mode bit이 1 → 0으로 바뀐다**

  ---

  **Mode bit**의 역할

  0일 때, 운영체제가 CPU를 가지고 있을 경우:

  - 모든 작업이 가능하다. 메모리접근, I/O Device 접근 등등

  1일 때, 사용자 프로그램이 CPU를 가지고 있을 경우

  - 제한된 Instruction만 CPU에서 실행 가능. I/O 장치 접근 불가, 운영체제 위치 접근 등등 불가하다.

  

  <img width="720" alt="운영체제 2-4" src="https://user-images.githubusercontent.com/69072471/111486281-b2c5cf00-877a-11eb-9b1f-bf87c6092ca1.png">

  **Timer**: 특정 프로그램이 CPU를 독점하는 것을 막기 위해 설계된 하드웨어.

  타이머 또한 interrupt를 걸 수 있다.

  - 여러 프로그램이 CPU를 사용해야 하기 때문에 `time sharing`을 위해 구현된
  - [운영체제가 CPU를 가지고 있다가 특정 프로그램이 시작되면 메모리에 올라와 있는  해당 프로그램에게 CPU를 넘겨주는데 넘겨줘기 전 Timer에 제한 값을 설정한 뒤 넘겨준다.]

  **Interrupt line**: 세팅된 시간이 되면 timer가 CPU에 interrupt를 걸면 CPU는 하던 일을 잠시 멈추고 CPU의 제어권이 사용자 프로그램으로부터 운영체제로 자동으로 넘어가게 된다. 한 번 넘어가면 뺏는건 불가하다.

  

  QnA:

  🙋 운영체제에게 넘겨진 CPU는 무엇을 하나?

  📚 다음 프로그램에게 CPU를 넘겨준다. 물론 타이머 세팅 후 넘겨준다.

  

  🙋 사용자 프로그램은 I/O 장치를 접근할 수 있나?

  📚 여러가지 보안상의 이유로 프로그램은 I/O 장치를 필요로 해도 직접적으로 I/O 장치를 접근할 수 없다. 그래서 I/O 작업을 하려면 스스로 운영체제에게 CPU를 넘겨주고 해당 작업을 운영체제가 I/O 장치에게 작업을 시킨다.

  

  <img width="682" alt="운영체제 2-7" src="https://user-images.githubusercontent.com/69072471/111486292-b5282900-877a-11eb-839f-6578e27879b5.png">

  🙋넓은 의미의 interrupt는 무엇인가?

  <슬라이드 참조>

  📚 Hard Interrupt와 soft Interrupt 이 두가지를 통틀어서 interrupt라고 부른다.

  다만 interrupt라고 말할 때 보통 우리는 하드웨어 인터럽트를 부르는 것이다.

  

  🙋 사용자 프로그램A가 키보드작업을 필요로 하는데 뒤에서는 프로그램B가 기다린다. 이런 상황에서 CPU는 어떻게 작동하는가?

  📚 

  1. 키보드에서 입력이 들어오기 전까지 CPU는 프로그램B로부터 호출을 받아서 다른작업을 한다. 
  2. 사용자가 키보드를 입력하고 해당 데이터가 키보드의 local buffer로 들어온 경우 keyboard controller가 CPU에게 하드웨어 interrupt를 건다. 
  3. 그러면 어떠한 프로그램을 돌리던 간에 CPU 제어권이 자동으로 운영체제에게 넘어가게 되고 운영체제는 interrupt를 체크하고 입력된 키보드 값을 키보드 입력을 요청한 프로그램에 copy를 해준 뒤
  4. 다시 프로그램 B로 돌아가서 일을 처리한다.

  - 위와 같은 메커니즘으로 운영체제는 프로그램을 처리한다.

  

  🙋 System Call은 무엇인가?

  <img width="696" alt="운영체제 2-8" src="https://user-images.githubusercontent.com/69072471/111486295-b5c0bf80-877a-11eb-8c61-e174cd58cce5.png">

  📚 메모리 속 프로그램이 I/O Device 요청할 때 운영체제 코드를 직접 수행하는 것이 불가능 하기 때문에  사용자 interrupt라인을 구축하여 CPU운영권을 memory로 넘겨서 부탁한 일을 하기 운영체제가 대신 하도록 커널 함수를 호출해야한다 이를 system call이라 한다. Software interrupt 중 하나인 system call을 통해 CPU jumps from...

  사용자프로그램 →  OS

  

  🙋 CPU를 가지고 instruction을 실행하다     I/O 작업을 하려 하면 어떤 과정을 거쳐야 하는가?

  📚*사용자 프로그램을 실행하다 I/O를 호출하는 것은 불가하다. 왜냐하면 모드빗이 1이고 사용자프로그램만을 접근할 수 있기 때문이다. 그렇기에 운영체제에 해당하는 주소로 넘어가야하는데 이 또한 허용이 안된다. 

  따라서 프로그램이 **직접** interrupt line을 세팅하는 instruction을 실행한다. 프로그램이 CPU를 갖고 있고 instruction을 쭉 실행하다 interrupt들어온게 있는지 확인을 할텐데 프로그램이 이  경우에는 소프트웨어적으로 interrupt를 걸 수가 있어진다. 

  그러면 CPU는 이 프로그램의 다음 instruction을 실행하는 것이 아니라 software  interrupt가 들어왔기에 Mode bit이 0으로 바뀌고 CPU의 소유권이 운영체제로 넘어가게 된다. 

  그러면 운영체제가 해당 요청이 올바른 I/O 요청인지 확인한 뒤 디스크에서 무엇인가를 불러오기위해 system call을 하고  운영체제가 disk control 에게 디스크에서 해당 데이터를 불러오도록 시킬 수 있다.

  - 즉 사용자 프로그램이 직접 실행하지 못하는 것을 운영체제에게 대신 해달라고 요청하기 위해 interrupt line을 세팅해서 CPU가 OS에게 넘어가도록 한다. 이를 Soft Interrupt 또는 Trap이라고 한다. Soft Interrupt == Trap

**즉 프로그램에서 I/O작업을 하기 위해서는**

프로그램 →소프트인터럽트 또는 Trap → 운영체제가 CPU를 활용해 작업 처리 → 작업이 끝났음을 알리는 하드웨어 인터럽트(from I/O controller to CPU)

**두 가지 interrupt 모두 다 I/O 작업을 위해 필요하다.**:

- 요청은 **소프트 인터럽트**를 통해 알림

- 요청한 작업이 끝났으면 **하드웨어 인터럽트**를 통해 알림 



<img width="693" alt="운영체제 2-5" src="https://user-images.githubusercontent.com/69072471/111486283-b35e6580-877a-11eb-88ea-4e3a4122c77a.png">



**I/O Device**

<img width="800" alt="운영체제 2-2" src="https://user-images.githubusercontent.com/69072471/111486269-b0fc0b80-877a-11eb-826a-8fc3a52266bb.png">

- 키보드, 마우스 등 정보를 컴퓨터에 저장하는 `Input Device`

- 프린터, 모니터와 같이 컴퓨터로부터 정보를 받아서 출력하는 `Output Device`

- 하드디스크: 보조기억장치이기도 하면서 I/O Device. 디스크 내 데이터를 읽어서 메모리로 읽어 오기도 하고 메모리의 처리결과를 파일디스크에 저장하기도 한다.

- I/O 장치는 특권 명령으로만 가능하기에 사용자 프로그램은 다이랙트로 해당 장치에 접근이 불가하다.

  <img width="720" alt="운영체제 2-6" src="https://user-images.githubusercontent.com/69072471/111486289-b48f9280-877a-11eb-858e-1948ea6d9102.png">

  

  

  **Device driver**: 각 장치별 처리 루틴 → 소프트웨어

  - 코드 중 각 Device에 맞게 접근할게 할 수 있는 소프트웨어.
    - 운영체제가 Device를 접근할 수 있게 해주는 코드를 담고 있는 그릇이다.
  - 하드웨어를 접근하기 위해서는 Device driver 설치가 필요하다.
  - 일을 하기위해서 존재하는 Manual: Firmware

  

  **Device controller**: 각각의 device를 전담하는 작은 CPU. → 하드웨어

  - Disk Controller: 하드디스크의 내부를 통제 [Head의 움직임과 같은 요소를 컨트롤한다.]
  - 각 Device의 local buffer를 접근할 수 있다 [데이터 처리]
  - 필요시 CPU에게 hardware interrupt를 걸 수 있다

  - 위와 같은 작은 컨트롤러가 각 I/O Device에 붙어있다.

    

  **Local buffer**: 각각의 device의 작업공간 

  [CPU에게 Memory가 있듯이 Device에겐 Local buffer가 있다]

  

  QnA:

  🙋 왜 I/O Device는 CPU가 관리하지 않는가?

  📚: CPU와 I/O Device는 처리하는 속도차이가 많이 나기 때문이다. CPU에 비해 하드디스크는 100만배 정도 느리기에 전담 컨트롤러가 필요하다



<img width="574" alt="Screen Shot 2021-03-20 at 8 15 11 PM" src="https://user-images.githubusercontent.com/69072471/111867685-01f94300-89b9-11eb-875d-1d7a218cb2f1.png">

좌측 그림은 메모리 접근하는 instruction 따로 있고 I/O를 수행 하려면 special instruction에 의해 I/O를 하게 만들어 놓여 있는 것.

우측 그림은 I/O 장치도 메모리 주소의 연장선. 메모리접근은 메모리를 접근하는 것 뿐 아니라 I/O장치까지 접근하도록 설계 되어 있는 것


<img width="923" alt="Screen Shot 2021-03-20 at 4 10 50 PM" src="https://user-images.githubusercontent.com/69072471/111862174-f09f3f00-8996-11eb-8dc2-36cbc2d38c87.png">

**[READ Program]**읽어오는 프로그램 같은 경우 I/O 작업을 하는 경우 데이터를 먼저 읽어온 뒤 처리해야 하기에 보통 syncronous하게 짜여져있다. 물론 asyncronous하게도 설계할 수 있다.

ex): disk에서 특정한 데이터를 읽어오라고 사용자가 I/O요청을 할 경우:

동기식인 경우: Disk가 데이터를 불러오고 disk controller가 CPU에게 하드웨어 인터럽트를 통해 데이터처리가 완료되었다는 알림을 주기 전까지 때 까지 사용자는 기다리도록 프로그램이 짜여져있다. 

비동기식인 경우: Disk가 데이터를 읽어 오는 중에 읽어오는 데이터와 상관없이 할 수 있는 작업을 한 뒤 데이터가 필요한 나머지 작업을 수행하도록 READ 프로그램을 짤 수도 있다.



[WRITE Program] 쓰는 프로그램 같은 경우  출력이 되었는지 확인하고 다음 작업을 할 필요가 없기 때문에Asyncronous하게 프로그램을 짜는 것이 더 자연스럽다. 내가 어떻게 썻는지 굳이 확인하고 이어서 write 작업을 하려면 syncronous하게 프로그램을 짤 수도 있다.



동기식 입출력 vs 비동기식 입출력

<img width="460" alt="Screen Shot 2021-03-20 at 4 03 23 PM" src="https://user-images.githubusercontent.com/69072471/111862209-30662680-8997-11eb-8872-adbe2f168862.png">



<img width="468" alt="Screen Shot 2021-03-20 at 8 22 50 PM" src="https://user-images.githubusercontent.com/69072471/111867845-11c55700-89ba-11eb-8365-8c8344b4b1b7.png">

**맨 위부터**

- CPU 속 Register, and Cache Memory.

- 그 뒤 DRAM으로 구성된 Main memory

- 마지막으로 Secondary Storage: Magnetic Disk, Optical Disk, Magnetic tape

**위로 갈 수록 속도가 빠르고 단공간당 가격이 비싸다라는 특징이 있다.**



**휘발성 vs 비휘발성**

휘발성: Register, Cache Memory, Main Memory

비휘발성: Magnetic Disk, Optical Disk, Magnetic Tape



**Primary(Executable) vs Secondary**

Primary: Register, Cache Memory, Main Memory등 CPU가 직접 접근할 수 있는 메모리

Secondary: Hard Disk와 같이 바이트 단위가 아니라 섹터단위로 접근이 가능한 메모리는 executable 하지 않은 매체이다.

🙋 Cashing이란?

📚 CPU가 데이터를 가져오려면 위 슬라이드 처럼 메모리 피라미드 맨 밑에서부터 데이터를 복사 해 와야 한다[Primary memory는 용량이 더 작기 때문에 대부분의 데이터를 용량이 큰 Secondary memory로 쫒아내둔다.]해당 작업을 처음 실행할 경우 시간이 비교적 더 많이 걸린다. 하지만 해당 작업을 두 번 째 실행할 경우 cache Memory에 저장되어있던 전 내용을 활용하여 비교적 더 빠르게 전에 사용했던 데이터를 복사 해 올 수 있다. 이를 cashing이라 부른다.



<img width="446" alt="Screen Shot 2021-03-20 at 8 36 58 PM" src="https://user-images.githubusercontent.com/69072471/111868224-0bd07580-89bc-11eb-865f-fb9e8c3c2b8f.png">

🙋 프로그램은 어떻게 실행 되는가?

1. 프로그램은 실행파일로써 하드 디스크의 파일시스템에 `file` 형태로 저장되어 있다.

2. 해당 프로그램 실행시 프로그램에 해당하는 파일이 메모리로 올라가서 프로세스가 된다.

<img width="1190" alt="Screen Shot 2021-03-21 at 3 26 08 PM" src="https://user-images.githubusercontent.com/69072471/111895990-c793b280-8a59-11eb-8028-cb4924c2d3a7.png">

프로세스가 되기 전 한 단계를 더 거치게 된다. 프로그램을 실행하면 해당 프로그램의 독자적인 주소공간이 만들어지는데 이 공간을 `Virtual Memory` 라고 한다.

- 이 공간은 stack, data 그리고 code로 이루어져있다.

- 이 주소공간은 프로그이 시작될 때 생성되고 종료 될 때 없어진다는 특징을 가지고 있다.

- 또한 해당 공간은 Physical memory에 모든 부분이 올라가있지 않고 필요한 부분만 Physical memory에 상주하게 된다 [메모리 절약을 위해] 그리고 나머지 부분은 disk의 `swap area`에 상주하게 된다.



🙋 Swap Area, File System의 차이점은?

📚 둘 다 hard disk에 존재하나 용도가 다르다.

File system: 전원이 나가더라도 file이 유지가 되도록 비휘발성의 용도로 hard disk를 사용하는 것이다.

Swap area: 전원이 나가면 해당 부분에 존재하는 메모리는 없어진다. 이 공간은 physical memory의 연장선상으로 휘발성의 용도로 hard disk를 사용하는 것이다.



**Physical memory의 커널영역**

<img width="963" alt="Screen Shot 2021-03-21 at 3 40 00 PM" src="https://user-images.githubusercontent.com/69072471/111896305-09255d00-8a5c-11eb-856b-69e8d2d838a2.png">

운영체제 커널의 code 영역에는...

- 자원을 효율적이게 관리를 할 수 있는 코드 존재
- 편리한 서비스 제공을 위한 코드 존재
- 각각의 인터럽트마다 어떻게 처리해야 하는지 매뉴얼 코드 존재

운영체제 커널의 data 영역에는...

- 하드웨어 종류마다 관리하기 위한 자료구조를 만들어서 관리
- 실행 중 프로그램을 관리하기 위한 자료구조를 만들어서 관리

운영체제 커널의 stack 영역은...

*운영체제는 함수구조로 코드가 짜여있는데 이를 호출하고 리턴할 때 stack 영역을 사용해야 한다.*



<img width="950" alt="Screen Shot 2021-03-21 at 3 52 41 PM" src="https://user-images.githubusercontent.com/69072471/111896510-7c7b9e80-8a5d-11eb-8f74-3c85dfce5c1c.png">



#### 프로그램의 실행단계:

<img width="952" alt="Screen Shot 2021-03-21 at 3 58 15 PM" src="https://user-images.githubusercontent.com/69072471/111896607-45f25380-8a5e-11eb-8b79-8eeea2643e14.png">

Program이 CPU를 갖고 있으면 이를 User Mode라고 부른다.

이 프로그램이 system call을 하게 되면 운영체제 커널의 주소공간의 코드를 실행하게 되는데 이때는 kernel mode에서 프로그램이 작동하게 된다.

system call이 끝나면 다시 프로그램에게 CPU가 넘어가고 user mode로 바뀌게 된다.

User mode와 kernel mode의 반복

