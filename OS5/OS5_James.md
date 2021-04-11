# CPU Scheduling

---

<img width="1977" alt="Screen Shot 2021-04-04 at 3 29 04 AM" src="https://user-images.githubusercontent.com/69072471/113498947-794bdc80-954c-11eb-8853-400977158a2f.png">

cpu 스케줄링

CPU만을 연속적으로 사용하는 단계를 `CPU burst` 라고 부른다.

I/O를 연속적으로 사용하는 단계를 `I/O burst`  라고 부른다.

주로 사람이 하는 작업에서 I/O가 자주 끼어들고 이는 CPU burst와 I/O burst 자주 골고루 사용된다.

특정한 단계의 빈도수를 결정하는 것은 프로그램의 종류에 달려있다.

[사용자의 작업 vs 컴퓨터 연산]

<img width="1973" alt="Screen Shot 2021-04-04 at 3 35 17 AM" src="https://user-images.githubusercontent.com/69072471/113498948-79e47300-954c-11eb-8b13-d0370df65f20.png">

<img width="1976" alt="Screen Shot 2021-04-04 at 3 41 05 AM" src="https://user-images.githubusercontent.com/69072471/113498950-7a7d0980-954c-11eb-9ee0-e445de91a7fd.png">



CPU를 짧게 쓰고 I/O 작업이 끼어는 작업을 `I/O bound job` 이라 부르고

CPU만을 오랫동안 쓰는 프로그램 작업을 `CPU bound job`이라 부른다.



#### CPU 스케줄링의 필요 이유

컴퓨터 시스템 안에 있는 job들이 homogeneous 하지 않고 I/O bound job과 CPU bound job이 섞여있기에 스케줄링이 필요하다. 

`I/O bound job` 사용자가 사용하는 작업이기 때문에 매우 interactive한 job이다. CPU를 CPU bound job이 잡고 오랜시간 동안 CPU를 놓아주지 않으면 사용자 입장에서는 응답시간이 길어지고 이는 답답한 상황까지 이를 수 있다.

이를 방지하기 위해 가능하면 사람하고 interaction이 많은  I/O bound job에게 CPU를 우선적으로 주는 것이 CPU 스케줄링의 중요한 역할 중 하나이다.



<img width="992" alt="Screen Shot 2021-04-11 at 2 15 14 PM" src="https://user-images.githubusercontent.com/69072471/114293368-7f076c00-9ad0-11eb-8899-e888ce0a3edf.png">

`CPU Scheduler`는 운영체제 안에서 CPU scheduling 하는 커널 코드를 지칭한다.

`Dispatcher`는 CPU를 넘겨주는 역할을 하는 운영체제의 커널 코드를 지칭한다.  



스케줄링이 필요한 경우

1. 프로세스가 CPU를 잡고 있다 I/O작업이 필요한 경우 자진해서 CPU를 자진해서 내 놓는 경우 
2. 프로세스가 CPU를 사용해야 하나 할당 시간 만료일 때 CPU를 강제로 빼앗고 다른 프로세스에게 CPU를 넘겨줄 때
3. 프로세스B가 CPU를 갖고 있는데 I/O 작업이 끝난 뒤 디바이스 컨트롤러가 interrupt를 걸어서 프로세스A에게 CPU를 얻을 권한을 주고 프로세스A는 다음 인스트럭션을 실행할 수 있는 ready상태가 된다. 만약 프로세스B의 우선순위가 프로세스A보다 떨어질 경우 CPU스케줄러는 프로세스B로부터 CPU를 강제로 빼앗아서 프로세스A에게 CPU를 넘겨주게 된다.
4. 어떤 프로세스가 종료될 경우 다른 프로세스에게 넘겨 줄 때



스케줄링은 크게 2가지로 나뉘어진다.

- 비선점형 nonpreemptive(강제로 빼앗지 않는)
- 선점형 (강제로 빼앗는)


알고리즘 성능의 척도는 어떻게 되는가?

