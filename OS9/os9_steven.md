https://www.notion.so/stevenkim18/OS-8-2-48a862ce1c194b55ac353ee08d9f5254

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a0ec61b9-03df-42e4-a294-e2444c6e3d66/Screen_Shot_2021-05-15_at_2.40.19_PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a0ec61b9-03df-42e4-a294-e2444c6e3d66/Screen_Shot_2021-05-15_at_2.40.19_PM.png)

- 메모리의 공간이 더 커지면서 n단계의 PageTable를 사용할 수 있게 되었다.
- 그러면 메모리 접근이 n+1번 필요해서 시간이 많이 걸린다.
- 하지만 저번 시간에 배운 TLB 때문에 그렇지 않다.
- 대부분의 주소 변환 시간 동안에는 TLB를 사용한다.

## Valid / Invalid Bit in a Page Table

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4b5e5816-2c6f-40c3-892d-b4d420071d9e/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4b5e5816-2c6f-40c3-892d-b4d420071d9e/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/65ed45a2-3bee-433f-ae79-527e017f4e76/Screen_Shot_2021-05-15_at_2.56.48_PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/65ed45a2-3bee-433f-ae79-527e017f4e76/Screen_Shot_2021-05-15_at_2.56.48_PM.png)

- Page Table 공간에 물리주소와 한개의 비트를 더 둔다.
- 실제로 메모리에 사용되는 메모리 공간에는 v(vaild)
- 사용되지 않는 메모리 공간에는 i(invalid)

## Protection bit

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/874757c8-099e-4836-9e7d-9741d970923e/Screen_Shot_2021-05-15_at_2.59.53_PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/874757c8-099e-4836-9e7d-9741d970923e/Screen_Shot_2021-05-15_at_2.59.53_PM.png)

- 해당 페이지의 접근 권한을 나타낸다.
- 어떤 프로세스에 대한 Code 영역은 수정되어서는 안돼서 → read-only
- Data, Stack 영역은 → read, write 모두 부여

## Inverted Page Table

- 모든 프로세스가 공유하는 한개의 페이지 테이블을 만드는 것
- Page Table의 Entry는 물리적인 메모리의 Page Frame 개수 만큼 존재

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2a8a8d64-d6ed-4b85-ae04-581d4009b42d/Screen_Shot_2021-05-15_at_3.14.52_PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2a8a8d64-d6ed-4b85-ae04-581d4009b42d/Screen_Shot_2021-05-15_at_3.14.52_PM.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/57471950-640e-453c-8632-3eb8948873d6/Screen_Shot_2021-05-15_at_3.15.22_PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/57471950-640e-453c-8632-3eb8948873d6/Screen_Shot_2021-05-15_at_3.15.22_PM.png)

- 프로세스 마다 페이지 테이블을 생성해야 되는 문제 때문에 나오게 되었다.
- Page Table의 모든 공간을 탐색해야 되기 때문에 시간이 많이 소요된다.

## Shared Page

- 만약 동일한 프로그램을 여러개 띄워둔다고 생각해보자
- 그러면 프로세스는 여러개 생기지만 같은 코드를 사용하고 있다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5a068fe1-79b8-4d73-b850-b813efa98fa4/Screen_Shot_2021-05-15_at_3.36.37_PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5a068fe1-79b8-4d73-b850-b813efa98fa4/Screen_Shot_2021-05-15_at_3.36.37_PM.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/159fd3f5-a489-4871-a86e-4b39b86d3463/Screen_Shot_2021-05-15_at_3.36.48_PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/159fd3f5-a489-4871-a86e-4b39b86d3463/Screen_Shot_2021-05-15_at_3.36.48_PM.png)

- 공유되는 코드는 물리적인 주소에 한번 저장이 된다.
- 그 주소를 공유해서 사용한다(예시에서 물리적 주소 3, 4, 6)
- 이 공유된 코드들은 read-only로 하여 메모리 올려야 한다. 그리고 순서가 동일해야 한다.

## Segmentation

- 사전적 정의: 구분 분할
- 메모리를 의미 단위로 쪼갠것(Code, Stack, Data...)
- 프로그램 전체와 함수나 변수도 세그먼트로 나눌 수 있다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d77245a1-e00c-4bf2-8c5c-0344ffc48202/_2021-05-16__7.21.13.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d77245a1-e00c-4bf2-8c5c-0344ffc48202/_2021-05-16__7.21.13.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4c6f32c4-c4d8-4e36-ab44-489505b9fc12/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4c6f32c4-c4d8-4e36-ab44-489505b9fc12/Untitled.png)

- Paging 기법과 비슷하게 Base Register 와 Length Register를 가지고 있다.
- 다른 점은 각각 세그먼트의 길이를 추가적으로 가지고 있다(limit)

### Segmentation 장단점

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c3b35d8a-0128-49e8-808e-af8e6c2565b1/_2021-05-16__7.37.05.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c3b35d8a-0128-49e8-808e-af8e6c2565b1/_2021-05-16__7.37.05.png)

- 의미 단위로 쪼개기 때문에 권한 설정(read/write/read-only)하기 메모리를 보호할 때와 공유할 때 유리하다
- 반면 가변 분할 방식 처럼 Hole이 생겨 그 안에서도 적당한 위치를 찾아야 하는 오버헤드가 생긴다.

### 예시

- 5개의 세그먼트가 있는 경우

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d4d94094-7ed2-4a5f-a127-d735ad2bfa19/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d4d94094-7ed2-4a5f-a127-d735ad2bfa19/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/35f70dab-4e78-491d-a670-aa8187887d3f/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/35f70dab-4e78-491d-a670-aa8187887d3f/Untitled.png)

- 이 예시는 editor 메모리 영역은 같이 공유하고
- 각각 데이터 영역은 각자 저장한 경우이다.

## Segmentation with Paging

- 세그멘트는 각각 크기가 다르기 때문에 그 세그먼트를 일정한 크기로 나눠서 저장하는 기법

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8d7568ce-498a-4f9f-80e6-f7dfb46340f2/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8d7568ce-498a-4f9f-80e6-f7dfb46340f2/Untitled.png)

- 순서는 먼저 세그먼트에 대한 주소변환을 한다.
- 그리고 그 각 세그먼트에 대해 나눠서 저장한 PageTable에 접근해서 물리적 메모리에 접근한다.
- 이렇게 되면 Page 기법에 PageTable을 모두 만들어서 메모리를 낭비하는 것과 크기가 달라서 Hole이 생기는 문제를 해결할 수 있다.

## 충격..

- 주소 변환에 있어어 OS가 하는 역할은 없다.
- 전부 하드웨어가 한다(MMU, TLB...)

### 참고

[https://higunnew.tistory.com/60?category=873234](https://higunnew.tistory.com/60?category=873234)

[https://higunnew.tistory.com/61?category=873234](https://higunnew.tistory.com/61?category=873234)
