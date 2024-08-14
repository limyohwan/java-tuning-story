# 자바 성능 튜닝 이야기

## 1. 디자인 패턴 꼭 써야 한다
- MVC 패턴 
    - JSP Model 1(애매한 MVC)
    - JSP Model 2
- J2EE 디자인 패턴
    - Interceptor Filter Pattern: 요청 타입에 따라 다른 처리를 하기 위한 패턴
    - Front Controller Pattern: 요청 전후에 처리하기 위한 컨트롤러를 지정하는 패턴
    - View Helper Pattern: 프레젠테이션 로직과 상관 없는 비즈니스 로직을 헬퍼로 지정하는 패턴
    - Composite View Pattern: 최소 단위의 하위 컴포넌트를 분리하여 화면을 구성하는 패턴
    - Service to Worker Pattern: Front Controller와 View Helper 사이에 디스패치를 두어 조합하는 패턴
    - Dispatcher View Pattern: Front Controller와 View Helper로 디스패처 컴포넌트를 형성. 뷰처리가 종료될때까지 다른 활동을 지연한다는 점이 Service to Worker Pattern가 다름
    - Business Delegate Pattern: 비즈니스 서비스 접근을 캡슐화하는 패턴
    - Service Locator Pattern: 서비스와 컴포넌트 검색을 쉽게하는 패턴
    - Session Facade Pattern: 비즈니스 티어 컴포넌트를 캡슐화하고, 원격 클라이언트에서 접근할 수 있는 서비스를 제공하는 패턴
    - Composite Entity Pattern: 로컬 엔티티 빈과 POJO를 이용하여 큰 단위의 엔티티 객체를 구현
    - Transfer Object Pattern: 일명 Value Object Pattern 이라고 많이 알려져 있음. 데이터를 전송하기 위한 객체에 대한 패턴
    - Transfer Object Assembler Pattern: 하나의 Transfer Object로 모든 타입 데이터를 처리할 수 없으므로 여러 Transfer Object를 조합하거나 변형한 객체를 생성하여 사용하는 패턴
    - Value List Handler Pattern: 데이터 조회를 처리하고 결과를 임시 저장하며 결과 집합을 검색하여 필요한 항목을 선택하는 역할을 수행함
    - Data Access Object Pattern: 일명 DAO라고 많이 알려져 있음. DB에 접근을 전담하는 클래스를 추상화하고 캡슐화함
    - Service Activator Pattern: 비동기적 호출을 처리하기 위한 패턴
## 2. 내가 만든 프로그램의 속도를 알고 싶다
- 프로파일링 툴
    - 소스 레벨의 분석
    - 애플리케이션의 세부 응답 시간까지 분석 가능
    - 메모리 사용량을 객체나 클래스, 소스의 라인 단위까지 분석 가능
    - APM 툴에 비해 처렴함
    - 사용자수 기반 가격
    - 자바 기반의 클라이언트 프로그램 분석 가능
- APM 툴
    - 애플리케이션의 장애 상황에 대한 모니터링 및 문제점 진단이 주 목적임
    - 서버의 사용자 수나 리소스에 대한 모니터링 가능
    - 실시간 모니터링
    - 가격이 비쌈
    - CPU 수 기반 가격
    - 자바 기반의 클라이언트 프로그램 분석 불가능
- 메서드
    - System.currentTimeMillis
    - System.nanoTime
- 라이브러리
    - JMH
    - Caliper
    - JUnitPerf
    - JUnitBench
    - ContiPerf

### 절대 사용하면안되는 메서드
- static void gc(): 자바에서 사용하는 메모리를 명시적으로 해제하도록 GC를 수행하는 메서드
- static void exit(int status): 현재 수행중인 자바 VM을 멈춤
- static void runFinalization(): 참조 해제 작업을 기다리는 모든 객체의 finalize() 메서드를 수동으로 수행해야 함

## 3. 왜 자꾸 String을 쓰지 말라는 거야
- String: 새로운 객체를 생성함(메모리 사용 + 사용안하는 객체 GC 작업),
- StringBuilder: 기존 객체에 더하는 방식
- StringBuffer: 기존 객체에 더하는 방식, 쓰레드에 안전한 프로그래밍
- 위와 같은 특징으로 StringBuillder는 String 보다 512배, StringBuffer는 367배 빠르지만 메모리는 String이 StringBuilder와 StringBuffer보다 3390배 더 사용함
- JDK 5.0 이상이면 컴파일하게 되면 String 연산을 StringBuilder로 변경함

## 4. 어디에 담아야 하는지...
