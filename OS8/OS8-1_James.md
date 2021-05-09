#  Memory Management

---

> "우리가 사는 집마다 고유의 주소가 있듯이 메모리 역시 주소를 통해 접근하는 저장장치이다."

![Screen Shot 2021-05-05 at 8.32.51 PM](/Users/hwang-in-u/Library/Application Support/typora-user-images/Screen Shot 2021-05-05 at 8.32.51 PM.png)

메모리에게는 주소가 주어지는데 이를 크게 두 가지로 나눌 수 있다.

- Logical address
  - 프로그램이 시작하면 프로그램 내에 생성되는 0번째 부터 시작되는 논리적인 주소
  - CPU가 바라보는 주소
    - 프로세스가 물리적 메모리에 올라가더라도 Logical address에 있는 코드 자체까지 바꿀 수는 없다. 그렇기 대문에 물리적 메모리 주소에는 코드 주소가 남아있을 수 밖에 없다. 그래서 CPU가  바라보는 또는 참조하는 주소는 Logical address가 되는 것이다. 
- Physical address
  - 프로그램이 시작하면 메모리에 실제 올라가는 위치. 프로그램이 실행되기 위해서는 메모리에 올라가야 한다.
  - 프로그램이 물리 메모리 주소 어디에 올라갈지 결정하는 것 == 주소 바인딩



> "프로세스가 실행되기 위해서는 해당 프로그램이 물리적 메모리에 올라가 있어야 한다. 또한 CPU가 기계어 명령ㅇ을 수행하기 위해 논리적 주소를 통해 메모리 참조를 하게 되면 해당 논리적 주소가 물리적 메모리의 어느 위치에 매핑되는지 확인해야 한다. 이렇게 프로세스의 논리적 주소를 물리적 메모리 주소로 연결시켜주는 작업을 주소 바인딩(address binding)이라고 한다."

![Screen Shot 2021-05-05 at 8.56.04 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210505205608.png)

- Compile time binding
  - 주소변환이 컴파일시 실행될 때
- Load time binding
  - 프로그램 실행이 시작 될 때
- Execution time binding(=Run time binding)



![Screen Shot 2021-05-05 at 8.58.27 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210505205850.png)

#### 1. Compile time binding: 

- 해당 주소 바인딩을 쓰게 되면 컴파일시 물리적인 메모리가 이미 결정 되는 것. 따라서 Logical Address에 정해져있는 주소를 그대로 메모리에 올려야 한다. [옛날 컴퓨터]
- 비효율적[다른 주소가 비었음에도 불구하고 똑같이 올려야 하니까] 
- 메모리 위치를 바꾸려면 다시 컴파일 해야 한다.

#### 2. Load time binding:

- 프로그램을 메모리에 올릴 때 논리적인 주소를 변환하여 메모리에 올리게 된다. 프로그램 논리적인 주소가 메모리의 물리적인 주소로 fix된다.
- 컴파일 중에 만들어진 코드가 재배치가 가능하다. 항상 특정 위치에 올라가야 하는 것이 아니라 비어있는 물리적 메모리 주소로 올라갈 수 있다.

#### 3. Run time binding

- 프로그램이 실행되다 경우에 따라 메모리 위치가 달라질 수 있다
  - 예: 메모리가 해제되었다 다시 올라갈 때 기존에 있던 메모리에 다른 프로그램이 존재하면 다른 메모리에 해당 프로그램을 저장한다 [요즘 컴퓨터]



![Screen Shot 2021-05-05 at 11.12.49 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210505231259.png)

주소변환을 지원해 주는 물리적인(hardware) 디바이스 == Memory-Management Unit

- **MMU Scheme**: Register 두 개를 이용하여 사용자 프로세스가 CPU에서 수행되며 생성해내는 모든 주소 값을 알려주는 기법 [ relocation register + limit register = 프로세스 주소 범위]

- ![Screen Shot 2021-05-05 at 11.14.39 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210505231451.png)

  MMU기법을 활용한 주소 변환

  "CPU가 특정 프로세스의 논리적 주소를 참조하려고 할 때 MMU 기법은 그 주소값에 기준 레지스터의 값을 더해 물리적 주소값을 얻어 댄다."

- ![Screen Shot 2021-05-05 at 11.19.23 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210505231936.png)
- LImit register가 필요한 이유
  - 메모리 보안
    - 다중 프로그래밍 환경에서 relocation register의 값을 더 했을 때 특정 프로세스의 주소 공간을 벗어나는 경우가 발생할 수 있는데 이는 다른 사람이 수행 중인 프로그램 영역을 침범하고 운영체제가 존재하는 메모리 영역을 변경해 시스템에 치명적인 결과를 초래할 수도 있다. 이를 방지하기 위해 limit register가 존재한다.

