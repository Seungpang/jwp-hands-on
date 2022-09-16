## 서블릿 구현하기
### 1단계 - 서블릿 학습 테스트
+ SharedCounterServlet, LocalCounterServlet 어떤 차이점?
  + SharedCounterServlet: 인스턴스 변수가 있어서 다른 스레드와 공유가 되는 문제가 있음
  + LocalCounterServlet: 로컬 변수만 존재해서 다른 스레드와 공유가 되지 않음
  
+ init(): 클라이언트 요청이 들어오면 컨테이너는 서블릿이 메모리에 있는지 확인, 메모리에 없다면 init()메서드를 호출하여 적재
+ service(): 클라이언트 요청에 따라서 service() 메소드를 통해 요청에 대한 응답이 doGet(), doPost()로 분기, 이때 HttpServletRequest,
            HttpServletResponse에 의해 request, response 객체가 제공된다.
+ destory(): 컨테이너가 서블릿에 종료 요청을 하면 destory() 메서드가 호출된다. 종료시 처리해야하는 작업은 destory() 메서드를 오버라이딩하여 구현해야함

### 2단계 - 필터 학습 테스트
+ doFilter 메서드는 어느 시점에 실행될까?
  + filter init() -> servlet init() -> filter doFilter() -> service() -> servlet destory() -> filter destory()
+ 왜 인코딩을 따로 설정해줘야 할까?
  + "test/"로 시작하는 MIME 유형의 HTTP를 통해 전달된 문서의 기본 인코딩인 ISO-8859-1로 설정되어 있다. 따라서 한글이 안깨지려면 utf-8로 인코딩해줘야함
