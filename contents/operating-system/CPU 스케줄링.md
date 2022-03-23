# CPU 스케줄링

> 메모리에 여러개의 프로세스가 있는 **멀티 프로그래밍** 환경에서 CPU의 이용률을 최대화하기 위한 과정이다.  
> **CPU 이용률** 을 최대화 하는 것은 다중 프로세서의 핵심이라고 할 수 있다.  
> 실행 준비가 된 프로세서 중에서 그 중 하나를 선택해 CPU에 할당한다.
> <br>

## 스케줄링 기법

> 스케줄링 기법은 크게 **선점(Preemptive)과 비선점(Non-preemptive)** 기법으로 나뉜다.

### Preemptive

- 선점 스케줄링
- 어떤 cpu를 한 프로세스가 선점하고 있어도 새롭게 선점할 수 있다.

### Non-Preemptive

- 비선점 스케줄링
- 어떤 cpu를 한 프로세스가 선점하고 나면 그 프로세스가 자발적으로 나오기 전까지는 바꾸지 않는다.

## Cpu 스케줄링 결정하는 경우

![cpu스케줄링](./img/cpu%EC%8A%A4%EC%BC%80%EC%A4%84%EB%A7%81.jpg)

1. 프로세스가 running 상태에서 waiting 상태로 변경된 경우
2. 프로세스가 running 상태에서 ready 상태로 변경된 경우
3. 프로세스가 waiting 상태에서 ready 상태로 변경된 경우
4. 프로세스가 terminate(종료) 될 경우

## 결정에 따라 분류 되는 선점 비선점

- Non-preemptive (1,4번 비선점)

  - running -> wait
  - running -> terminate

  **CPU의 상태가 종료 혹은 대기 상태로 변경되기 때문에 무조건 비선점**

- preemptive (2,3번 선점 or 비선점)

  - running -> ready
  - waiting -> ready

  **CPU의 상태가 준비 상태로 변경되기 때문에 무조건 선택 가능**

## 스케줄링의 종류

### First Come First Served(FCFS)

- 비선점형 스케줄링
- 레디큐에 먼저 도착한 프로세스를 먼저 진행.
- 프로세스가 CPU를 점유하면 종료(cpu burst)시까지 CPU를 계속 점유하고 있는다.

- 선행되는 프로세스가 오랜 running time을 가지게 된다면 waiting time이 늘어나 효율성을 낮춘다

### Shortest-Job-First(SJF)

- 비선점 스케줄링
- 종료시간(burst)이 가장 짧은 프로세스 부터 CPU를 할당하나.
- 시간이 긴 프로세스는 오랜시간 실행되지 못하는 기아 상태가 발생할 수 있다.

### Shortest-Remaining-Time-First(SRTF)

- 선점 스케줄링
- SJF를 선점 형태로 바꾼 스케줄링 기법이다
- 새로운 프로세스의 요구시간이 현재 CPU를 점유중인 프로세스의 남은 요구시간보다 적다면, CPU를 선점한다.
- 가작 적은 waiting time을 가질 수 있다.
- 요구시간이 긴 프로세스들은 SJF 스케줄링보다 더 많은 기아 상태를 발생시킬 수 있다.

### Round-Robin(RR)

- 선점 스케줄링
- 레디큐에 있는 모든 프로세스가 time quantum (시간을 쪼갠 단위)를 가진다. 각 프 로세스는 1tq 동안 CPU를 점유한다.
- 1tq 만큼 점유한 프로세스는 레디큐의 끝에 다시 추가된다.

- time quantum 을 끊는 정도에 따라 성능이 달라진다
  - time quantum를 길게 설정하면 FCFS 방식과 유사하다
  - time quantum를 짧게 설정한다면, Context switching에 많은 리소스를 소비하게 된다

### Priority

    - 선점 & 비선점 모두 가능
    - 각 프로세스가 우선순위를 가진다.
    - 우선순위가 높은 프로세스부터 CPU를 점유한다
        > 낮은 우선순위를 가진 프로세스는 무한 봉쇄와 기아 상태를 가질 수 있다.

- aging: waiting time이 길어 시스템에 오래 남아있는 프로세스들의 우선순위를 점진적으로 증가시킨다.

- RR 응용: 우선순위가 동일한 프로세스를 RR을 적용시킨다

### Highest Response Ratio Next(HRRN)

    - 비선점 스케줄링
    - SJF의 문제점인 작업시간별 불평등을 보완하였다.
    - 아래의 공식으로 우선순위를 결정한다

```
>> HRRN 공식
대기시간 + 수행시간 / 수행시간
```

    - 대기시간이 길 수록 우선순위는 감소한다.
    - 수행시간이 짧을수록 우선순위는 증가한다.

> 문제1: 선점과 비선점의 차이

> 문제2: 비선점형 스케줄링을 말하고 간략히 설명하라

> 문제3: RR 스케줄링에서 time quantum 이 작을때와 클때에 문제점을 말하라
