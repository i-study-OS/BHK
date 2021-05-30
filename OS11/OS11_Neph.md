---
layout: post
title: "[#16] File System-1"
image:
categories: 운영체제
tags: 
  - 운영체제
  - OS
sitemap:
  changefreq: daily
  priority: 1.0
---

## File and File System

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210530165749.png" alt="image-20210530165746939" style="zoom:50%;" />

### 메모리 vs 파일

**메모리**: 주소를 통해서 접근하는 장치

**파일**: 디스크에 일반적으로 저장되며 이름을 통해 접근함, 비휘발성인 하드디스크에 저장됨, 데이터를 저장하는 목적으로만 사용하는 것이 아닌 리눅스와 같은 운영체제에서는 장치들을 관리하기 위해서도 file이라는 이름을 사용해 관리하기도 함

### 연산들

- create / delete
- read / write
- lseek : 파일의 특정 부분부터 읽고싶을 때 현재 접근 위치를 수정해주는 연산
- open / close : open(file의 meta data를 올려놓는 작업) 

### metadata

**metadata(attribute)** : file 자체의 내용이 아닌 file을 관리하기 위한 정보

음악 file을 예시로 들면 음악 data 자체는 file이지만 음악의 이름, 파일의 위치 등등은 파일을 관리하기 위한 내용

### File system

파일에는 파일 자체의 내용과 metadata를 같이 저장함

디렉토리를 제공하여 관리의 용이성 증대

파일을 어떻게 저장할지, 어떻게 관리할지를 담당



## Directory and Logical Disk

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210530171103.png" alt="스크린샷" style="zoom:50%;" />

### Directory

**Directory file**: `하위 디렉토리의 metadata`를 내용으로 가지는 file (모든 metadata가 저장되어 있지는 않다)



### Partition (논리 디스크)

운영체제가 보는 디스크는 Partition이다. 

`file system`을 설치하거나 virtual memory의 `swap area`로도 사용할 수 있다.



## open 연산

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210530171952.png" alt="image-20210530171949680" style="zoom:50%;" />

open(): file의 metadata를 메모리로 올려놓는 연산

논리적인 disk안에 file system이 있으면 그 file system에 metadata와 file의 내용이 저장되어 있음

file의 metadata 중에는 file의 내용을 가리키는 포인터도 같이 저장되어 있음

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210530172047.png" alt="image-20210530172044838" style="zoom:50%;" />

### `/a/b/c`를 open하는 과정

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210530175643.png" alt="image-20210530175640769" style="zoom:50%;" />

`fd`: file descriptor

c의 metadata를 메모리로 가져오고 싶은 상황이므로 이를 위해 directory path를 search한다.

root directory의 metadata는 미리 알고 있으며, root의 metadata를 가져와서 열어보면 root data의 실제 위치를 알 수 있는데 이 내용은 하위 디렉토리의 metadata이다.

이 metadata를 통해 순차적으로 따라 내려가다보면 원하는 파일의 위치정보를 얻을 수 있고

이 위치정보는 PCB에 저장되기 때문에 프로세스가 살아있는 동안 계속해서 사용할 수 있다.



## File Protection

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210530180216.png" alt="image-20210530180213252" style="zoom:50%;" />

접근권한을 가지고 있는 user에 대한 정보와 지원되는 접근 연산에 대한 정보 를 같이 가지고 있어야 한다.

### Access Control Matrix

1. Matrix (Array)

   행렬에 사용자와 파일의 이름을 행과 열에 나열하고 권한에 대해 표시한다.

   사용하지 않는 부분은 놀기 때문에 효율이 좋지 않다.

2. ACL (Linked List): 파일별로 누구에게 어떤 접근 권한이 있는지 표시

3. Capa bility (Linked List): 사용자별로 자신이 접근 권한을 가진 파일 및 해당 권한 표시

모든 접근 파일의 접근 권한을 제어하는 것이 가능하다.

