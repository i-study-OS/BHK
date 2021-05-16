![Screen Shot 2021-05-12 at 5.05.38 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210512170546.png)

![Screen Shot 2021-05-12 at 5.09.50 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210512170955.png)

사용되지 않는 entry에 대한 공간도 존재. 다만 invalid bit으로 표기

Valid == 페이지가 실제 메모리에 올라가 있다.

![Screen Shot 2021-05-12 at 5.12.25 PM](/Users/hwang-in-u/Library/Application Support/typora-user-images/Screen Shot 2021-05-12 at 5.12.25 PM.png)

![Screen Shot 2021-05-12 at 5.25.15 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210512172520.png)

![Screen Shot 2021-05-12 at 5.26.15 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210512172621.png)

시스템 안에 페이지 테이블이 딱 하나 존재

페이지 테이블의 entry가 물리적 페이지의 frame 갯수만큼 존재

F번째 frame을 가면 p번째 page가 나오도록 설정.

physical address를 보고 logical addresss로 바꾸는 테이블

이 테이블을 통해서는 논리 주소에 해당되는 p가 물리적 위치를 찾으려면 테이블의 entry를 모두 검색하여 p가 f번째 frame에 올라가있구나 찾을 수 있다.

이 방식을 쓰는 이유: 페이지 테이블의 공간을 줄이기 위해. 대신 시간적 오버헤드가 존재한다.

해당 테이블에는 p 번째 숫자, 그리고 어떤 프로세스의 것인지 알려주는 id가 존재한다.

associative register를 사용하여 entry를 병렬적으로 찾게 하여 시간적 오버헤드를 줄일 수 있다.

![Screen Shot 2021-05-12 at 5.35.34 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210512173544.png)



여러 프로세스가 공유할 수 있는 코드 부분을 같은 물리적 메모리 frame으로 매핑해주는 기법: Shared Page

![Screen Shot 2021-05-12 at 5.37.44 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210512173750.png)

동일한 물리적 메모리 위치를 공유하고 더 나아가 논리적 메모리 공간에서도 동일한 위치에 있어야 한다

![Screen Shot 2021-05-12 at 5.37.00 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210512173707.png)



#### Segmentation 기법

![Screen Shot 2021-05-12 at 6.06.43 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210512180647.png)

![Screen Shot 2021-05-12 at 6.07.28 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210512180734.png)

![Screen Shot 2021-05-12 at 6.08.26 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210512180832.png)

base: 각 segment의 시작 위치[주소]

Limit: 각 segment의 길이

![Screen Shot 2021-05-16 at 10.38.58 AM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210516103907.png)

테이블을 위한 메모리낭비를 고려하면 세그먼테이션 기법이 메모리 낭비가 더 적다.



![Screen Shot 2021-05-12 at 6.12.33 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210512181242.png)

![Screen Shot 2021-05-16 at 10.45.33 AM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210516104542.png)



#### Paged Segmentation

- 세그먼트 하나가 여러 페이지로 존재
  - 프로그램을 여러 세그먼트로 나눈다.
  - 세그먼트는 반드시 동일한 크기 페이지들의 집합으로 구성되어야 한다.
  - 물리적 메모리에 적재하는 단위는 페이지 단위로 한다.
- 두 단계 테이블 이용
- - 외부의 세그먼트 테이블
  - 각 세그먼트 마다 페이지 테이블 보유

![Screen Shot 2021-05-16 at 10.47.03 AM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210516104711.png)

1. segment번호를 통해 segment 테이블의 항목 접근
2. Segment항목: segment 길이 & segment page테이블 시작 주소
3. Segment table 항목: segment를 위한 페이지 테이블의 시작 위치 get
4. 해당 시작 위치로부터 페이지 번호만큼 떨어진 페이지 테이블 항목으로부터 물리적 메모리의 page frame get



Paging 기법과 Segmentation 기법의 장단점 취합

- 외부조각 문제 해결
  - 하나의 세그먼트 크기 == 페이지 크기의 배수
- 세그먼트 단위로 프로세스 간의 접근 권한 보호

주소변환에 있어서 운영체제의 역할: 주소변환을 위한 운영체제의 역할은 없다. 전부 다 하드웨어가 해줘야 하는 역할이다. 