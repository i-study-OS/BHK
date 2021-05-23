#  Virtual Memory

---

#### 가상 메모리(Virtual Memory)란?

> 가상 메모리는 메모리를 관리하는 방법의 하나로, 각 프로그램에 실제 메모리 주소가 아닌 가상의 메모리 주소를 주는 방식을 말한다. 이러한 방식은 멀티태스킹 운영 체제에서 흔히 사용되며, 실제 주기억장치(RAM) 보다 큰 메모리 영역을 제공하는 방법으로 사용된다.

"프로그램이 실행되기 위해 그 프로세스의 주소 공간 전체가 메모리에 올라와 있어야 하는 것은 아니다. 따라서 운영체제는 CPU에서 당장 수행해야 할 부분만을 메모리에 올려놓고 그렇지 않은 부분은 디스크의 스왑 영역(swap area)에 내려놓았다가 다시 필요해지면 메모리에 올라가 있는 부분과 교체하는 방식을 사용한다...운영체제는 프로그램이 물리적 메모리를 고려할 필요 없이 자기 자신만이 메모리를 사용하는 것처럼 가정해 프로그램하는 것을 지원한다.이렇게 되면 프로그램은 0번지부터 시작하는 자기 자신만의 메모리 주소 공간을 가정할 수 있는데, 이러한  메모리 공간을 **가상메모리(virtual memory)**라고 부른다."

"즉 가상메모리는 프로세스마다 각각 0번지부터 주소공간을 가지게 되며, 이들 공간 중 일부는 물리적 메모리에 적재되고 일부는 디스크의 스왑 영역에 존재하게 된다." 운영체제와 정보기술의 원리 pg216



가상 메모리 기법은 전적으로 **운영체제**가 관여한다.

Paging 기법을 사용한다고 가정하고 가상 메모리 기법에 대해 알아보자.

![Screen Shot 2021-05-23 at 11.28.00 AM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210523112809.png)

- 완성도 높은 소프트웨어 == 대체로 방어적인 코드

  - 필요할 때 마다 페이지를 메모리에 올린다.

    ↓ I/O 양 → ↑응답속도

    물리적 메모리양 사용량 ↓

    멀티  프로그래밍 환경에서 더 많은 사용자 프로그램 수용

  - 응답 시간에 대한 다른 이견

    한정된 메모리 공간에서 많은 프로세스를 로드하여 실행하면 한 번에 모든 페이지를 로드하는 것 보다 응답 시간의 측면에서도 더 나을 수 있다.

![Screen Shot 2021-05-23 at 11.29.38 AM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210523112945.png)

- Invalid bit:
  - 사용되지 않는 주소 영역
  - 페이지가 Disk에 있는 경우 [물리적 메모리에 없다]



- initial Stage: 처음에는 모든 page entry는 invalid로 초기화 된다
- 주소변환시 해당 page entry가 invalid bit로 set 되어 있으면 해당 page는 baking store에 있을 것이기에 I/O 작업이 필요하다.
- 이때 MMU에 이해 page fault trap(software interrupt)이 발생한다.

![Screen Shot 2021-05-23 at 11.30.49 AM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210523113054.png)

![Screen Shot 2021-05-23 at 11.31.13 AM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210523113119.png)



![Screen Shot 2021-05-23 at 11.31.28 AM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210523113136.png)



![Screen Shot 2021-05-23 at 11.31.45 AM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210523113151.png)



![Screen Shot 2021-05-23 at 11.32.13 AM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210523113227.png)

- OS의 업무
  - 어떤 Page를 쫒아낼지 결정하는 것

![Screen Shot 2021-05-23 at 11.32.32 AM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210523113237.png)



![Screen Shot 2021-05-23 at 11.33.14 AM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210523113321.png)



![Screen Shot 2021-05-23 at 11.33.35 AM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210523113341.png)



![Screen Shot 2021-05-23 at 11.34.15 AM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210523113425.png)



![Screen Shot 2021-05-23 at 11.34.40 AM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210523113445.png)



![Screen Shot 2021-05-23 at 11.35.22 AM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210523113528.png)

