소개
이 프로젝트는 여러 개의 멀티스레딩 예제를 다룹니다. 각 예제는 다양한 동기화 기법을 활용하여 멀티스레딩 환경에서 발생할 수 있는 경쟁 조건(race condition)과 이를 해결하기 위한 동기화 메커니즘을 보여줍니다. 프로그램들은 서로 다른 문제를 다루며, pthread 라이브러리를 사용하여 작성되었습니다.

프로그램 목록
책장 프로그램 (Producer-Consumer) - 경쟁 조건과 동기화 없이 책을 추가하고 대여하는 문제
책장 프로그램 (Producer-Consumer with Mutex and Condition Variables) - Mutex와 Condition Variables을 사용하여 경쟁 조건을 해결한 버전
도서 리뷰 시스템 (Reader-Writer 문제) - 리뷰를 읽는 독자와 작성하는 작가의 동기화 문제
도서 리뷰 시스템 (Reader-Writer 문제 with Mutex and Condition Variables) - Mutex와 Condition Variables을 사용하여 동기화 문제를 해결한 버전
컴파일 방법
모든 프로그램은 gcc 컴파일러를 사용하여 컴파일할 수 있습니다. 각 파일에 대해 다음 명령어를 사용하십시오.
bash
코드 복사
gcc -o bookshelf bookshelf.c -lpthread
gcc -o review review.c -lpthread
gcc -o review_mutex review_mutex.c -lpthread
gcc -o review_condition review_condition.c -lpthread
-lpthread 옵션은 POSIX 스레드 라이브러리를 링크합니다.
컴파일 후, 생성된 실행 파일을 실행할 수 있습니다.
bash
코드 복사
./bookshelf
./review
./review_mutex
./review_condition
동작 시험 결과
1. 책장 프로그램 (Producer-Consumer)
목표: 한 라이브러리 직원(librarian)이 책을 책장에 추가하고, 여러 명의 학생(student)들이 그 책을 대여합니다.
실행: 라이브러리 직원은 20권의 책을 추가하고, 3명의 학생은 각 10권의 책을 대여합니다. 책장 용량이 5로 제한되며, 책이 꽉 찼을 때 대기하는 동기화 문제를 다룹니다.
결과: 학생들은 책을 대여하고, 라이브러리 직원은 책을 추가합니다. 동기화가 없으면 경쟁 조건이 발생하고, 이는 버퍼 오버플로우나 언더플로우를 초래할 수 있습니다.
2. 책장 프로그램 (Producer-Consumer with Mutex and Condition Variables)
목표: Mutex와 Condition Variables를 사용하여 동기화 문제를 해결한 버전입니다.
실행: 책장을 관리하는 라이브러리 직원과 책을 대여하는 학생들이 동기화된 방식으로 작업을 수행합니다. 책장에 공간이 부족하면, 라이브러리 직원은 대기하고, 책이 없으면 학생들이 대기합니다.
결과: 경쟁 조건이 해결되었으며, 동기화된 작업 흐름으로 책을 안전하게 대여하고 추가할 수 있습니다.
3. 도서 리뷰 시스템 (Reader-Writer 문제)
목표: 여러 명의 독자(reader)들이 리뷰를 읽고, 작가(writer)들이 리뷰를 작성하는 프로그램입니다. 독자들은 리뷰를 읽을 때 다른 독자들이 읽고 있을 경우 충돌 없이 읽어야 하고, 작가는 리뷰를 작성할 때 독자들이 읽지 않도록 해야 합니다.
실행: 작가와 독자들이 번갈아가며 작업을 수행합니다. 경쟁 조건이 발생할 수 있으며, 이를 동기화로 해결합니다.
결과: 작가가 리뷰를 쓸 때, 독자들은 해당 리뷰를 읽을 수 없습니다. 독자들이 리뷰를 읽을 때, 다른 독자들이 읽고 있어도 충돌이 없으며, 동시에 작성된 리뷰는 모두 공유됩니다.
4. 도서 리뷰 시스템 (Reader-Writer 문제 with Mutex and Condition Variables)
목표: Mutex와 Condition Variables를 사용하여 동기화 문제를 해결한 버전입니다. 작가는 독자가 읽는 동안 리뷰를 작성할 수 없으며, 독자들은 작가가 작업하는 동안 리뷰를 읽을 수 없습니다.
실행: 이전과 동일하지만, pthread_mutex와 pthread_cond를 활용하여 정확한 동기화를 구현합니다.
결과: 작가는 독자가 없을 때만 리뷰를 쓸 수 있고, 독자는 작가가 작업 중일 때는 대기합니다. 동기화가 잘 이루어지며, 경쟁 조건이 발생하지 않습니다.
토의
1. 책장 프로그램 (Producer-Consumer)
이 예제는 공유 자원인 책장에 대해 여러 스레드가 접근하면서 발생할 수 있는 경쟁 조건을 다룹니다. 동기화 없이 접근할 경우, 예기치 못한 동작(예: 책을 덮어씌우는 경우)이 발생할 수 있습니다. 첫 번째 버전은 동기화 없이 경쟁 조건을 유도하여 이를 관찰할 수 있도록 하였습니다. 두 번째 버전은 pthread_mutex와 pthread_cond를 활용하여 동기화를 통해 경쟁 조건을 해결합니다.

