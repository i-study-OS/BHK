# File and File System

![Screen Shot 2021-05-30 at 8.04.15 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210530200425.png)

- reposition(lseek): 파일의 특정 부분부터 읽기 위해 포인터의 위치를 수정해 주는 연산
- open: 파일의 metadata를 메모리에 올려 놓는 연산



# Directory and Logical Disk

![Screen Shot 2021-05-30 at 8.08.46 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210530200850.png)

하위 디렉토리의 metadata를 내용으로 가지는 file

## Open()

![Screen Shot 2021-05-30 at 8.10.05 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210530201014.png)

![Screen Shot 2021-05-30 at 8.10.51 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210530201056.png)

![Screen Shot 2021-05-30 at 8.14.37 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210530201455.png)

사용자 프로세스 → 시스템 콜[운영체제가 CPU를 get]

운영체제는 root directory의 메타데이터를 메모리에 올리게 된다.

그러면서 운영체제는 각 프로세스의 PCB 또는 프로세스 제어블록에 오픈한 metadata의 포인터를 file discripter table에 저장한다..[fd]

경로의 metadata를 읽고 나면 해당 파일을 바로 전달하는 것이 아니라 카피본을 운영체제에 남겨두고 프로세스에게 전달한다

- 다른 프로세스가 동일한 요청을 하면 바로 건내 줄 수 있다.
- 버퍼 캐쉬 역할

운영체제에게 CPU가 넘겨져있기 때문에 LRU 사용가능



# File Protection

![Screen Shot 2021-05-30 at 8.26.44 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210530202649.png)



# File System의 Mounting

![Screen Shot 2021-05-30 at 8.28.05 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210530202815.png)

다른 파티션에 설치되어있는 파일접근을 위해서는 mounting이 필요하다.

![Screen Shot 2021-05-30 at 8.29.07 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210530202911.png)



# Access Methods

![Screen Shot 2021-05-30 at 8.29.55 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210530202959.png)

# Allocation of File Data in Disk

파일을 디스크에 저장하는 방법 3가지

![Screen Shot 2021-05-30 at 8.30.55 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210530203059.png)

## Contiguous Allocation

![Screen Shot 2021-05-30 at 8.31.44 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210530203149.png)

![Screen Shot 2021-05-30 at 8.32.50 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210530203256.png)



## Linked Allocation

![Screen Shot 2021-05-30 at 8.33.56 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210530203359.png)

![Screen Shot 2021-05-30 at 8.34.30 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210530203435.png)



## Indexed Allocation

![Screen Shot 2021-05-30 at 8.35.32 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210530203535.png)

한 파일이 어떻게 나뉘어져있는지 포인터를 담아두는 index block 구현

- linked scheme: 인덱스 블록의 파일의 위치 정보를 끝까지 적다가 끝까지 갔는데 커버를 못하면 또 다른 인덱스 블록을 가리키도록 설정하는 것. 마지막 인덱스를 따라가면 나머지 파일의 위치 정보 확인 가능. 큰 파일도 커버 가능하도록 구현
- multi-level index: 하나의 인덱스 블록이 파일의 위치를 가리키는 것이 아니라 또 다른 인덱스 블록을 가리키도록 구현



# UNIX 파일 시스템의 구조

![Screen Shot 2021-05-30 at 8.41.25 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210530204129.png)

![Screen Shot 2021-05-30 at 8.40.38 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210530204041.png)

 inode List: UNIX 시스템에서는 디렉토리에 파일의 모든 메타데이터를 가지고 있지 않고 대 부분의 데이터를 inode list에 저장한다. 따라서 파일 하나당 하나의 inode를 갖고 있게 된다.

Unix 시스템은 기본적으로 indexed allocation을 활용하여 inode list를 구현한다.

파일의 이름은 directory에 저장되어 있다.



# FAT File System

![Screen Shot 2021-05-30 at 8.46.35 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210530204638.png)

파일의 메타 데이터의 일부분을 FAT에 저장한다.

- 데이터의 위치 정보 [작은 테이블]
  - 다음 블록의 번호
  - 직접접근 가능: FAT을 통해 블록의 테이블이 메모리에 올라가 있기 때문에 실제 데이터 블록을 바로 알 수가 있다.

나머지 정보는 디렉토리에 저장한다.

- 파일 이름, 접근권환, 첫 번째 위치



# Free-Space Management

![Screen Shot 2021-05-30 at 8.52.02 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210530205205.png)

![Screen Shot 2021-05-30 at 8.52.22 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210530205228.png)

![Screen Shot 2021-05-30 at 8.52.42 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210530205246.png)



# Directory Implementation

![Screen Shot 2021-05-30 at 8.53.19 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210530205326.png)

![Screen Shot 2021-05-30 at 8.54.20 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210530205425.png)



# VFS and NFS

![Screen Shot 2021-05-30 at 8.54.55 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210530205458.png)

![Screen Shot 2021-05-30 at 8.55.03 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210530205508.png)



# Page Cache and Buffer Cache

![Screen Shot 2021-05-30 at 8.55.42 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210530205549.png)

![Screen Shot 2021-05-30 at 8.56.48 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210530205653.png)