크게 두 가지로 분류 될 수 있다.

- 시스템 입장에서의 성능 척도

- 프로세스 입장에서 성능 척도(얼마나 빠르게 처리할 수 있는가)

<img width="991" alt="Screen Shot 2021-04-11 at 2 20 38 PM" src="https://user-images.githubusercontent.com/69072471/114293459-179dec00-9ad1-11eb-9596-4cdb1ec86d51.png">

CPU입장

- 이용률
  - CPU가 놀지 않은 비율을 뜻한다.
  - CPU를 바쁘게 일을 시켜라. 시스템입장에서 CPU를 잘 쓰는 방법
  - *사장이 주문장을 고용*
- 처리량
  - 주어진 시간동안 얼마나 많은 작업을 완료했는가
  - *몇 명이나 손님이 밥을 먹었는가*



프로세스입장

- 소요시간, 반환시간[Turnaround time]
  - CPU burst가 끝나고 나가는 대까지 걸리는 시간
  - *손님입장에서 고객이 주문을 하고 나서 다 먹고 나가는 데 까지 걸리는 시간*
- 대기시간
  - ready queue에 기다리는 시간
  - *밥 먹는 시간 외 기다리는 시간*
- 응답시간
  - 최초의 CPU를 얻기까지 기다리는 시간
  - *첫 번째 음식이 나오기 전까지 기다리는 시간*



#### 스케줄링의 알고리즘은 여러가지가 있다.

<img width="995" alt="Screen Shot 2021-04-11 at 2 18 05 PM" src="https://user-images.githubusercontent.com/69072471/114293404-bd9d2680-9ad0-11eb-9dc0-1d38b3ac990c.png">

1. 

<img width="985" alt="Screen Shot 2021-04-11 at 2 30 48 PM" src="https://user-images.githubusercontent.com/69072471/114293602-83cd1f80-9ad2-11eb-82f6-0d5763bfbbba.png">

- 비선점형 스케줄링[Nonpreemptive]
- 먼저온 프로세스 먼저 처리

**Convoy effect**: processing having knock on effects slowing the whole system down.

*짜장면을 먹으러 왔는데 앞에 손님이 시킨 샥스핀 요리 때문에 5시간 뒤에 짜장면을 먹는다면????*



2. 

<img width="991" alt="Screen Shot 2021-04-11 at 2 37 20 PM" src="https://user-images.githubusercontent.com/69072471/114293734-6cdafd00-9ad3-11eb-9da0-f3766a19f9c4.png">

- 전체적으로 짧아지는 queue →  최소 평균 대기시간

SRTF는 짧은 프로세스에게 CPU를 우선적으로 넘겨주지만 후에 들어온 프로세스 중 현재 사용되고 있는 프로세스를 처리하는 시간보다 더 짧은 시간을 필요로 하는 프로세스에게 강제로 CPU를 넘겨주게 하는 알고리즘이다.

<img width="983" alt="Screen Shot 2021-04-11 at 2 44 00 PM" src="https://user-images.githubusercontent.com/69072471/114293825-5c775200-9ad4-11eb-96d5-58c4cb96b714.png">

**Non-preemptive SJF**

- 프로세스 처리가 끝난 뒤 스케줄링이 이뤄진다.

- P1이 먼저 실행 된 후 들어와 있는 나머지 프로세스를 확인 한 뒤 가장 짧게 걸리는 프로세스 순서대로 처리한다.



<img width="993" alt="Screen Shot 2021-04-11 at 2 44 55 PM" src="https://user-images.githubusercontent.com/69072471/114293837-7dd83e00-9ad4-11eb-807b-581760556c38.png">

3. **Preemptive SJF or SRTF**

- 새로운 프로세스가 도착 할 때 스케줄링이 이뤄진다.

