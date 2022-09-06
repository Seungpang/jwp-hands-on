# 스레드

## 스레드 풀
### 스레드 풀은 무엇이고 어떻게 동작할까?
스레드 풀은 매번 생성 및 수거 요청이 올 때 스레드를 생성하고 수거하는 것이 아닌, 
스레드 사용자가 설정해둔 개수만큼 미리 생성하는 것이다.

+ 스레드: 어떤 프로그램 내에서 실행되는 흐름의 단위 (특히 프로세스 내에서 실행되는 흐름의 단위)

스레드는 동일한 메모리 영역에서 생성 및 관리가 이루어지지만, 생성하거나 수거할 때 커널 오브젝트를 동반한 리소스이므로
생성 비용이 크게 발생한다. 스레드를 제어할 수 없는 상태에서 스레드를 무차별적으로 생성하면 리소스가 빠르게 소진되는 상황이
발생할 수 있다. 그러면 어떻게 하면 효율적으로 스레드를 제어할 수 있을까? 스레드 풀의 동작 방식은 간단하게 보면 다음과 같다.

1. 병렬 작업의 형태로 동시 코드를 작성한다.
2. 실행을 위해 스레드 풀의 인스턴스에 제출한다.
3. 제출한 인스턴스에서 실행하기 위해 재사용되는 여러 스레드를 제어한다.


## WAS 스레드 설정하기
accept-count, max-connections, threads.max

### acceptCount
+ request Queue 길이를 정의합니다. 클아이언트가 HTTP Request를 요청했을 때 Idle Thread가 존재하지 않을 때, Idle Thread가 생길때까지 대기 길이이다.
+ 보통 큐에 메시지가 쌓여있다는 의미는 톰캣 인스턴스가 처리할 수 있는 쓰레드가 없다는 상황이며, 쓰레드를 사용해도 요청을 처리하지 못한다는 것을 이미 장애 상태일 가능성이 높다.
+ 디폴트 값은 100이다.

### maxConnections
+ 서버가 허용할 수 있는 최대 커넥션 수이다. 최대 커넥션 수가 도달하면 해당 메시지는 큐잉한다. 이때 큐 사이즈는 acceptConunt가 결정한다. 해당 큐에서 처리를 대기한다.
+ 디폴트 - NIO: 10000, APR/BIO: 8192

### maxThread
+ 톰캣 내의 스레드 수를 결정하는 옵션이다. 스레드의 수는 실제 Active User수를 의미한다. 즉 순간 처리 가능한 트랜잭션의 수이다. 그러나 너무 많은 수치는 스레드 context switch로 인해 되려 느려질 우려가 있다.
+ 해당 설정은 성능 테스트를 통해 서버 환경에 적절하게 조절하는 것이 좋다.
+ 디폴트는 200이다.