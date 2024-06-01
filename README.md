# xv6-virtual-memory

이 프로젝트는 MIT의 xv6 운영체제를 기반으로 우선순위 스케줄러를 구현한 것입니다. 

## 원 프로젝트 정보

이 프로젝트는 [MIT xv6](https://pdos.csail.mit.edu/6.828/2022/xv6.html) 운영체제를 기반으로 합니다. xv6는 Unix v6를 x86 아키텍처에 맞게 재구현한 교육용 운영체제입니다. 

xv6의 소스 코드는 Frans Kaashoek, Robert Morris, Russ Cox가 저작권을 갖고 있습니다. 자세한 내용은 [GitHub 저장소](https://github.com/mit-pdos/xv6-riscv)를 참조하세요.

# MLFQ Scheduler

이 브랜치에서는 xv6 운영체제에 MLFQ(Multilevel Feedback Queue) Scheduler를 구현했습니다.

## 구현 내용

### MLFQ 큐 구조 구현

프로세스의 우선순위에 따라 3개의 큐(high, medium, low)를 사용하는 MLFQ를 구현했습니다. 각 큐는 연결 리스트로 구현되며, 프로세스의 `level`, `time_slice`, `next` 필드를 사용하여 관리됩니다.

### 프로세스 상태 변경 시 큐 관리

프로세스가 생성, 종료, 블록 등의 상태 변화를 겪을 때 적절한 큐에 삽입되도록 처리했습니다. 이를 위해 `push()`, `push_old()`, `pop()` 등의 큐 관리 함수를 구현했습니다.

### 스케줄러 수정

`scheduler()` 함수를 수정하여 우선순위가 높은 큐에서부터 프로세스를 선택하도록 변경했습니다. 같은 큐 내에서는 Round Robin 방식으로 프로세스를 실행합니다.

### 타임 슬라이스 관리

타이머 인터럽트 발생 시 현재 실행 중인 프로세스의 타임 슬라이스를 감소시키고, 타임 슬라이스가 소진되면 프로세스의 우선순위를 낮춥니다.