- P1이 먼저 실행 되었음에도 더 짧은 프로세스가 들어오니 CPU를 빼앗긴다.
- P2작업이 끝난 뒤에 P1의 남은시간과 P3의 소요시간을 비교한 뒤 P3의 소요시간이 더 짧기 때문에 P3를 실행한다.
- 이런 식으로 남아있는 소요시간을 비교해서 작업을 순서대로 진행한다.
- 위 알고리즘보다 더 짧은 평균대기시간을 갖게 하는 알고리즘은 없다. [THIS is optimal algorithm]



#### SJF와 SRTF의 문제점

- Starvation
  - 극단적으로 짧은 프로세스를 선호하기에 긴 시간이 소요되는 프로세스는 영영 사용 못될 수 있다.
  - *변비 걸린 사람은 영영 화장실을 사용못할지도...*
- CPU 사용시간을 정확히 알 수가 없어진다.
  - 추정만이 가능하다.
  - 과거의 CPU burst time을 이용해서 추정은 가능하다. (exponential averaging)

<img width="592" alt="Screen Shot 2021-04-11 at 2 56 55 PM" src="https://user-images.githubusercontent.com/69072471/114294029-2935c280-9ad6-11eb-8b36-a1512713a9c4.png">

<img width="987" alt="Screen Shot 2021-04-11 at 3 01 20 PM" src="https://user-images.githubusercontent.com/69072471/114294083-c7c22380-9ad6-11eb-9a2e-92e12c78bed5.png">

과거를 토대로 미래를 예측하는데 가장 최근의 데이터를 많이 반영하면서 예전 데이터를 비교적 더 적게 반영함으로써 미래에 걸릴 시간을 추정하는 방법.



4. 

<img width="990" alt="Screen Shot 2021-04-11 at 3 02 00 PM" src="https://user-images.githubusercontent.com/69072471/114294096-df99a780-9ad6-11eb-9a64-9866436ab897.png">

우선순위가 제일 높은 프로세스에게 CPU를 주는 알고리즘

리눅스를 비롯한 일반적인 시스템에서는 우선순위가 정수로 주어지고 숫자가 작을수록 우선순위가 높아진다.

- Preemptive
  - 더 높은 우선순위 프로세스가 도착하면 스케줄링을 통해 CPU를 강제로 넘겨줌
- Non-Preemptive
  - 더 높은 우선순위 프로세스가 도착하더라도 먼저 도착한 프로세스를 처리 한 이후 스케줄링 실행



**문제점**

- Starvation: 특정 프로세스가 차별받게 된다.

**해결**

- Aging: 우선순위가 낮은 프로세스라도 시간이 지날 수록 우선순위를 높혀주는 솔류선기법이다.



5. 

<img width="987" alt="Screen Shot 2021-04-11 at 5 02 52 PM" src="https://user-images.githubusercontent.com/69072471/114296719-c2210980-9ae7-11eb-9c2c-6f58b2a52849.png">

- CPU를 줄 때 할당시간(time quantum)을 먼저 부여한 뒤 CPU를 할당한다.
- 할당시간이 초과되면 프로세스로부터 CPU를 빼앗는다.
- 소요시간이 짧게 요구되는 프로세스일 경우 빠르게 ready queue를 빠져나갈 수 있게 된다.
- 소요시간이 길게 요구되는 프로세스일 경우 할당 시간이 초과되면 CPU를 빼앗기고 대기열로 돌아가게되고 후에 순서가 되면 다시 CPU를 받게 된다.

Time quantum이 클 경우: FirstComeFirstServed 방식

Time quantum이 작을 경우: CPU의 할당과 회수의 횟수가 잦아지기에 context switch또한 잦아지고 이는 오버헤드가 커지는 문제 발생

적절한 TIme quantum을 부여하는게 타당하다.

<img width="981" alt="Screen Shot 2021-04-11 at 5 05 50 PM" src="https://user-images.githubusercontent.com/69072471/114296794-2c39ae80-9ae8-11eb-8af3-59fe85fd4786.png">

단점? 일수도 있는 특징

모든 프로세스가 마지막까지 CPU의 서비스를 많이 받기에 waiting time이 길어져서 안좋을 수 있지만 응답시간은 빨라질 수 있다.





