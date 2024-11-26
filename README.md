## 소개

이 프로젝트는 Producer/Consumer와 Reader/Writer 모델을 기반으로 한 각자 생각하는 프로그램을 제작하고, 동기화 기능이 없을 때 race condition이 발생함을 확인하고, 이를 해결하기 위하여 동기화 기능을 추가하는 프로젝트입니다.

### 프로그램 목록

1. **책장 프로그램 (Producer-Consumer)** - 동기화 없이 책을 추가하고 대여하는 문제
2. **책장 프로그램 (Producer-Consumer with Mutex and Condition Variables)** - Mutex와 Condition Variables을 사용하여 race condition을 해결한 버전
3. **도서 리뷰 시스템 (Reader-Writer 문제)** - 동기화 없이 리뷰를 읽는 독자와 작성하는 작가 문제
4. **도서 리뷰 시스템 (Reader-Writer 문제 with Mutex and Condition Variables)** - Mutex와 Condition Variables을 사용하여 race condition를 해결한 버전

## 컴파일 방법

1. 모든 프로그램은 `gcc` 컴파일러를 사용하여 컴파일할 수 있습니다.

```bash
gcc procon.c -o procon -lpthread
gcc proconmutex.c -o proconmutex -lpthread
gcc rw.c -o rw -lpthread
gcc rwmutex.c -o rwmutex -lpthread
```
-lpthread 옵션은 POSIX 스레드 라이브러리를 링크합니다.

2. 컴파일 후, 생성된 실행 파일을 실행할 수 있습니다.
```bash
./procon
./proconmutex
./rw
./rwmutex
```

## 동작 시험 결과

### 1. 책장 프로그램 (Producer-Consumer)
- **결과**: 학생들은 책을 대여하고, 라이브러리 직원은 책을 추가합니다. 동기화가 없어 race condition이 발생하는 모습을 볼 수 있습니다.
![image](https://github.com/user-attachments/assets/918dcb95-f6ab-49d1-98b3-9a33e9e03f6d)


### 2. 책장 프로그램 (Producer-Consumer with Mutex and Condition Variables)
- **결과**: race condition이 해결되었으며, 동기화된 작업 흐름으로 책을 안전하게 대여하고 추가할 수 있습니다.
![image](https://github.com/user-attachments/assets/3f966d3e-2471-4a9f-b7f3-9505058ff229)


### 3. 도서 리뷰 시스템 (Reader-Writer 문제)
- **결과**: 동기화가 없어 작가가 리뷰를 쓸 때 독자들이 해당 리뷰를 읽거나, 독자들이 리뷰를 읽을 때 다른 독자들이 읽고 있으면 race condition이 발생한다. 
![image](https://github.com/user-attachments/assets/79678bdf-b24e-4403-af4e-0e2fefcb5030)


### 4. 도서 리뷰 시스템 (Reader-Writer 문제 with Mutex and Condition Variables)
- **결과**: 작가는 독자가 없을 때만 리뷰를 쓸 수 있고, 독자는 작가가 작업 중일 때는 대기합니다. 동기화가 잘 이루어지며, race condition이 발생하지 않습니다.
![image](https://github.com/user-attachments/assets/53ec0ded-578e-4793-85cd-28391975bfbe)


## 토의 및 느낀점
멀티스레딩을 구현하는 과정에서 가장 중요한 점은 데이터의 무결성을 보장하는 것입니다. race condition이 발생할 경우, 예기치 않은 결과가 나올 수 있기 때문에 동기화 방법을 적절히 사용해야 합니다. `pthread_mutex`와 `pthread_cond`를 활용한 동기화 기법은 멀티스레딩 환경에서 중요한 역할을 한다는 것을 알게 되었습니다. 또한, `volatile`을 사용하여 메모리 일관성을 보장할 수 있는 방법을 알게 되어 유용했습니다. 각각의 예제에서 경쟁 조건을 어떻게 유발하고, 이를 어떻게 해결할 수 있는지에 대해 실험하면서 멀티스레딩 프로그래밍의 핵심을 이해하게 되었습니다.

이 프로젝트를 통해 멀티스레딩의 기본적인 문제들과 이를 해결하는 다양한 방법을 배우는 데 도움이 되었습니다. 경쟁 조건을 해결하기 위한 다양한 동기화 기법을 적용해 보며, 실제로 시스템에서 멀티스레드 작업을 처리할 때 고려해야 할 점들을 학습할 수 있었습니다.
