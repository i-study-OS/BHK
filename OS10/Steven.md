https://www.notion.so/stevenkim18/OS-9-1149d0b8254d4b6f9dff88ab5f9e4d4b

### 참고자료

- [https://higunnew.tistory.com/64](https://higunnew.tistory.com/64)

가상 메모리는 전적으로 운영체제가 관리한다.

- 페이징 기법을 사용한다고 가정한다 (대부분 페이징 기법을 사용)

## Demand Paging

- 요청이 있을 때 그 페이지를 메모리에 올리는 것
- 프로그램에서 자주 사용되는 부분은 극히 적다.
- 그래서 필요할 때만 페이지를 메모리에 올리는 것을 효율적이다.
- 한정된 메모리에서 모든 페이지를 올리는 것보다 필요할 때만 올리는 것이 응답이 빠르다

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9c735a27-d055-42c6-b4c8-e15afbebb3dc/_2021-05-22__9.17.25.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9c735a27-d055-42c6-b4c8-e15afbebb3dc/_2021-05-22__9.17.25.png)

- 이전에 invalid bit는 사용하지 않는 주소영역을 가르킨다고 했다.
- 또한 페이지가 물리적 메모리에 없는 경우도 invalid bit를 사용한다.(ssd에 있는 경우 → 백킹 스토어)
- 요청한 페이지가 물리적 메모리 없는 경우 → `page fault`

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5ddc16c9-285a-46a9-a805-eac28d38c084/_2021-05-22__9.34.04.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5ddc16c9-285a-46a9-a805-eac28d38c084/_2021-05-22__9.34.04.png)

- 페이지가 invalid 상태이면 trap으로 걸로 프로세스를 OS에게 준다.
- 그러면 백킹 스토어에서 I/O 로 저장되어 있는 페이지에 접근한다.
- 그 페이지를 물리적 메모리에 올린다.
- 그리고 다시 페이지 테이블에 접근하게 한다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f27b72f2-9829-470a-935f-a319f3684a29/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f27b72f2-9829-470a-935f-a319f3684a29/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/679c6c4e-13e8-450f-89bb-f715282e1497/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/679c6c4e-13e8-450f-89bb-f715282e1497/Untitled.png)

## Page Replacement

- 물리적 메모리에 빈 페이지가 없으면 기존에 있던 페이지를 백킹스토어롤 쫓아내는 것을 말한다.
- 운영체제가 하는 역활이다!
- 최대한 적게 page fault가 나는 것이 좋다.
- 이에 대한 다양한 알고리즘을 알아보자!(Replacement 알고리즘)

### Optimal 알고리즘

- page fault를 가장 적게 내는 알고리즘

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/60ae2727-3b5a-4b6d-a344-dd02641a3156/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/60ae2727-3b5a-4b6d-a344-dd02641a3156/Untitled.png)

- 미래에 참조 될 페이지들을 미리 전부 안다고 가정하자.(다른 말로 오프라인 알고리즘이라고 함)
- 가장 나중에 오는 페이지를 쫓아내는 것이다.
- 하지만 미래를 예측할 수 없기에 실제 적용은 어려움.
- 예시에는 6번의 Page fault가 남.
- 아무리 좋은 알고리즘을 만들어도 이 알고리즘 보다는 좋게 만들기 어려움.

### FIFO 알고리즘

- 큐와 같은 알고리즘

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/219a84ed-870f-4736-8438-08dd4718badf/_2021-05-22__11.24.32.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/219a84ed-870f-4736-8438-08dd4718badf/_2021-05-22__11.24.32.png)

- 큐의 페이지 수를 늘릴 수록 Page fault가 늘어나는 기이한 현상이 일어난다.

### LRU 알고리즘

- 가장 오래전에 사용된 것을 쫓아냄.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/70b1fccb-863b-4839-9e1e-3c962aefb29e/_2021-05-22__11.25.38.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/70b1fccb-863b-4839-9e1e-3c962aefb29e/_2021-05-22__11.25.38.png)

> 미래를 모르면 과거를 봐라!

### LFU 알고리즘

- 가장 사용적이 적게 된 페이지를 쫓아냄
- 사용 빈도를 보는 알고리즘

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8e5fd266-eede-4331-9830-fc90c0111170/_2021-05-22__11.28.12.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8e5fd266-eede-4331-9830-fc90c0111170/_2021-05-22__11.28.12.png)

### LRU, LFU 알고리즘 예제

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c1d68c37-6c4e-41c9-9547-c327a537937f/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c1d68c37-6c4e-41c9-9547-c327a537937f/Untitled.png)

- LRU 알고리즘의 경우 가장 나중에 사용된 1번을 지움 → 문제점은 가장 많이 사용되었다는 것
- LFU 알고리즘의 경우 가장 적게 사용된 4번을 지움 → 문제점은 가장 최근에 사용되었다는 것
- 이를 해결하기 위해 많은 학자들이 연구를 함.

### LRU, LFU 알고리즘 구현

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1b272dbe-f148-4825-b00c-b96708dcebb1/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1b272dbe-f148-4825-b00c-b96708dcebb1/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ce191186-8a17-4154-8a58-f774bc4a615f/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ce191186-8a17-4154-8a58-f774bc4a615f/Untitled.png)

- LRU은 시간 순서대로 나열을 함.
- 내부를 더블 링크 리스트로 구현
- LFU는 모든 노드를 순회하면서 참조 횟수를 비교하면서 움직여야함 → 최악의 경우의 시간 복잡도는 O(n)이 될 수 있음.
- 그래서 왼전 이직 트리로 구현하면 시간 복잡도가 개선 됨

