# Disk Management and Scheduling

![Screen Shot 2021-06-06 at 7.18.27 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210606191831.png)

## Disk Management

![Screen Shot 2021-06-06 at 7.21.26 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210606192130.png)

- ECC
  - 어느정도 규모의 ECC를 두느냐에 따라서 에러를 수정할 수도 있고 검출만하고 수정을 못할 수도 있다.
- Booting
  - small bootstrap loader 실행
    - 0번 sector의 내용을 물리적 메모리로 올리고 실행합니다.
    - 0 sector가 bootblock을 실행하게되면 file system에서 운영체제 커널의 위치를 찾게 해서 그것을 메모리로 올려서 실행하도록 지시합니다.
    - 운영체제가 메모리로 올라가게 되어서 booting이 이뤄집니다.



 ## Disk Scheduling

![Screen Shot 2021-06-06 at 7.34.06 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210606193411.png)

* Transfer time은 비교적 적은 시간이 소요된다. 디스크작업 중 가장 오래 걸리는 것은 seek time이라고 보면된다. 한 번의 seek로 많은양의 데이터를 전송하는 것이 이득



## Disk Scheduling Algorithm

![Screen Shot 2021-06-06 at 7.40.58 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210606194103.png)

### FCFS

![Screen Shot 2021-06-06 at 7.42.27 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210606194232.png)



### SSTF

![Screen Shot 2021-06-06 at 7.44.07 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210606194411.png)

### SCAN

![Screen Shot 2021-06-06 at 7.45.39 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210606194544.png)

Elevator scheduling과 유사

문제점: 제일 안쪽이나 제일 바깥쪽 위치보다 가운데 위치가 기다리는 평균시간이 더 짧다. 헤드가 가운데 위치를 지나가는 주기는 양 끝의 지나가는 주기의 절반에 불과하기 때문에 양끝에 있는 요청은 비교적 더 오랜시간을 기다려야 하는 문제가 있습니다. 

### C-SCAN

![Screen Shot 2021-06-06 at 7.48.11 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210606194817.png)

이동거리는 조금 길어질 수 있으나 Queue에 들어온 요청들을 처리하는 시간이 조금 더 균일 해 집니다.

SCAN보다 헤드의 이동거리는 조금 길어지지만 탐색시간의 편차를 줄일 수 있다

### N-SCAN / LOOK / C-LOOK

![Screen Shot 2021-06-06 at 7.50.32 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210606195036.png)

#### N-SCAN

- 지나가기 전에 들어온 요청은 모두 처리
- 지나가는 도중에 들어오는 요청은 기억해 놓았다가 뒤돌아 오면서 처리



#### LOOK

헤드가 한쪽 방향으로 이동하다가 해당 방향에 더 이상 대기 중인 요청이 없으면 헤드의 이동방향을 즉시 반대로 바꾸는 스케줄링 방식

#### C-Look

![Screen Shot 2021-06-06 at 7.52.48 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210606195253.png)

Look과 C-SCAN을 합친 알고리즘

- 전방에 요청이 없을 때 방향을 바꾼다.[LOOK 알고리즘]
- 한쪽 방향으로 이동하 ㄹ때만 요청을 처리한다[C-SCAN과 유사]



![Screen Shot 2021-06-06 at 8.00.16 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210606200023.png)

## Swap-Space Management

![Screen Shot 2021-06-06 at 8.01.29 PM](/Users/hwang-in-u/Library/Application Support/typora-user-images/Screen Shot 2021-06-06 at 8.01.29 PM.png)

## RAID

![Screen Shot 2021-06-06 at 8.06.14 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210606200618.png)

*parity: 중복저장도를 굉장히 낮게 해서 오류가 생겼는지 알아내고 복구할 정도로만 데이터를 저장하는 기법