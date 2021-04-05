# os 4강

Date: Apr 4, 2021

# 프로세스 생성

![Untitled](https://user-images.githubusercontent.com/35272802/113575231-881fb580-9658-11eb-94f1-9f609be301b7.png)

- 부모가 자식을 생성함
- 부모가 자식을 고대로 복사 생성
- 운영체제 요청해서 프로세스를 생성 `fork()` 사용

## fork() 시스템 콜

```c
#include <stdio.h>
#include <unistd.h>

int main(void)
{
	int pid;
	printf("before fork\n");
	pid = fork();
	printf("after fork\n");

	if (pid == 0)
		printf("child!!\n");
	else if (pid > 0)
		printf("parent!!\n");
}
```

결과

```c
before fork
after fork
parent!!
after fork
child!!  
```

- 부모프로세스가 다 실행되고 `fork` 된 부분 부터 자식 프로세스가 실행됨.

## exec() 시스템 콜

```c
#include <stdio.h>
#include <unistd.h>

int main(void)
{
	int pid;
	//printf("before fork\n");
	pid = fork();
	//printf("after fork\n");

	if (pid == 0)
	{
		printf("child!!\n");
		execlp("/bin/pwd", "bin/pwd", (char*)NULL);
		printf("no print!\n");  // 출력 안됨.
	}
	else if (pid > 0)
		printf("parent!!\n");
}
```

결과

```c
parent!!
child!!
/Users/seunkim/Desktop
```

## wait() 시스템 콜

- block 상태가 됨.
- 자식 프로세스가 다 실행되야지 wait() 프로세스가 실행됨

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h>

int main(void)
{
	int pid;
	int status;
	//printf("before fork\n");
	pid = fork();
	//printf("after fork\n");

	if (pid == 0)
	{
		printf("child!!\n");
		//execlp("/bin/pwd", "bin/pwd", (char*)NULL);
		//printf("no print!\n");
	}
	else if (pid > 0)
	{
		wait(&status);
		printf("parent!!\n");
	}
}
```

결과

```c
child!!
parent!!
```

## exit() 시스템 콜

### kill(비자발적 종료)

```c
 int main(void)
{
	while(1)
	{}
}
```

위 프로그램 실행 후 다른 터미널을 열어서

```c
$ ps
 PID TTY           TIME CMD
 4841 ttys000    0:01.30 -zsh
 5686 ttys000    0:03.62 ./a.out
 5688 ttys001    0:00.05 -zsh
 $ kill 5686
```

while문이 실행된 터미널에서

```c
[1]    5686 terminated  ./a.out
```

- 돌아가고 있는 프로세스가 강제 종료됨.

# 프로세스 간 협력

![Untitled 1](https://user-images.githubusercontent.com/35272802/113575229-86ee8880-9658-11eb-9585-14f331a4605c.png)

- 기본적으로는 프로세스는 각각 독립적인 자원을 가지고 있음.
- 프로세스 간에 자원을 공유하려면 무조건 운영체제가 해줘야 함!
- 커널을 통해서 전달하는 방식에는 두가지가 있음.
    - 직접 전달.
    - 간접 적달.

# CPU 스케줄링

![Untitled 2](https://user-images.githubusercontent.com/35272802/113575387-bc937180-9658-11eb-8c4e-62122ddeb01a.png)å


- cpu를 사용하는 경우 → cpu burst
- i/o 인터럽트를 처리하는 경우 → I/O burst

![Untitled 3](https://user-images.githubusercontent.com/35272802/113575215-82c26b00-9658-11eb-841f-e7b23b7d40d8.png)

- IO 를 많이 하는 프로그램(사용자하고 상호작용을 많이 하는) → I/o 바운드 잡
- cpu를 한번에 오랫동안 사용하는 프로그램(1000 x 1000 배열) → cpu 바운드 잡
- 그래프를 봤을 때 io를 많이 하는 프로그램이 휠씬 많다!!

![Untitled 4](https://user-images.githubusercontent.com/35272802/113575197-7a6a3000-9658-11eb-95e2-ff435dfde04b.png)
- 스케줄러나 디스페쳐는 os 안에 코드로 되있다(하드웨어는 아님)
- nonpreemptive, preemptive 용어 암기, 이후에도 많이 사용됨.
