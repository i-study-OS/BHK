https://www.notion.so/stevenkim18/OS-10-e03f6ac15339414996a651e630a2c94e


참고자료

[https://velog.io/@injoon2019/운영체제-운영체제-반효경-교수님-2017년-12.-파일시스템#file-protection](https://velog.io/@injoon2019/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-%EB%B0%98%ED%9A%A8%EA%B2%BD-%EA%B5%90%EC%88%98%EB%8B%98-2017%EB%85%84-12.-%ED%8C%8C%EC%9D%BC%EC%8B%9C%EC%8A%A4%ED%85%9C#file-protection)

## 파일, 파일 시스템

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/70e41c16-83e9-4fa0-b37c-6a3ceaefd21e/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/70e41c16-83e9-4fa0-b37c-6a3ceaefd21e/Untitled.png)

### 파일

- 하드디스크에 저장되는 단위(비휘발성, 보조기억장치)
- 메모리는 주소를 통해서 접근 ↔  **파일은 이름을 통해서 접근**
- 파일의 연산중 reposition(lseek)는 파일의 읽고 있는 포인터를 수정해주는 것

> Open하고 Close는 왜 할까? → 파일의 메타데이터를 메모리 올리고 내리는 작업

### 파일 속성(메타 데이터)

- 파일의 각종 정보들

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/dc488e87-eb2a-4d30-920d-cf44cd89dc45/_2021-05-29__3.15.19.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/dc488e87-eb2a-4d30-920d-cf44cd89dc45/_2021-05-29__3.15.19.png)

### 파일 시스템

- 운영체제에서 파일을 관리하는 부분
- 파일, 메타데이터를 관리

## 디렉토리와 논리적 디스크

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7c202cac-f2b9-4459-bab1-0749b6030ed3/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7c202cac-f2b9-4459-bab1-0749b6030ed3/Untitled.png)

### 디렉토리(폴더)

- 디렉토리도 하나의 파일
- 디렉토리는 자기 안에 파일들의 메타데이터를 가지고 있다.
- 파일 찾기, 생성, 지우기, 리스트, 이름 바꾸기, 파일 시스템을 전체 탐색 등의 연산을 할 수 있다.

### 파티션(논리적 디스크)

- 운영체제가 보는 것은 물리적 디스크가 아닌 논리적 디스크

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c745d00f-4064-4901-8641-b820034c1c82/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c745d00f-4064-4901-8641-b820034c1c82/Untitled.png)

## Open()

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0a34e420-fa65-47ac-9098-ad37f87d7cb2/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0a34e420-fa65-47ac-9098-ad37f87d7cb2/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/174a2cac-d26f-42aa-82e3-83ab7858c6a9/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/174a2cac-d26f-42aa-82e3-83ab7858c6a9/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c03234f6-30db-4dbc-bf51-d9b2f77fa04e/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c03234f6-30db-4dbc-bf51-d9b2f77fa04e/Untitled.png)

- A 프로세스가 open 함수를 사용함 → 시스템 콜이라서 프로세스가 운영체제에게 넘어감.
- 오픈된 파일이 전역으로 관리 되는 `Open file table` 이 있다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/eb64eddf-23c6-4432-a789-4c969856a021/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/eb64eddf-23c6-4432-a789-4c969856a021/Untitled.png)

- 운영체제의 Root 디렉토리의 메타데이터를 가지고 있다.
- Root의 메타데이터를 Open 한다.
- Root 폴더 안에서 a의 메타데이터를 접근해서 다시 연다
- 이 작업을 찾을 때까지 반복한다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/845fa8b3-23cd-43ad-95c7-396c7b268746/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/845fa8b3-23cd-43ad-95c7-396c7b268746/Untitled.png)

- Open 함수가 리턴하는 값은 각 프로세스에 있는 파일 디스크립터(fd) 값을 반환한다.
    - 보통 3부터 시작(0, 1, 2 표준 입출력 애러)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/dd817f60-0434-4cdd-a998-745d661b1e54/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/dd817f60-0434-4cdd-a998-745d661b1e54/Untitled.png)

- 오픈한 파일을 읽을 때 이미 올라온 FD에 접근만 하면 된다.
- 그러면 그 fd는 파일의 메타데이터를 가리키고 있고 그 메타데이터는 파일의 내용을 가르키고 있다.
- 파일의 내용을 메모리의 작은 부분에 저장하는 데 이를 버퍼 캐시라고 한다.
- 메모리 안에 버퍼 캐시에 저장을 하면 다시 사용자 read 요청을 했을 때 캐시에 있는 것을 바로 주면 된다.
- 이 버퍼에 대한 권한은 모두 OS가 가지고 있기 떄문에 LRU, LFU 알고리즘이 사용 가능하다.
- 여러 프로세스에 같은 파일을 접근할 수 있다. 각 프로세스 마자 offset을 둔다.

## 파일 보호

