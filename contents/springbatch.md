# spring batch

**:Contents**
* [왜 spring batch 인가] 
* [spring batch란]
* [처리대상이 많은 데이터는]
* [멱등성 유지하기]

### 왜 spring batch 인가?

* 효과적인 로깅, 통계 처리, 트랜잭션 관리 등 재사용 가능한 필수 기능을 지원한다.
* 수동으로 처리하지 않도록 자동화되어 있다.
* 예외사항과 비정상 동작에 대한 방어 기능이 있다.
* 스프링 부트 배치의 반복되는 작업 프로세스를 이해하면 비즈니스 로직에 집중 할 수 있다.


-----------------------------------------------

### standardize process flow위한 기능

#### Tasket model
* 간단한 process에 사용

#### Chunk model
* 대량의 데이터를 효율적으로 처리
* 고정 된 양의 데이터를 일괄 적으로 read / process / writer 하는 방법

### 다양한 데이터 형식의 I / O
* 파일, 데이터베이스, 메시지 큐 등과 같은 다양한 데이터 리소스에 대한 입출력을 쉽게 수행 할 수 있습니다.

### 효율적인 처리
* 설정에 따라 다중 실행, 병렬 실행, 조건 분기가 수행됩니다.

### 작업 실행 제어
* 데이터 레코드를 표준으로 사용하여 영구 실행, 재시작 작업을 수행 할 수 있습니다.




### spring batch 주의 사항


### spring batch의 기본 구조

![A](imgs/batch_structure.PNG)

* Job과 Step 1:M
* Step과 ItemReader, ItemProcessor, ItemWriter 1:1
* Tasklet 하나와 Reader & Processor & Writer 한 묶음이 같은 레벨이다.

![A](imgs/springbatch_step.PNG)

### 청크 지향 프로세싱 프로세스

![A](imgs/batch_chunk.PNG)

#### 용어 정리
`Job`<br>
* Spring Batch의 배치 애플리케이션을위한 일련의 프로세스를 요약하는 단일 실행 단위입니다

`Step`<br>
* Job을 구성하는 처리 단위입니다. 하나의 작업은 1 ~ N 개의 단계를 포함
* 단계는 청크 모델 또는 태스크 모델 (나중에 설명)에 의해 구현됩니다.

`JobLauncher`<br>
* 작업 실행을위한 인터페이스입니다.

`JobRepository`<br>
* Job 및 Step의 상태를 관리하는 시스템입니다.<br>
관리 정보는 Spring Batch에서 지정한 테이블 스키마를 기반으로 데이터베이스에 유지됩니다.


https://terasoluna-batch.github.io/guideline/5.0.1.RELEASE/en/Ch02_SpringBatchArchitecture.html#Ch02_SpringBatchArch_Overview



-----------------------------------------------

### 처리대상이 많은 데이터를 처리하는 경우 어떻게 처리를 하였는지?

>> 파티셔닝을 사용한 병렬 프로그래밍 방식을 이용하여 처리 속도 개선을 하였습니다.

* partitioner gridSize는 3으로 설정하여 사용
gridSize가 크다고 무조건 좋은게 아니다.<br>
스레드를 생성하여 사용하기 때문에 자원과 연결이 되어 있기때문에 실무에서 사용하는 서버에 상황에 맞게 설정하여<br>
최적화해야 한다.



-----------------------------------------------

### Spring Batch의 멱등성 유지하기

`멱등성`<br>
* 연산을 여러번 적용하더라도 결과가 달라지지 않는 성질

 Spring Batch를 사용하다보면 동적으로 변하는 날짜를 사용하는 경우가 있습니다.
 ex).<br>
*  매일 한번 어제 매출 데이터를 집계해야할 때
*  현재 시간을 기준으로 유효기간이 만료된 포인트를 정리할 때
*  매일 한번 오늘을 기준으로 휴면회원 처리를 할 때

그래서 주로 LocalDate.now()를 통해 그날 그날의 데이터를 집계합니다.<br>
하지만 이슈가 발생해서 어제 데이터를 다시 처리할 필요가 생긴다면 LocalDate.now()로 날짜를 정해서 처리하는 방식에는 문제가 생깁니다.<br>

`이런 문제가 발생한 이유는`<br>
* 제어할 수 없는 코드가 내부에 있기 때문
* new Scanner(System.in), LocalDate.now(), new Random() 등 결과를 개발자가 제어할 수 없는 코드는 내부에서 사용 하면 안됩니다.

>> 그래서 실무에서는 jobParameter값에 날짜가 없는 경우에만 LocalDate.now() 날짜를 정해서 집계를 하는 방식으로 운영하고 있습니다. 