하지만 부가적인 오버헤드가 너무 크기 때문에 일반적인 운영체제에서 잘 사용하지 않는다.

### Grouping

모든 파일에 대해 접근권한을 가지고 있는 것이 아닌

유저를 owner, group, public 세 가지로 구분하여 사용하는데

이 경우 총 9비트만 있으면 파일에 대한 접근 권한을 알 수 있다.

### Password

파일이나 디렉토리마다 password를 걸어 관리하는 방법도 가능하다.

암기 문제, 관리 문제로 인해 잘 사용되지 않는다.



## Mounting

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210530190745.png" alt="image-20210530190742476" style="zoom:50%;" />

하나의 물리적 디스크를 파티션을 통해 여러 디스크로 나누어 놓은 모습

각각의 물리적 디스크에는 파일 시스템을 설치하여 사용할 수 있음.

만약 다른 파티션의 파일시스템에 접근해야하는 상황이 온다면?

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210530190853.png" alt="image-20210530190850175" style="zoom:50%;" />

그것을 제공하기 위한 연산이 Mounting

루트 파일 시스템의 특정 디렉토리의 이름에 또 다른 파티션(B)의 파일시스템을 가져다 붙여주면 

 B의 루트에 접근하는 것과 같은 효과를 누릴 수 있게 됨



## 파일 접근 방법

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210530191033.png" alt="스크린샷-2369407" style="zoom:50%;" />

파일 접근 방법에는 순차 접근과 직접 접근이 있다.

매체에 따라 순차 접근만 지원할수도, 직접 접근만 지원할수도 있다.



## 디스크로의 파일 데이터의 저장 방법

파일을 저장할 때는 동일한 크기의 sector 단위로 나누어서 저장을 한다.

동일한 크기의 저장 단위를 논리적인 block이라 한다.

### Contiguous Allocation (연속할당)

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210530193330.png" alt="image-20210530193326379" style="zoom:50%;" />

연속할당: 하나의 파일이 디스크상에 연속적으로 저장되는 방법

ppt의 list를 예시로 들면 28번째부터 4개의 공간만큼 list의 내용이 할당되어 있는 것이다.

**장점**

- 빠른 I/O의 가능 
  (하드디스크의 접근시간의 대부분은 디스크 헤드가 움직이는데 소요되는 시간이며 크기는 그닥 상관이 없다.) 
  즉, 한 번의 이동으로 많은 양의 데이터를 한번에 가져올 수 있는 것

- 직접 접근의 가능

  길이가 긴 파일의 특정 block을 보고싶을 때 굳이 앞의 block 내용을 다 보지 않아도 뒷부분에 바로 접근이 가능하다.

**단점** 

- 외부 조각의 발생 (비어있는 block들이 일정한 크기를 가지지 않으므로)
- 파일의 크기를 키우는데 제약이 있다.



### Linked Allocation

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210530194724.png" alt="image-20210530194721665" style="zoom:50%;" />

연속적 배치가 아닌 빈 위치에 아무렇게나 배치할 수 있다. 다음 block의 위치를 계속해서 적음으로써 

**장점**

- 외부조각이 발생하지 않는다.

**단점**

- random access가 불가능하다
- 한 부분의 데이터만 없어져도 뒷부분의 데이터를 잃게된다. (의존성 문제)
- pointer를 위한 공간이 block의 일부분을 차지하므로 공간 효율성이 낮아진다.

**Linked Allocation의 변형**

File allocation table (FAT) 파일 시스템 : 포인터를 별도의 위치에 보관하여 의존성 문제와 공간 효율성 문제를 해결하는 방식

### Indexed Allocation

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210530195316.png" alt="image-20210530195314362" style="zoom:50%;" />

어느 위치에 파일의 내용이 저장되어있는지에 대한 정보를 하나의 block에 담아놓는 방식으로 이루어져있다.

**장점**

- 외부조각이 생기지 않음
- 직접 접근 가능

**단점**