- 메모리에서는 read, write에서만 신경 썼다.
- 파일은 여러 곳에 사용되기 떄문에.. 접근권한 유저와, 어디까지 연산이 가능한지를 동시에 가지고 있어야 한다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9af1eda0-d7c7-47f6-ae6e-1993e4aab893/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9af1eda0-d7c7-47f6-ae6e-1993e4aab893/Untitled.png)

### Access control Matrix

- 파일과 사용자의 사용권한을 표로 만든 것

### Grouping

- 3bit씩 사용자 권한을 표시(9bit)
- 유닉스에서 사용(맥 터미널에서 ls -al)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/03944520-d877-455b-bd35-2c82d7355931/_2021-05-29__4.44.53.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/03944520-d877-455b-bd35-2c82d7355931/_2021-05-29__4.44.53.png)

### Password

- 파일 마다 비밀번호를 부여
- 비번을 암기 및 관리 문제가 생김.

> 다른 디스크에 파일 시스템을 접근하려면 어떻게 해야 되냐 → Mounting

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ced006cc-cfd5-4eac-817c-73ccf84ced6e/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ced006cc-cfd5-4eac-817c-73ccf84ced6e/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ee9c2708-8bef-4873-90a2-ddee7dc76ce8/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ee9c2708-8bef-4873-90a2-ddee7dc76ce8/Untitled.png)

## Access Methods

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c190f435-6278-4bcb-95c8-76df4c687717/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c190f435-6278-4bcb-95c8-76df4c687717/Untitled.png)

---

## Allocation of File Data in Disk(파일 저장 방법)

### 연속할당 방법(Contiguous Allocaiton)

- 파일을 연속적인 블록에 저장하는 방법

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9281ab1b-6403-437f-810c-063e6c28aecb/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9281ab1b-6403-437f-810c-063e6c28aecb/Untitled.png)

- 디렉토리에는 파일의 시작 블럭과 길이만 가지고 있다.(다른말로 메타데이터를 가지고 있다)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/196fe13b-d8c4-4db4-9ad1-d050ea4b0e64/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/196fe13b-d8c4-4db4-9ad1-d050ea4b0e64/Untitled.png)

장점

- 빠른 io가 가능하다
- 하드디스크는 스와핑 용도로 사용되기도 한다.
- 직접 접근이 가능하다.

단점

- 외부 조각이 생길 수 있다
- 파일의 용량이 커질 때 어려움이 생길 수 있다.

### 링크로 저장하는 법(Linked Allocation)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/40643d65-d73b-4f1a-b954-fbcf2d47f709/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/40643d65-d73b-4f1a-b954-fbcf2d47f709/Untitled.png)

- 디렉토리의 메타데이터는 시작과 끝의 위치만 가지고 있다.
- 연결 리스트 처럼 연결이 되어 있다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9158a778-76d5-4dd1-b156-b35f09757a83/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9158a778-76d5-4dd1-b156-b35f09757a83/Untitled.png)

장점

- 외부조각이 발생 x

단점

- 직접 접근 불가능하다
- 신뢰성 문제 - 중간의 하나의 포인터가 사라지면 나머지도 끊긴다.
- 공간의 효율성이 떨어짐

변형

- FAT
- 포인터를 별도 위치에 보관

### 인덱스로 저장하는 방법(Indexed Allocation)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/60ca2080-3002-4ed4-a322-cd2c3e160dee/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/60ca2080-3002-4ed4-a322-cd2c3e160dee/Untitled.png)

- 하나의 블럭에 인덱스들을 저장한다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a2e46520-013e-42b4-86db-421dbf1ef4a6/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a2e46520-013e-42b4-86db-421dbf1ef4a6/Untitled.png)

장점

- 외부 조각이 발생하제 않음
- 직접 접근 가능

단점

- 작은 파일일 경우 공간이 낭비됨
- 큰 파일일 경우 한개 블럭에 모든 인덱스를 담을 수 없음.

해결방안

1. 마지막 위치는 또 다른 블럭의 위치를 가르킴(linker scheme)
2. 2단계 페이지 테이블 같이(multi-level index)

> 실제는 어떻게 쓸까..

## 유닉스 파일시스템의 구조

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/893df44c-f6f1-441d-beca-b3f0f29a9302/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/893df44c-f6f1-441d-beca-b3f0f29a9302/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/53110138-c848-4cb9-b9f6-4b02a4f9a3cb/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/53110138-c848-4cb9-b9f6-4b02a4f9a3cb/Untitled.png)

### Boot Block

- 어떤 파일 시스템이던 제일 먼저 나온다
- 컴터가 부팅 할 때 사용 된다.

### Super Block

- 어디가 빈 블록이고 어디가 사용되고 있는 블록, 블록 간의 경계에 대한 전반적인 정보를 담고 있다.

### Inode list

- 실페 파일의 메타 데이터를 가지고 있다.
- 각 inode는 크기가 정해져 있다.

### Data Block

- 여기에 실제 파일이 저장되는 것 같지만 **파일의 이름**과 **inode 번호**만 가지고 있다

### 유닉스는 파일을 어떻게 가지고 있을까?

- 작은 파일이라면 direct blocks에만 가지고 있다.
- 파일이 커질 수록 single indirect → double → triple 로 단계 별로 저장을 한다.(단계별 페이지 같은)

