# Process Synchronization - 2

---

<img width="499" alt="Screen Shot 2021-04-25 at 2 58 48 PM" src="https://user-images.githubusercontent.com/69072471/115982511-bf89de00-a5d6-11eb-8f97-4c9078671852.png">



1. Bounded-Buffer Problem

<img width="548" alt="Screen Shot 2021-04-25 at 3 00 01 PM" src="https://user-images.githubusercontent.com/69072471/115982540-eba55f00-a5d6-11eb-9d9f-c1292cde1d67.png">

각 버퍼에 접근해야 하는 생산자 process나 소비자 process는 한 번에 한 개씩만 접근할 수 있어야 한다. 그렇기 때문에 접근하는 생산자나 소비자는 먼저 공유데이터에 lock을 걸고 데이터 buffer를 조작한다.



**생산자 프로세스 입장에서의 문제:**

유한한 버퍼가 꽉찬 경우 → 소비자 프로세스가 버퍼에서 데이터를 꺼내기 전까지 작동 중지



***소비자 프로세스 입장에서의 문제:**

버퍼가 빈 경우 → 자원이 없기 때문에 생산자 프로세스가 내용을 만들어 넣어줄 때 까지 기다려야 한다.



**이러한 버퍼 문제에서 Semaphore가 해야 할 일:**

1. 버퍼에 lock을 걸어서 한 번에 한 프로세스만 공유데이터 버퍼에 접근할 수 있게 해야 한다. [mutual exclusion, mutex]
2. 버퍼가 가득 차거나 빈 경우 가용자원의 갯수를 세는 counting 세마포어의 용도로 세마포어 변수가 필요하다.

<img width="549" alt="Screen Shot 2021-04-25 at 3 16 22 PM" src="https://user-images.githubusercontent.com/69072471/115982902-345e1780-a5d9-11eb-9528-ec41b125142d.png">



2. Readers-Writers Problem

<img width="547" alt="Screen Shot 2021-04-25 at 3 19 15 PM" src="https://user-images.githubusercontent.com/69072471/115982974-9ae33580-a5d9-11eb-9b97-5a4632bac8b1.png">

<img width="548" alt="Screen Shot 2021-04-25 at 3 23 52 PM" src="https://user-images.githubusercontent.com/69072471/115983108-3f657780-a5da-11eb-949d-3982e3b7ceb6.png">

starvation 발생 가능: writer가 평생 기다려야 할지도...



3. Dinig-Philosophers Problem

<img width="484" alt="Screen Shot 2021-04-25 at 3 46 20 PM" src="https://user-images.githubusercontent.com/69072471/115983644-64a7b500-a5dd-11eb-8277-e0b2d12d3220.png">

  <img width="485" alt="Screen Shot 2021-04-25 at 3 50 16 PM" src="https://user-images.githubusercontent.com/69072471/115983729-ef88af80-a5dd-11eb-998c-f708e3b2f038.png">

여러가지 해결방안이 제시되어 있고 이 중 두 번째 해결 방안에 대한 코드를 아래 사진을 통해 볼 수 있다.

<img width="487" alt="Screen Shot 2021-04-25 at 3 55 11 PM" src="https://user-images.githubusercontent.com/69072471/115983832-9f5e1d00-a5de-11eb-9397-ed282d1d89d9.png">



## Monitor

<img width="488" alt="Screen Shot 2021-04-25 at 4 07 31 PM" src="https://user-images.githubusercontent.com/69072471/115984113-5909bd80-a5e0-11eb-9bfa-60d96e888b60.png">

<img width="489" alt="Screen Shot 2021-04-25 at 4 16 41 PM" src="https://user-images.githubusercontent.com/69072471/115984343-a0447e00-a5e1-11eb-9022-afa2cd056423.png">

<img width="489" alt="Screen Shot 2021-04-25 at 4 17 59 PM" src="https://user-images.githubusercontent.com/69072471/115984371-cec25900-a5e1-11eb-9613-ffbbdcfb3ecf.png">

장점 / 차이점: 모니터는 기본적으로 여러 프로세스의 동시접근을 허락하지 않기 때문에 별도의 lock을 걸 필요가 없어진다. 프로그래머의 부담이 적어진다.

만약 특정 프로세스가 모니터에 들어와 있다가 다른 프로세스에게 CPU를 뺏긴다 하더라도 전자는 모니터 안에서 active하게 남아있고 후자는 모니터로 접근이 불가능 해 진다.

다른 프로세스가 모니터에 접근할 수 있는 경우:

- 모니터안에 더이상 active한 프로세스가 없을 때
- 프로세스가 내부에서 충족이 되지 않아서 inactive한 상태가 되거나 모니터를 빠져나오는 경우

<img width="489" alt="Screen Shot 2021-04-25 at 4 19 24 PM" src="https://user-images.githubusercontent.com/69072471/115984439-0204e800-a5e2-11eb-9d6d-b33d03038943.png">

conditiona variable은 값을 가지는 변수가 아니라 특정 프로세스를 잠들게 하고 queue에 줄 세우기 위한 변수이다.

- Wait() 연산
  - 특정 프로세스를 잠들게 하는 메서드
- signal() 연산
  - 잠들어 있는 프로세스를 깨워주는 메서드
  - 잠들어 있는 프로세스가 없다면 아무런 일을 하지 않는다.



모니터: Bounded-Buffer Problem

<img width="487" alt="Screen Shot 2021-04-25 at 4 21 33 PM" src="https://user-images.githubusercontent.com/69072471/115984488-4e502800-a5e2-11eb-95d5-20835d84d4a2.png">

<img width="1105" alt="Screen Shot 2021-04-25 at 4 45 55 PM" src="https://user-images.githubusercontent.com/69072471/115985215-bd7b4b80-a5e5-11eb-92f4-149dbfcb84e4.png">