#### 용어 설명

1. Dynamic Loading

![Screen Shot 2021-05-05 at 11.22.30 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210505232239.png)

프로그램을 동적으로 메모리에 올리는 방법: 

- 해당 루틴이 필요할 때 마다
- 필요할 때 마다 올려주는 방식

*Paging 기법과 원래는 다르다!!

- 다이나믹 로딩은 운영체제의 특별한 지원 없이 프로그램 자체에서 프로그래머가 구현 가능. 이를 쉽게 하도록 OS가 라이브러리를 통해 지원한다.
- 섞어서 쓰긴 하지만 원래는 프로그래머가 명시적으로 활용하는 것이 다이나믹 로딩이다.



2. Overlays

![Screen Shot 2021-05-05 at 11.27.43 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210505232753.png)

- 초창기 컴퓨터 시스템에서 사용 된 용어.
- 다이나믹 로딩과의 차이점:
  - 프로그래머가 수작업으로 구현하는 것
  - 운영체제의 지원이 없다. all coding



3. Swapping

![Screen Shot 2021-05-05 at 11.29.34 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210505232945.png)

- 중기 스케줄러가 일부 프로그램을 골라서 해당 프로그램을 구성하는 것을 전부 다 통째로 쫒아낼지 결정.

- Swapping이 더 효율적으로 사용되려면 Run time binding이 지원되어야 한다. 쫒겨난 뒤 나중에 메모리에 올라갈 때 다른 비어있는 위치에도 올라갈 수 있게 지원되어야 더 효율적인 swapping 가능
- 스와핑은 많은 양의 데이터를 다루는 일이라 swap time 중 상당부분은 swap을 하여 데이터를 transfer하는데 걸리는 시간이 차지한다.



4. Dynamic Linking

![Screen Shot 2021-05-05 at 11.35.49 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210505233600.png)

- Static Linking: 컴파일 된 파일을 엮어서 실행파일을 만드는 것. 라이브러리 또한 linking이 되어서 실행파일이 만들어지면 내 코드가 라이브러리에 포함이 된다.
  - 100개의 프로세스 printf실행 → 100개의 copy된 printf함수 라이브러리 코드
  - 메모리 낭비↑↑↑
- Dynamic Linking: 실행 파일에는 라이브러리가 별도의 파일로 존재. 라이브러리를 찾을 수 있는 포인터만 존재한다. 실행 시 연결 된다.
  - printf를 사용시 매 실행파일에 들어가지 않고 해당printf가 어디에 있는지 알려주는 포인터만 존재하게 된다. 파일 실행 시 다이나믹 링크 실행 시 printf에 해당되는  파일을 찾아서 메모리에 올려서 실행. 다이나믹 링킹이 이미 올라가 있다면 다른 100개의 프로세스도 해당 링크를 공유하게 된다.(share)
  - 윈도우의 DLL == 다이나믹 링킹을 하는 shared 파일

![Screen Shot 2021-05-05 at 11.44.37 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210505234446.png)

- 운영체제 상주 영역
- 사용자 프로세스 영역:
  - 메모리 할당 방법:
    1. Contiguous allocation: 메모리가 통째로 연속적인 공간에 적재되는 것
    2. Noncontiguous allocation: 프로세스가 여러 영역에 분산되어 올라가는 것



#### 1. Contiguous Allocation

![Screen Shot 2021-05-06 at 12.27.44 AM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210506002752.png)

- 가변분할 방식
  - 

![Screen Shot 2021-05-06 at 12.28.26 AM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210506002834.png)

위 가변 분할 방식 예에서는 프로그램 B가 끝난 공간이 할당되어야 할 프로그램 D가 분할의 크기보다 작아 해당 곤간에 할당되지 못하고 프로그램 C 밑에 할당되어 프로그램 B가 끝난 공간이 외부조각이 된 상황이다.



낭비되는 메모리 조각:

- ![Screen Shot 2021-05-06 at 12.31.18 AM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210506003125.png)

  - 외부 조각에 대한 추가설명 pg 191

    "외부조각은 특정 프로그램에 배당된 공간이 아니기 때문에 만약 이 외부조강의 크기보다 작은 크기의 프로그램이 도착한다면 그 프로그램을 외부조각에 적재시킬 수 없다."

  - 내부 조각에 대한 추가 설명 pg 191

    "내부조각은 특정 프로그램에 이미 배당된 공간으로 볼 수 있으므로 내부조각에 수용할 수 있는 충분히 작은 프로그램이 있다 해도 공간을 활용할 수 없어 메모리가 낭비된다."

![Screen Shot 2021-05-06 at 12.35.22 AM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210506003531.png)

![Screen Shot 2021-05-06 at 12.36.30 AM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210506003640.png)

