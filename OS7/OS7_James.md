# DeadLock

---

![Screen Shot 2021-05-01 at 4.44.32 PM](https://tva1.sinaimg.cn/large/008i3skNgy1gq2z1gw3ocj315k0u0npe.jpg)

P0과 P1의 교착상태

- P1은 자원A를 획득한 뒤 자원B를 획들하려 하는 상황에 CPU를 빼앗김
- P0은 자원 B를 획득한 상황에서 자원A를 획득하려 하지만 이미 A는 점유된 상태
- 결국 각 프로그램은 CPU를 획득해도 필요한 자원을 획득하지 못하기에 교착상태에 빠지게 된다.



![Screen Shot 2021-05-01 at 4.48.08 PM](https://tva1.sinaimg.cn/large/008i3skNgy1gq2z5243kxj31610u0npe.jpg)

- 이론가들이 이 4가지 조건이 DeadLock의 발생 조건이라고 명시하였다.



*P: Process

*R: Resources

R → P: 해당 자원이 해당 프로세스에 속해있다.

P → R: 해당 프로세스가 가리키는 자원을 요청을 한것. 아직 획득하지는 못했다.

"●" : 자원의 수(instance)

![Screen Shot 2021-05-01 at 4.52.03 PM](https://tva1.sinaimg.cn/large/008i3skNgy1gq2z95r51xj31650u0x6p.jpg)

![Screen Shot 2021-05-01 at 4.56.11 PM](https://tva1.sinaimg.cn/large/008i3skNgy1gq2zdqwd1bj31600u0qv6.jpg)

그래프를 볼 때 싸이클이 존재하면 이는 교착상태를 의미한다. 다만 자원의 인스턴스가 여러개인 상황에는 교착상태일 수도 있고 아닐 수도 있다.

첫 번째 상황: 비록 R2에 자원이 두 개가 있지만 더이상 진행하기 어려운 상황이다(두 개의 싸이클 존재)

두 번째 상황: 싸이클이 존재하긴 하지만 자원이 두 개씩 존재하고 교착상태가 아니다. P4가 자원을 가지고 있다 쓰고 반납하면 교착상태가 종료되기 때문이다.



![Screen Shot 2021-05-01 at 5.01.44 PM](https://tva1.sinaimg.cn/large/008i3skNgy1gq2zja3i4yj315t0u0u0y.jpg)

대부분의 운영체제가 4번째 방법을 채택한다. 그럼 이런 상황에서 프로그램이 교착 상태에 빠지면 사용자가 할 수 있는 것은 프로세스를 죽이는 것이다. 교착상태는 빈번히 발생하는 것이 아니기에 이를 처리하기 위해 오버헤드를 주기 보다는 생기든 말든 내비두는 것이다.



#### 1. DeadLock Prevention

교착상태의 4가지 조건을 원천적으로 차단하여 교착상태를 방지하는 방법

![Screen Shot 2021-05-01 at 5.05.40 PM](https://tva1.sinaimg.cn/large/008i3skNgy1gq2znc18tsj315s0u0u0z.jpg)

- Mutual Exclusion
  - 공유해야하면 안되는 자원의 경우 반드시 매 순간 하나의 프로세스만이 해당 자원을 사용할 수 있어야 한다.
- Hold and Wait
  - 방법 1: 프로세스 시작 시 모든 필요한 자원을 할당받게 하는 방법
    - 문제점: 매 순간 프로세스가 필요할 자원이 다 다를텐데 필요한 모든 자원을 미리 할당 받게 되면 이는 자원의 비효율성을 야기한다.
  - 방법 2: 자원이 필요할 경우 보유 자원을 모두 놓고 다시 요청
- No Preemption
  - 프로세스가 어떤 자원을 기다려야 하는 경우 이미 보유한 자원이 선점됨
  - 자원을 빼앗을 수 있게 설정하여 교착상태를 방지함
  - 모든 자원을 빼앗을 수 있는 것이 아니라 memory, CPU등 자원의 현재 상태를 save하고 restore할 수 있는 자원들을 빼앗도록 하는 것이다.
- Circular wait
  - 이를 막기 위해서 모든 자원 유형에 할당 순서를 정하여 정해진 순서대로만 자원을 할당 해 준다.

문제점:

→ 자원의 이용률 저하, Utilization 저하. 생길일이 적은 교착상태를 위해 많은 제약조건을 달아 놓는 것이기 때문에 비효율적이다.



#### 2. DeadLock Avoidance

프로세스가 시작될 때 프로세스의 평생에 쓸 자원의 최대량을 알고 있게 해서 교착발생의 가능성이 보이면 자원의 여분이 있더라도 해당 프로세스에게 자원을 할당하지 않는다. 교착상태 미연 방지

![Screen Shot 2021-05-01 at 5.37.58 PM](https://tva1.sinaimg.cn/large/008i3skNgy1gq30kxezgbj31610u07wj.jpg)

![Screen Shot 2021-05-01 at 5.40.38 PM](https://tva1.sinaimg.cn/large/008i3skNgy1gq30nqhjwjj315x0u0npe.jpg)

**Resource Allocation Graph Algorithm**

- 자원이 하나 밖에 없는 환경에서의 avoidance 알고리즘

"⇢" : 프로세스가 평생에 적어도 한번은 가리키는 자원을 사용할 일이 있다. 지금 요청을 한 것이 아니지만.

요청을 하게되면 점선 화살표에서 실선 화살표로 바뀌게 된다. 

![Screen Shot 2021-05-01 at 5.42.37 PM](https://tva1.sinaimg.cn/large/008i3skNgy1gq30prn8ndj315s0u0x6q.jpg)

위 사진에서는 R2같은 경우 P2에게 자원할당이 가능하나 자원을 내어줄 시 최악의 상황에는 (점선이 실선으로 바뀌는 상황)에는 교착상태에 빠질 수 있기 때문에 P2에게 자원을 내어주지 않아서 교착을 사전에 방지하는 방법



**Banker's Algorithm**

![Screen Shot 2021-05-01 at 5.49.06 PM](https://tva1.sinaimg.cn/large/008i3skNgy1gq30wk0kyej31600u0e82.jpg)

![Screen Shot 2021-05-01 at 5.50.09 PM](https://tva1.sinaimg.cn/large/008i3skNgy1gq30xl5msdj315t0u0kjm.jpg)

![Screen Shot 2021-05-01 at 5.56.24 PM](https://tva1.sinaimg.cn/large/008i3skNgy1gq31446ynqj315o0u07wi.jpg)

Can request for (0,2,0) by P0 be granted?

No! 물론 여유 자원을 확인 해 보면 B를 3개 가지고 있기 때문에 줄 수는 있지만 P0의 최대 자원 필요량은 현재 할당 가능한 자원의 숫자를 훨씬 넘기기 때문에 만약 P0이 최대자원을 요청하게 되면 지금 가용자원만으로는 처리가 되지 않기 때문에 요청을 받아드리지 않고 기다리게한다. 언젠가 다른 프로세스의 처리가 끝나고 할당되어있는 자원을 뱉어내면 P0의 요청을 받아드리게 된다.

대단히 비효율적인 상황: 자원이 남아도는데 그걸 쓰지 못하게 억제하는 것이니기 때문이다.

가용 자원 전체가 0이 된다 해서 무조건 Deadlock상태가 되는 것은 아니지만 해당 알고리즘에 따르면 이런 상황은 unsafe한 상황인 것이다. 안전한 상황에 최대한 초점을 맞추기 때문에 이런 unsafe한 상황을 피하려고 자원이 남아있더라도 특정 프로세스의 최대자원이 가용자원보다 요구하는 자원이 더 많을 경우 아예 할당을 하지 않는 이유도 safe한 상태를 유지하려 함에 초점을 맞추기 때문에 그런 것이다.



#### DeadLock Detection and recovery

![Screen Shot 2021-05-01 at 11.58.03 PM](https://tva1.sinaimg.cn/large/008i3skNgy1gq3bkfqhzxj315w0u0b2d.jpg)

![Screen Shot 2021-05-01 at 11.59.07 PM](https://tva1.sinaimg.cn/large/008i3skNgy1gq3blkcz0cj315x0u0kjn.jpg)

싸이클을 찾는 오버헤드는 얼마나 되나?

- O(n^2)

![Screen Shot 2021-05-02 at 12.03.12 AM](https://tva1.sinaimg.cn/large/008i3skNgy1gq3bptb7dcj315h0u04qs.jpg)

낙관적인 접근:

각 프로세스는 자기에게 할당된 것을 무조건 반납할 것이라 가정. 물론 추가로 요청할 수도 있지만...



만약 P2가 자원C 1개를 요청하는 경우:

![Screen Shot 2021-05-02 at 12.25.05 AM](https://tva1.sinaimg.cn/large/008i3skNgy1gq3cchre77j328e0sc4qp.jpg)

P0이 끝나고 B1개가 반납되었지만 어느 누구의 요청도 받아드릴 수 없는 상황이고 이는 Deadlock 상태를 뜻한다.

이렇게 교착상태에 빠지는 경우 Recovery가 실행되어야 한다.

![Screen Shot 2021-05-02 at 12.26.13 AM](https://tva1.sinaimg.cn/large/008i3skNgy1gq3cdra6awj315q0u0u0z.jpg)

**Process termination**

- 연류된 프로세스를 모두 죽이는 것

- 연류된 프로세스를 하나씩 죽여보는 것

**Process Preemption**

연류된 프로세스로부터 자원을 빼앗는 것

- 희생양을 하나 찾아 뺏어서 해결
  - 문제점1: 자원을 뺏는 패턴이 지속될 경우 문제가 될 수 있다. 그래서 조금씩 패턴을 바꿔야 한다.
  - 문제점2: starvation문제. 한 프로세스만 지속적으로 자원을 뺏길 수 있다.



#### DeadLock Ignorance

![Screen Shot 2021-05-02 at 12.29.50 AM](https://tva1.sinaimg.cn/large/008i3skNgy1gq3chh2jjlj315m0u0kjn.jpg)

Deadlock을 방지하는 것도 자원을 비효율하게 쓰는 것이고 Deadlock 찾아내는 것도 시스템에 오버헤드를 일으킨다.

따라서 그냥 아무런 조치를 취하지 않고 사용자가 대응하도록 하는 방법