- 작은 크기의 파일의 경우 공간을 낭비하게 됨 (하나의 block이 더 필요한 것이므로)
- 너무 큰 파일의 경우 하나의 block에 index들을 전부 담을 수 없어서 곤란



**개선 방안**

- linked scheme: 놀게 되는 index (-1 index) 자리에 다른 파일의 index를 저장해놓는 방법
- multi level index: index가 또 다른 index를 가리키게 하여 큰 파일을 표현할 수 있다. (page table처럼)



##  UNIX 파일시스템의 구조

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210530195754.png" alt="image-20210530195751054" style="zoom:50%;" />



 <img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210530195928.png" alt="image-20210530195926307" style="zoom:50%;" />



- Boot block

  부팅에 필요한 정보를 담고있는 block으로 0번째 block은 Boot block으로 하는 것이 모든 파일시스템의 약속

- Super block

  다른 block들의 위치와 같은 파일 시스템의 전반적인 정보를 담고 있다.

- Inode (index node)

  파일 하나당 inode가 하나씩 할당되며  file의 metadata 중 file의 이름을 제외한 모든 metadata를 가지고 있다.

  (file의 이름은 directory가 가지고 있음)

- Data block



## FAT File System

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210530201331.png" alt="image-20210530201328526" style="zoom:50%;" />

metadata 중 파일의 위치정보만 FAT에 담아두고 나머지는 directory가 가지고 있다.

다음위치를 별도의 FAT이라는 공간에 담음으로써 directory 파일의 공간 낭비를 줄이고 이후 block을 찾는 과정에서 이존 block 의존성을 없앤 방식이다.

linked allocation 방식과 다르게 random access가 가능하고 FAT에 보통 하나의 copy가 아닌 두개의 copy를 저장해놓으므로 data 유실에 대한 걱정도 적다.



## Free Space Management

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210530201946.png" alt="image-20210530201943408" style="zoom:50%;" />

비어있는 공간을 관리하는 방법에는 여러가지가 있다.

### Bit map or Bit vector

비트맵은 비어있는 block을 표시해줌 

부가적인 공간이 필요하지만 그렇게 큰 공간의 낭비는 아님

가능하면 연속적인 공간에 할당해주는 것이 좋음

### Linked List 

모든 free block을 연결해놓는 방식이다. 

특정 크기의 연속적인 가용공간을 찾는게 쉽지 않다는 단점이 있지만 공간의 낭비가 적다

### Grouping

Linked List의 변형 버전이다.

첫번째 free block이 n개의 pointer를 가지며 그 pointer를 따라가면 실제 free data block들이 있다.

마지막 pointer는 또 다시 n개의 pointer를 가지는 block이다.

### Counting

빈 블록의 위치를 가리킴과 동시에 몇개의 빈 블록이 연결되어 있는지 알려주는 방법이다.



## 디렉토리의 구현

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210530202439.png" alt="image-20210530202436695" style="zoom:50%;" />

### Linear list

구현이 간단하지만 시간복잡도가 크다

### Hash Table

탐색 시간이 없어지는 장점이 있다



<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210530202854.png" alt="image-20210530202851077" style="zoom:50%;" />

파일의 이름이 일정 길이보다 길면 일정 길이 이상은 metadata에 보관하며, 이를 포인터를 통해 관리한다. 



## VFS and NFS

### Virtual File System

서로 다양한 file system에 대해 동일한 시스템 콜 API를 통해 접근할 수 있도록 해주는 OS의 layer

### Network File System

분산 시스템에서는 네트워크를 통해 파일이 공유될 수 있음

NFS는 분산 환경에서의 대표적인 파일 공유 방법

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210530203255.png" alt="image-20210530203252561" style="zoom:50%;" />

## Page Cache와 Buffer Cache

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210530203841.png" alt="image-20210530203837137" style="zoom:50%;" />

page frame이 backing store에 비해 빠르므로 cache

위와 같은 것을 파일 시스템 관점에서는 buffer cache라 부름



<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210530204310.png" alt="image-20210530204305715" style="zoom:50%;" />