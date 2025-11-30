# Linux_commands
## 리눅스 명령어 top, ps, jobs, kill 조사(컴퓨터공학과_20205077_정해윤)
### 1. top 명령어
**현재 OS의 상태를 나타내주는 CLI 어플리케이션으로 CPU, 메모리 사용률, 프로세스 상태 등을 확인하는 명령어**
> 실시간 시스템 모니터링, 프로세스 관리, 정렬 기준 변경의 기능 수행


- 기본 사용법
~~~bash
top
top -u userName    # 특정 사용자 프로세스만 보기
top -p PID         # 특정 PID만 보기
~~~
<details>
<summary>실행화면</summary>
  <img width="618" height="385" alt="image" src="https://github.com/user-attachments/assets/198b8721-d208-44d1-84b0-f3aaccc41768" />

</details>

- 주요 화면 구성
  
| 항목 | 설명 |
|:-----:|:-----:|
|PID|프로세스 ID|
|USER|실행 사용자|
|%CPU|CPU 사용률|
|%MEM|메모리 사용률|
|TIME+|누적 CPU 사용 시간|
|COMMAND|실행된 프로그램 이름|


- 자주 쓰는 단축키

| 항목 | 설명 |
|:---:|:---:|
|q|top 종료|
|P|CPU 사용률 기준 정렬|
|M|메모리 사용률 기준 정렬|
|N|프로세스 ID 기준 정렬|
|T|러닝타임 기준 정렬|
|k|프로세스 종료|
|h|도움말|
---
### 2. ps 명령어
**현재 실행 중인 프로세스의 목록과 상태를 스냅샷 형태로 한 번만 출력**
> top은 실시간, ps는 정적 출력

- 기본 사용법
~~~bash
ps
ps -e           # 전체 프로세스 출력
ps -f           # 프로세스의 자세한 정보(부모 프로세스 ID 등)를 함께 출력
ps aux          # 상세 정보 출력(BSD 스타일)
ps -ef          # 상세 정보 출력(System V 스타일)
~~~
<details>
<summary>실행화면</summary>
  <summary>ps</summary>
<img width="632" height="189" alt="image" src="https://github.com/user-attachments/assets/d8ad140d-d534-423f-aa88-7257d115bae5" />
  <summary>ps aux</summary>
<img width="632" height="189" alt="image" src="https://github.com/user-attachments/assets/6cbc38cd-c442-40cd-929c-4258968082b0" />

</details>

- 주요 컬럼

| 컬럼 | 설명 |
|:-----:|:-----:|
|PID|프로세스 ID|
|%CPU|CPU 사용률|
|%MEM|메모리 사용률|
|STAT|프로세스 상태(R/S/T/Z 등)|
|START|시작 시간|
|COMMAND|실행 명령어|

- 자주 사용하는 조합
~~~bash
ps aux | grep python     # 모든 실행 중인 프로세스 목록에서 Python 관련 프로세스만 필터링하여 확인
ps -ef | grep nginx      # 특정 프로세스(nginx)가 실행 중인지 확인
~~~
---
### 3. jobs 명령어
**현재 쉘 세션에서 백그라운드로 실행 중인 작업이나 정지된 작업을 확인하는 명령어**

- 기본 사용법
~~~bash
jobs
jobs -l       # 작업 번호 + PID 표시
~~~

- 주요 옵션
  
| 옵션 | 설명 |
|:-----:|:-----:|
|-l|일반 정보와 함께 프로세스 그룹ID(PGID) 추가 표시|
|-p|해당 작업 그룹의 프로세스ID(PID)만 표시|
|-r|Running 상태인 작업만 표시|
|-s|Stopped 상태인 작업만 표시|


- 관련 명령어

| 명령어 | 설명 |
|:-----:|:-----:|
|bg %1|1번 job을 백그라운드에서 재실행|
|fg %1|1번 job을 포그라운드로 가져오기|
|kill %1|1번 job 종료|

- 예시
~~~bash
sleep 100 &
jobs
~~~

- 출력 예시
~~~text
[1]+  Running    sleep 100 &
~~~
---

### 4. kill 명령어
**특정 PID에 시그널을 보내 종료하거나 특정 동작을 수행하도록 지시**
> 시그널은 프로세스에게 특정 동작(종료, 재시작, 설정 파일 다시 로드 등)을 하도록 지시

- 기본 사용법
~~~bash
kill PID          # 부드러운 종료(SIGTERM, 15)
kill -9 PID       # 강제 종료(SIGKILL)
kill -SIGINT PID  # Ctrl + C와 동일한 효과
kill -l           # 신호 목록
~~~

- 주요 시그널
> 시그널은 이름이나 번호로 지정 가능

| 시그널 번호 | 시그널 이름 | 설명 | 용도 |
|:--------:|:---------:|:--:|:---:|
|1|SIGHUP|Hangup(재시작)|프로세스에게 설정 파일을 다시 로그하거나 연결을 끊는 것을 지시(서버 프로세스를 재시작 없이 설정만 변경할 때 자주 사용)|
|2|SIGINT|Interrupt|터미널에서 "Ctrl + C"를 눌렀을 때 보내는 시그널로 프로세스가 정상적으로 종료될 수 있도록 함|
|9|SIGKILL|Kill(강제 종료)|프로세스에게 즉시 종료하라고 지시하며 프로세스는 이 시그널을 무시하거나 포착할 수 없음|
|15|SIGTERM|Termination(정상 종료)|프로세스에게 정상적으로 종료할 시간을 주고 정리 작업을 수행하도록 지시하는 시그널(kill 명령어 사용 시 기본값)|

- 주의사항
***프로세스를 종료할 때는 SIGTERM(15)를 먼저 사용해보고 프로세스가 응답하지 않을 때만 SIGKILL(9)을 사용하는 것을 권장***
> SIGTERM(정상 종료)는 프로세스가 데이터를 저장하거나 리소스를 정리할 시간을 주지만 SIGKILL(강제 종료)는 프로세스가 정리 작업을 할 수 없어 데이터 손상이나 리소스 누수 발생 가능
---
- 관련 참고 문서
> https://docs.rockylinux.org/10/ko/books/admin_guide/08-process/
