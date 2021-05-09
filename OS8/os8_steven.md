https://www.notion.so/stevenkim18/OS-8-1-1f5ce8f5559447488fd4e9bce14ab8b8

메모리에 접근하기 위해서는 주소가 필요하다!

우리가 주로 보게 되는것은 **논리적 주소**

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4e7bc9a9-ad4e-433f-b20b-526267c10477/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4e7bc9a9-ad4e-433f-b20b-526267c10477/Untitled.png)

### 논리적 주소(가상 주소)

- 프로세스 마다 독립적으로 가지는 주소 공간이다
- 각 프로세스 마다 0번 부터 시작!
- Cpu가 보는 주소!(물리적 주소를 볼 것 같지만 아님)

### 물리적 주소

- 정말 레알 하드웨어적인 메모리

> 개발자가 변수나 함수를 선언 함으로 메모리를 사용하게 되는데 이를 Symbolic Address 라고 한다.

→ 컴파일을 하면 이것들이 논리적 주소로 변환되고, 논리적 주소가 물리적 주소로 변환이 되어야 한다. 

이 주소 변환이 언제 이뤄지느냐....

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/751995fa-a9ff-4a96-a288-34bce9f3deb2/_2021-05-08__7.40.07.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/751995fa-a9ff-4a96-a288-34bce9f3deb2/_2021-05-08__7.40.07.png)

### Complie time binding

- 컴파일시 물리적인 주소로 변환하는 방법
- 매우 비효율적
- 컴파일 될 때 마다 물리적 주소가 바뀜.

### Load time binding

- 프로그램이 실행시에 물리적인 주소로 변환하는 방법

### Run time binding

- Load time binding처럼 실행시 변환!
- 실행 도중에도 물리적 메모리 주소가 바뀔 수 있음(자리가 없어서 쫓겨 났다가 다시 메모리로 들어 올 때)
- 이를 위해서는 하드웨어적인 지원이 필요한대 이를 MMU(Memory Management Unit)

```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
	char *ptr = malloc(1);
	printf("%p\n", ptr);
}

$ ./a.out
0x145e06790
$ ./a.out
0x130e06790
$ ./a.out
0x14a606790
$ ./a.out
0x137e06790
```

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0493ff04-04eb-4152-9108-1826f45d97d5/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0493ff04-04eb-4152-9108-1826f45d97d5/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/68c20cbe-e1e0-477a-919e-95f9f8544a3f/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/68c20cbe-e1e0-477a-919e-95f9f8544a3f/Untitled.png)

- 2개의 레지스터를 사용한다.
- 어느 한 프로세스의 논리적인 메모리의 0번지는 물리적 메모리의 특정 위치(예시는 14000)을 base register가 기억하고 있다.
- 그리고 그 프로세스가 사용하고 있는 메모리 양을 limit register가 기억하고 있다.

## 용어 설명

### Dynamic Loading

- 프로그램 전부를 메모리에 올리는 것이 아닌 필요한 부분만 올리고 필요 없는 부분(애러처리 등)은 올리지 않는 것
- 개발자가 직접 메모리를 올리는 것을 말하지만 요즘은 OS가 해주는 것과 같은 용어로 사용되기도 함.\

### Overlays

- 다이나믹 로딩이랑 비슷하나, 역사적 관점에서 다르다
- 초기 PC는 한개의 프로그램도 메모리에 올라갈 수 없을 만큼 작았다.
- 개발자가 OS의 도움없이 직접 수동으로 메모리에 올리고 내리는 것을 구현했다.

### Swapping

- 프로세스를 일시적으로 메모리에서 하드디스크(백킹 스토어)로 쫓아내는 것
- 중기 스케줄러에 의해서 swapout 될 프로세스를 선정
- 주소 변환 기법중 Runtime binding 일때 의미가 있음.
- swap time 대부분은 프로세스의 크기와 비례

### Dynamic Linking

- Linking 이란 컴파일된 파일들을 하나의 실행 파일로 만드는 것(자바코드 → javac → 바이너리 → java → 실행)
- Static linking - 만약 printf함수가 있는 라이브러리를 사용한다고 한다면 그 라이브러리를 모두 다 링킹하는 것
- **Dynamic Linking - printf 함수가 있는 라이브러리의 포인터만 저장하고 printf가 사용 될때 만 라이브러리를 사용 됨.**
- 이것을 해주게 하는 것을 윈도우에서는 dll, 리눅스에서는 Shared Object라고 한다.

# 물리적인 메모리를 어떻게 관리할 것인가?

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6c5effc3-64c8-4738-b5f6-6ee5e60b8c35/_2021-05-08__11.15.07.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6c5effc3-64c8-4738-b5f6-6ee5e60b8c35/_2021-05-08__11.15.07.png)