---

## 다양한 Cashing

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/648b5080-e366-4edd-9a94-54c6125a1a35/_2021-05-23__1.12.14.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/648b5080-e366-4edd-9a94-54c6125a1a35/_2021-05-23__1.12.14.png)

- Cashing은 다양한 컴퓨터 시스템에서 쓰인다.(캐쉬 메모리, 웹페이지 캐쉬 등)
- Paging 기법도 일종의 Cashing이다!
- Page fault가 나면 그때 OS에게 자원이 넘어가서 백킹스토어에 접근을 한다. 그전까지는 Page의 참조시간을 OS가 알 수 없다. → 따라서 저번시간에 배운 LRU, LFU또한 사용할 수 없다.(그럼 왜 배운거지...)

> 가상 메모리에서 사용되는 알고리즘은 Clock 알고리즘이다.

## Clock 알고리즘(Second-Change 알고리즘)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b9363b26-76f5-4123-ad98-099331b6ddef/_2021-05-23__1.23.39.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b9363b26-76f5-4123-ad98-099331b6ddef/_2021-05-23__1.23.39.png)

- bit를 바꾸는 것은 OS가 아닌 하드웨어가 한다(하드웨어의 도움을 받음)
- 추가적으로 modified bit를 사용한다, 1일 때는 페이지에 올라와서 수정이 된것이고 0은 물리적메모리에 올라왔지만 사용이 되지 않았다. 그래서 0은 그냥 쫓아 내기만 하면 되고 1은 수정되어있기 때문에 다시 백킹스토어에 저장해야 한다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a7aeecd6-24b4-47c7-80d6-bd070174dbe9/_2021-05-23__1.26.00.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a7aeecd6-24b4-47c7-80d6-bd070174dbe9/_2021-05-23__1.26.00.png)

- 네모는 각 Page 프레임을 의미함.
- 시계 바늘처럼 돌면서 확인
- 1은 시계바늘이 다시 오기전까지 페이지가 사용된 것이고 0은 사용되지 않은 것
- 그래서 1은 0으로 바꾸고 0은 쫓아내는 방법

## Page Frame의 Allocation

- 우리가 실제 컴퓨터를 사용할 때는 여러개 프로그램이 메모리에 올라가 있다.
- 그런데 한개의 프로그램이 동작 할 때 page fault가 자주 일어나면 매우 비효율적이다.
- 그래서 어느 일정 부분 자주 사용되는 부분은 할당을 받아야 한다.
- 할당을 해주는 방법은 아래 여러개가 있다(분홍색 글씨들)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/eefa2094-b142-46c7-9f43-e47c782eda07/_2021-05-23__8.06.37.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/eefa2094-b142-46c7-9f43-e47c782eda07/_2021-05-23__8.06.37.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4023ed3a-6a41-41d9-8123-adf76c4fd08d/_2021-05-23__8.12.40.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4023ed3a-6a41-41d9-8123-adf76c4fd08d/_2021-05-23__8.12.40.png)

- Global replacement는 다른 프로세서에 할당된 프레임을 뻇을 수 있다.
- Local replacement는 각 프로그램 별로 일정 부분을 할당안에서 그 안에서 나눠서 사용하는 것이다
- 이 두 방법 모두 컴퓨터 시스템 안에서 쓰인다.

> 프레임이 적게 할당이 되어서 Page fault가 자주 일어나는 것을 Thrashing이라고 한다.

## Thrashing(쓰래슁)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/819782d5-d7a7-4e4c-afb8-d90f471d3e32/_2021-05-23__8.16.25.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/819782d5-d7a7-4e4c-afb8-d90f471d3e32/_2021-05-23__8.16.25.png)

- 여러개의 프로그램이 동시 올라갈 수록 cpu 이용률이 높아진다.
- 그런데 어느 순간부터 프레임이 부족하게 되면 계속 Page fault가 나서 계속 백킹스토어에 접근을 해야하는 I/O가 발생한다 그러면 어느 순간 cpu는 일하는 시간이 적어져 이용률이 뚝 떨어진다. 이것을 Thrashing이라고 한다.

> 어느 정도의 프로그램이 올라 올지를 조절해줘야 한다

## Working-Set 알고리즘

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c862323a-8762-46f2-8c31-40f22a55f51a/_2021-05-23__8.28.34.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c862323a-8762-46f2-8c31-40f22a55f51a/_2021-05-23__8.28.34.png)

- 페이지 프레임을 참조하는 특정 길이를 정해서 한칸식 움직이면서 몇개의 프레임이 집중적으로 참조되었는지 확인함.
- 그리고 그 프레임들의 개수가 지금 내가 들고 있는 프레임의 개수면 들고 있고 아니면 전부를 swap out 시킨다.

 

## PFF(Page-Fault-Frequency) Scheme

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/16b9a7f7-c53a-4460-9cb0-782ca7511399/_2021-05-23__8.33.01.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/16b9a7f7-c53a-4460-9cb0-782ca7511399/_2021-05-23__8.33.01.png)

- Page fault가 일어나는 비율의 상한선, 하한선을 둔다.
- Page fault가 많이 발생을 하면 프레임을 더 주고 적게 발생하면 프레임을 빼았는 방법이다.

## Page size의 결정

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/de7aa1a2-1ba7-4f51-aab8-272f7531b413/_2021-05-23__8.37.09.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/de7aa1a2-1ba7-4f51-aab8-272f7531b413/_2021-05-23__8.37.09.png)

- 최근에는 Page size를 키워주는 추세!