## FAT 파일 시스템

- MS-DOS가 채용한 구조, 윈도우에서 일부 사용되고 있음

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8c3b96d1-24df-4c86-be80-e6e7aff93dfb/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8c3b96d1-24df-4c86-be80-e6e7aff93dfb/Untitled.png)

- 유닉스와는 다르게 Data block의 파일의 대부분의 정보를 가지고 있다.
- 그리고 FAT에 그 블럭이 시작 되는 위치를 가지고 있다(217)
- 그러면 FAT에 링크드 방식으로 저장되어 있는 위치를 계속 따라간다. End Of File이 나올 떄까지
- 이렇게 해서 각각 나눠서 저장된 파일을 불러올 수 있다.
- 장점은 직접 접근이 가능하다.
- 링크드로 접근하는 방식의 단점을 보안했다.
- 보통은 FAT은 중요한 정보라서 2Copy 이상을 떠둔다.

> 파일 시스템은 상당히 종류가 많다.

## 비어 있는 공간 관리 하기(Free Space Management)

### Bit map을 사용

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/45eb3867-1381-4e8b-91f2-f88eed135c7f/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/45eb3867-1381-4e8b-91f2-f88eed135c7f/Untitled.png)

- Data Block에 각 블럭이 비어있는지 아닌지를 비트로 표시하는 방법
- Super Block에 Bit map을 둠

### Linked list 사용

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0dacd778-2919-4733-8b82-02768684fbdc/_2021-05-30__1.33.21.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0dacd778-2919-4733-8b82-02768684fbdc/_2021-05-30__1.33.21.png)

- 비어있는 블록을 링크드 리스트로 구현한다.
- 추가적인 공간 낭비는 없다.
- 그런데 연속적인 공간을 찾기는 어렵다.(직접접근 불가)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4f6c1706-6e93-4f0b-b543-4109d4a1e681/_2021-05-30__1.33.32.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4f6c1706-6e93-4f0b-b543-4109d4a1e681/_2021-05-30__1.33.32.png)

### Grouping

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4833de2d-f049-492d-869d-0aac2b1a0f6d/_2021-05-30__1.34.52.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4833de2d-f049-492d-869d-0aac2b1a0f6d/_2021-05-30__1.34.52.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4735b183-d4cc-4cae-ba83-f9b9e1b5254e/_2021-05-30__1.34.59.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4735b183-d4cc-4cae-ba83-f9b9e1b5254e/_2021-05-30__1.34.59.png)

- 파일 저장 방식 중에 인덱스로 저장하는 방식과 유사하다
- 하나의 빈 블록에 다른 빈블록의 인덱스를 넣는다.
- 그래서 그룹을 지어준다.

### Counting

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/809b670f-d545-4737-9b65-1635662a3227/_2021-05-30__1.37.00.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/809b670f-d545-4737-9b65-1635662a3227/_2021-05-30__1.37.00.png)

- 빈블록의 첫번째 위치가 연속적으로 몇개의 블록이 비어있는지를 같이 저장한다.

## 디렉토리의 구현(Directory Implementation)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/df0965be-149b-4bbc-ab65-9503d22a833f/_2021-05-30__1.38.04.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/df0965be-149b-4bbc-ab65-9503d22a833f/_2021-05-30__1.38.04.png)

### Linear list

- 고정된 크기에 파일이름과 파일의 메타데이터를 저장한다
- 구현이 간단하지만, 찾는데 처음 부터 일일히 확인을 해야한다는 단점이 있다.

### Hash Table

- 위 방식에 해쉬 함수를 적용하는 방법이다.
- 파일이름에 해쉬 함수를 적용하면 특정 범위(1<x<n) 안에 숫자를 리턴한다.
- 이것을 적용한 것을 인덱스로 해서 메타데이터를 저장한다.
- 해쉬함수만 적용을 하면 되서 검색 시간이 없어진다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/07243fdf-aa18-4b32-8fec-8f1386c360f2/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/07243fdf-aa18-4b32-8fec-8f1386c360f2/Untitled.png)

## VFS와 NFS

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0f1db2e1-dba8-457f-883e-8db6c94c0d98/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0f1db2e1-dba8-457f-883e-8db6c94c0d98/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8d708e1a-0b43-44c1-9b48-728a00daaf87/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8d708e1a-0b43-44c1-9b48-728a00daaf87/Untitled.png)

- OS 안에서 다양한 파일시스템이 존재한다.
- 그 사이 중간에 가상 파일 시스템을 주어서 OS가 모든 파일 시스템에 대해서 같은 대응을 하게 해준다.
- 네크워트을 통해서 파일 시스템에 접근하고 집으면 NFS를 사용하면 된다.

## Page Cache and Buffer Cache

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7073f374-d4e8-407e-9cc9-9cb526e7c1c2/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7073f374-d4e8-407e-9cc9-9cb526e7c1c2/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/80881967-b916-41ec-8de4-bb8f64270078/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/80881967-b916-41ec-8de4-bb8f64270078/Untitled.png)