- LRU 알고리즘의 예시
  - 한 페이지가 한 번만 참조되면 해당 페이지의 노드의 우선순위를 가장 높게 set
  - dobuly linked list를 활용
- LFU를 LinkedList로 구현하게 된다면?
  - 특정 노드의 페이지가 참조되어도 바로 내려갈 수 없음
  - 밑에 있는 다른 페이지를 참조하는 노드와 비교를 하면서 내려간다.
  - 최악의 경우 모든 노드를 비교해야한다.

![Screen Shot 2021-05-23 at 11.36.36 AM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210523113640.png)

- LFU는 heap을 이용하여 구현
- 시간 복잡도 개선

![Screen Shot 2021-05-23 at 11.36.52 AM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210523113656.png)

- Paging 기법도 캐싱의 한 예
- Page fault는 cashe miss



**캐시 교체 알고리즘의 제약**

- Buffer caching & Web caching

  - 삭제할 항목은 0(1)~ )(logn) 안에 결정되어야 한다.
  - LRU & LFU 사용가능

- Paging System

  - 메모리에 page가 이미 있는 경우 OS는 CPU 제어권을 갖지 않는다.

    따라서 OS는 LRU 또는 LFU알고리즘을 실제로 적용할 수 없습니다.

    Page Fault 이후 trap에 걸렸을 때만 OS가 CPU를 갖게 되기 때문이다.

![Screen Shot 2021-05-23 at 11.38.07 AM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210523113813.png)



![Screen Shot 2021-05-23 at 11.38.42 AM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210523113847.png)

- bit이 0인 것을 찾은것은 한 번 회전할 때 까지 해당 페이지를 참조한 적이 없다는 것을 의미
- bit 세팅은 OS가 아니라 하드웨어가 한다.
- 오랫동안 참조 되지 않은 페이지를 쫒하낸다 == LRU와 비슷한 효과



Clock Algorithm의 개선

- dirty bit
  - Dirty bit 1 == 적어도 한 번은 내용을 수정한 페이지라 가정
  - 쫒아내려면 backing store에 수정된 내용을 반영하고 실행 가능
- 왠만하면 dirty bit가 1인것을 쫒아내지 않으며 성능 향상에 기여

![Screen Shot 2021-05-23 at 11.39.22 AM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210523113927.png)



프로그램이 원활하게 실행되기 위해서는 page fault를 잘 내지 않으며 일련의 page들이 메모리에 함께 올라와 있어야 한다.

![Screen Shot 2021-05-23 at 11.39.56 AM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210523114022.png)



![Screen Shot 2021-05-23 at 11.40.32 AM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210523114038.png)



![Screen Shot 2021-05-23 at 11.40.56 AM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210523114100.png)



![Screen Shot 2021-05-23 at 11.41.15 AM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210523114120.png)



![Screen Shot 2021-05-23 at 11.42.05 AM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210523114209.png)

**Working-Set Algorithm**

- MultiProgramingDegree를 조정해 주는 것
  - 프로세스가 특정 시간 동안 일정 장소만을 집중적으로 참조하는 특징을 갖고 있다.
  - 한 프로세스가 일정 시간 동안 원활하게 수행되기 위해 한꺼번에 메모리에 올라와 있어야 하는 page set을 

![Screen Shot 2021-05-23 at 11.42.24 AM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210523114227.png)



![Screen Shot 2021-05-23 at 11.42.46 AM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210523114249.png)



![Screen Shot 2021-05-23 at 11.43.29 AM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210523114332.png)

- Dis transfer의 효율성 감소
  - disk는 HEAD가 이동할 때는 많은 양의 데이터를 읽어서 메모리로 올리는게 좋은데 Page가 작으면 Page fault가 일어날 때 너무 큰 비용이 든다.
  - Locality의 측면에서는 page size가 작은 것이 좋지 않다. 하지만 필요한 정보들만 메모리에 올릴 수 있어서 메모리 이용에 효율적일 수 있다.

최근에는 Page size를 점점 키우고 있는 추세이다. [기본적으로 4kb]