- 운영체제 커널의 메모리 영역은 낮은 번호
- 사용자 프로그램은 높은 주소 영역을 사용
- 물리적인 메모리를 할당하는 방법은 크게
    1. 연속 할당
    2. 불연속 할당

## 연속 할당

- 각 프로세스를 메모리의 연속적인 공간에 **통째로** 넣는 것
- 여기서서도 2가지 방법으로 나뉜다.
    1. 고정 분할 방식
    2. 가변 분할 방식

### 고정 분할 방식

- 말 그대로 미리 메모리의 공간을 분할해서 그 크기가 맞으면 넣는 것
- 메모리 낭비가 심하다.

### 가변 분할 방식

- 메모리 공간을 나누지 않고 차례대로 넣는 법
- 문제는 중간에 있는 프로그램이 메모리를 사용하고 나가면 중간 부분이 텅비게 된다.
- 그러면 다른 프로그램이 들어오게 되면 메모리가  부족해서 못들어오거나 남는 현상이 발생한다.
- 이것도 낭비가 발생

> 이런 낭비 되는 공간을 Hole(구멍)이라고 함.

- 이런 홀들이 생기는 문제를 Dynamic Storage-Allocation Problem 이라 하고 이 안에서 최대한 활용하려고 나온 방법은 3가지(first- fit, best-fit, worst-fit) 이다

또 다른 방법은 프로그램을 위로 미는 방법이 있는데, 이것은 오버헤드가 너무 커서 비효율적이다.

> 우리가 지금 쓰는 os들은 불연속 할당 방식이다!

## 불연속 할당

- 하나의 프로세스가 메모리의 여러 영역에 분산 되어 올라가는 것이다.
- 여기서는 3가지 방법으로 나뉜다.
    1. Paging
    2. Segmentation
    3. Paged Segmentation(1, 2번을 합친 것)

### Paging

- 프로세서가 올라가 있는 논리적 메모리를 일정한 크기로 잘라서 물리적인 메모리에 저장하는 방법이다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a346f099-e1fc-482c-9e0d-7847b30e9691/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a346f099-e1fc-482c-9e0d-7847b30e9691/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ba84c45c-3d55-48a1-bcb4-fe03b4a27fa2/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ba84c45c-3d55-48a1-bcb4-fe03b4a27fa2/Untitled.png)

> Page Table은 어디에 저장이 될까?

- Page Table은 캐쉬나 레지스터에 저장되기는 너무 큼.  → 그래서 Main Memery에 저장이 됨.
- PageTableBaseRegister(PTBR)은 어느 PageTable을 가리키는 것이다.
- PageTableLengthRegister(PTLR)은 Table의 크기를 보관한다.
- 그래서 이 기법은 메모리는 2번 접근하게 된다.
    1. page table을 통해 주소 변환 
    2. 변환된 주소로 실제 물리적 주소 접근

→ 이 비용이 크기 때문에 속도 향상을 위해서 associative register or **translation look-aside butter(TLB)** 라는 빠른 일종의 캐시에 저장된다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/684bc9ca-27d3-473c-a9f3-0cfe2237fe2f/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/684bc9ca-27d3-473c-a9f3-0cfe2237fe2f/Untitled.png)

### Two Level Page Table (2단계 테이블 접근)

- 앞에서 봤던 것은 한번의 Page Table만 사용하지만 이번에는 2개의 Page Table을 사용함.
- 이것을 사용하게 된 이유는 시간이 아니라 **공간을 줄이기** 위해서 사용됐다.

> 현대 컴퓨터는 32bit 주소(논리적) 체계이다(요즘은 64bit)

→ 프로세스마다 PageTable이 필요한데 이를 다 만들기에는 공간의 낭비가 심하다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8fe61a0b-1fc5-4cb3-b152-254b6b03d559/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8fe61a0b-1fc5-4cb3-b152-254b6b03d559/Untitled.png)

- Outer page table에서는 Inner Page table들을 가르키고 있다.
- Inner page table에서는 물리적인 주소를 가지고 있다.

> 그런데 이렇게 하면 2번 접근하고 공간도 더 생기는 거 아냐?

→ 실제 한 프로세스 안에서 필요 없는 것들까지 메모리로 만들어 지는데 사용 되지 않는 곳들의 PageTable을 NULL로 두게 되면 많은 양의 공간을 절약할 수 있다.

- NULL 부분으로 접근하는 Outer Page Table만 만들고 사용 안하는 Inner Page Table을 많이 비워둘 수 있다.