![Screen Shot 2021-05-06 at 12.39.21 AM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210506003929.png)

- 메모리의 영역을 한 군데로 모으는 방법
- 비용이 많이 드는 방법: 주소 바인딩을 고려해야 하기에
- Runtime-binding이 가능 할 때만 사용 가능한 방법

작은 이동을 통해 더 큰 hole을 만드는게 더 나은 방법이지 않을까?



#### 2. Noncontiguous allocation

![Screen Shot 2021-05-06 at 12.47.29 AM](/Users/hwang-in-u/Library/Application Support/typora-user-images/Screen Shot 2021-05-06 at 12.47.29 AM.png)

- 기존의 방식인 2개의 register를 이용해서 사용자 프로세스가 CPU에서 수행되며 생성해내는 모든 주소 값을 알 수 없다.
  - 페이지 단위로 나뉘어서 분산처리되어 메모리에 위치하기 때문에
  - 따라서 page table 이 필요하다.
  - Page Table: 프로그램을 구성하는 페이지의 물리적 메모리 주소 공간에 대한 정보를 담고 있는 배열이라고 볼 수 있다.
  - page table을 활용하여 각 각의 page의 logical address를 physical address로 변환할 수 있다.
  - page frame: 프로세스의 각각의 page가 물리적인 메모리에 저장되는 공간
- Internal fragmentation(내부조각) 발생 가능
  - 프로그램의 크기가 반드시 페이지 크기의 배수가 되지 않을 수도 있다. 맨 마지막에 가서는 페지 하나 보다 남는 프로그램의 크기가 작을 수 있다. 그래서 이 조각이 페이지에 포함 되지만 이렇게 될 경우 마지막의 분할의 크기보다 페이지가 작을 수 있고 남는 공간은 쓰이지 않는 공간이 된다. 

![Screen Shot 2021-05-08 at 11.11.54 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210508231159.png)

page table:

- 인덱스를 이용하여 곧바로 물리적인 메모리에 접근할 수 있는 자료구조

- page의 수만금 page table의 entry가 존재
- entry에는 각 페이지의 물리적인 주소에 대한 정보가 존재

![Screen Shot 2021-05-08 at 11.16.33 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210508231644.png)

p: page 번호

f: page내에서 얼만큼 떨어져있는지 off set

- Table의 P번째 번호를 통해 메인 메모리에서 몇 번째 frame에 존재하는지 알 수 있다.

CPU는 P를 page table에 대입해  f라는 page frame 번호가 나오게 되면 논리적인 주소를 물리적인 주소로 바꾸게 되고 물리적인 메모리의 f번째에 해당 페이지를 접근할 수 있게 된다.. Offset은 변함이 없다. 그저 행정구역이 바뀌게 되는 것

![Screen Shot 2021-05-08 at 11.21.59 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210508232205.png)

Page Table은 Main Memory[물리적인 메모리]에 상주

relocation register → Page-table base register로써 기능

limit register → Page-table length register\



2번 접근하면 작업이 느려질텐데 어떡하지...: Thanks to TLB

![Screen Shot 2021-05-08 at 11.26.52 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210508232659.png)

TLB: 

- 빈번히 참조되는 entry 일부 가지고 있음[논리적인 페이지 번호와 물리적인 페이지 frame 번호 가지고 있음]
- 전체 search 필요하다. 병렬 적으로 탐색이 가능한 associative register 탑재

![Screen Shot 2021-05-08 at 11.41.47 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210508234203.png)

Main Memory 접근 전 먼저 TLB확인 후 TLB에 필요한 주소가 존재하는 경우 서 다이렉트로 주소 변환.

각각의 프로세스마다 주소변환 정보도 달라지기 때문에 context switch를 통해 entry 정보를 지워야 한다.

TLB로 메모리 접근 시간 vs Page Table을 통한 메모리 접근 시간

TLB로 메모리 접근 시간은 Page Table만 있을 때 접근하는 시간보다 훨씬 적게 소요된다. ㅓㄷ 빠르게 접근 가능

![Screen Shot 2021-05-08 at 11.43.49 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210508234356.png)



#### 계층적 페이징

![Screen Shot 2021-05-08 at 11.52.37 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210508235243.png)

2단계 page table을 사용하는 이유: 

![Screen Shot 2021-05-08 at 11.45.55 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210508234604.png)

책 참고

![Screen Shot 2021-05-09 at 12.00.17 AM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210509000024.png)

![Screen Shot 2021-05-09 at 12.02.04 AM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210509000210.png)

상당부분 사용되지 않는 page의 entry를 만들 되 안쪽 page table은 만들어지지않고 null상태로 존재하며 주소를 가리키게 되기 때문에 메모리 공간에 효과적이게 된다.