6. 

<img width="1960" alt="Screen Shot 2021-04-11 at 5 22 02 PM" src="https://user-images.githubusercontent.com/69072471/114297234-6f951c80-9aea-11eb-9867-a958c69155cb.png">

기존에는 프로세스가 한 줄로 줄을 섰지만 multiLevel에서는 여러 줄로 서게 된다.



<img width="1973" alt="Screen Shot 2021-04-11 at 5 23 34 PM" src="https://user-images.githubusercontent.com/69072471/114297278-a79c5f80-9aea-11eb-8c33-f03cabf254a2.png">

각 줄마다 우선순위가 존재하는 우선순위에 서있는 프로세스부터 처리하는 CPU스케줄링 방법



<img width="1979" alt="Screen Shot 2021-04-11 at 5 27 55 PM" src="https://user-images.githubusercontent.com/69072471/114297381-41640c80-9aeb-11eb-99b0-aced2f5e3364.png">

- 위 예시에서는 줄이 두 개가 있다(foreground & background)
- 줄의 특성에 맞는 독립적인 스케줄링 알고리즘을 갖는다.
- 각 큐에 대한 스케줄링이 필요하다.
  - Fixed priority scheduling
  - time slice

노비(하위 우선순위 queue에 있는 프로세스)도 먹고 살 수 있는 환경이 time slice에 의해 가능해지지만 신분 격차(다른 queue로 이동)를 뛰어넘을 수는 없다.



7. 

<img width="1983" alt="Screen Shot 2021-04-11 at 5 26 41 PM" src="https://user-images.githubusercontent.com/69072471/114297353-15488b80-9aeb-11eb-8cf9-a219fc5f548d.png">

- 여러 줄이 있지만 우선순위가 상황에 따라 변할 수 있다.



<img width="1972" alt="Screen Shot 2021-04-11 at 5 31 45 PM" src="https://user-images.githubusercontent.com/69072471/114297470-cb13da00-9aeb-11eb-8740-c1987bd6c322.png">

<img width="1973" alt="Screen Shot 2021-04-11 at 5 37 37 PM" src="https://user-images.githubusercontent.com/69072471/114297633-9c4a3380-9aec-11eb-8503-b4ceb65cea65.png">



지금까지는 CPU가 하나일 경우 어떻게 CPU 스케줄링이 되는지 알아보았다. 



CPU가 여러개인 **멀티프로세서**의 스케줄링은 어떻게 되는가?

<img width="1974" alt="Screen Shot 2021-04-11 at 5 38 58 PM" src="https://user-images.githubusercontent.com/69072471/114297651-cd2a6880-9aec-11eb-96b5-0515f935b86e.png">



정해진 시간 안에 끝내야 하거나 제약조건이 존재하는 프로세스인 경우...

<img width="1982" alt="Screen Shot 2021-04-11 at 5 44 14 PM" src="https://user-images.githubusercontent.com/69072471/114297759-88530180-9aed-11eb-9336-98a53a4abf5d.png">



(Recap)

Thread: Thread란 프로세스 하나 안에 CPU수행단위가 여러 개 있는 것.

User level Thread: 사용자 프로세스가 직접 Thread를 관리(운영체제는 thread의 존재를 모른다.)

Kernel level thread: 운영체제가 Thread의 존재 여부를 알고 있는 것

<img width="1964" alt="Screen Shot 2021-04-11 at 5 46 43 PM" src="https://user-images.githubusercontent.com/69072471/114297826-e2ec5d80-9aed-11eb-8781-75595802d850.png">



어떤 알고리즘이 좋은지 평가하는 방법은...

<img width="1961" alt="Screen Shot 2021-04-11 at 5 50 07 PM" src="https://user-images.githubusercontent.com/69072471/114297903-5b531e80-9aee-11eb-8b0e-94268996cb1f.png">

*trace: 시뮬레이션 프로그램에 들어갈 input 데이터