2. 도서 리뷰 시스템 (Reader-Writer 문제)
이 문제는 독자와 작가가 동시에 작업할 때 발생할 수 있는 충돌을 다룹니다. 독자는 다수일 수 있지만, 작가는 한 명만 작업할 수 있습니다. 첫 번째 버전에서는 간단히 경쟁 조건을 유발하고, 두 번째 버전에서는 Mutex와 Condition Variable을 사용하여 정확한 동기화 문제를 해결합니다. 이를 통해 멀티스레딩에서 데이터 무결성을 유지하는 방법을 배울 수 있습니다.

3. 도서 리뷰 시스템 (Reader-Writer 문제 with Mutex and Condition Variables)
Mutex와 Condition Variables를 적절히 사용하면, 멀티스레딩 환경에서 발생할 수 있는 동기화 문제를 해결할 수 있습니다. 두 번째 버전에서는 Mutex로 보호되는 공유 자원에 대한 동기화뿐만 아니라, Condition Variable을 사용하여 읽기/쓰기 시점을 제어할 수 있습니다. 이를 통해 프로그램의 동작이 일관되게 유지됩니다.

느낀점
멀티스레딩을 구현하는 과정에서 가장 중요한 점은 데이터의 무결성을 보장하는 것입니다. 경쟁 조건이 발생할 경우, 예기치 않은 결과가 나올 수 있기 때문에 동기화 방법을 적절히 사용해야 합니다. pthread_mutex와 pthread_cond를 활용한 동기화 기법은 멀티스레딩 환경에서 중요한 역할을 합니다. 또한, volatile을 사용하여 메모리 일관성을 보장할 수 있는 방법을 알게 되어 유용했습니다.

각각의 예제에서 경쟁 조건을 어떻게 유발하고, 이를 어떻게 해결할 수 있는지에 대해 실험하면서 멀티스레딩 프로그래밍의 핵심을 이해하게 되었습니다.

결론
이 프로젝트는 멀티스레딩의 기본적인 문제들과 이를 해결하는 다양한 방법을 배우는 데 도움이 됩니다. 경쟁 조건을 해결하기 위한 다양한 동기화 기법을 적용해 보며, 실제로 시스템에서 멀티스레드 작업을 처리할 때 고려해야 할 점들을 학습할 수 있었습니다.

위의 내용으로 README 파일을 업데이트해 주시길 권장합니다. 4개의 프로그램 모두 정확하게 포함되어 있으며, 각 프로그램의 동작 및 해결책을 설명하고 있습니다.











ChatGPT는 실수를 할 수 있습니다. 중요한
