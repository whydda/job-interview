# job-interview
면접볼때 예상질문들


1. Java
	* Optional
		* null은 에러의 근원
		* 코드 가독성을어렵게 한다
		* 자바 철학에 위배 된다 자비는 포인터가 없는데 null만 예외다
		* Optional 을 사용하면 null 회피나 exception에서 자유로울 수 있다.
		* Optional은  일종의 랩핑클래스이다. 
	* Map, FlatMap, Reduce 등
		* Map 은 스트림에 전달받은 요소를 가지고 새로운 스트림을 만든다.
		* FlatMap 은 이차원적인 데이터 스트림을 하나의 스트림으로 병합하여 평준화 시킴
		* Reduce 는 첫번째 두번째 파라미터를 누적시켜서 요소가 한개가 될때까지 반복적으로 요소를 계산한다.
			* 내부 반복이 추상화 되면서 병렬로 실행하기 쉽다. 일반 for문을 사용해서 병렬처리를 하면 sum 변수를 동기화 시켜야하므로 이득이 없다.
		* findAny 는 병렬상황에서 요소의 반환 순서가 상관없을경우  		
	* Fork/Join 프레임워크 
		* 병렬화할 수 있는 작업을 재귀적으로 작은 작업으로 분할한 다음에 서브태스크 각각의 결과를 합쳐서 전체 결과를 만들도록 설계되었다. 
		* 디버깅하기어렵다. 
		* 병렬처리가 순차적 접근보다 항상 빠른게 아니다.  
		* 태스크를 여러 독립적인 서브태스크로 분할할 수 있어야한다. 
		* 예를들어 하나의 API요청을 받은 컨트롤러에서 다시 여러 API들을 병렬적으로 호출해서 결과값을 받은 후 모든 결과를 합쳐서 응답을 내려준다면 ? fork/join이 순차적 처리보다 빠를것이다. 
		* job stealing
	* 람다식 장/단점
		* 람다식을 사용하면 기존에 filter, map, reduce 등을 반복문을 통해 직접 구현했던 것을 내부구현이 되어있는 람다함수를 통해 정해진 의도의 코드를 간결하게 만들 수 있다. 
		* 구현이 편하고 버그가 적을것이다. 
		* 병렬 프로그래밍에 유리하다.
		* 디버깅이 어려워질수있고 자바의 경우 클로저가 지원되지 않는다.
		* 재귀처리가 힘듬
	* 일급객체
			* 함수를 값이나 인자, 리턴타입으로 취급하는것
	* Marker Interface
		* 구현할 추상 메서드는 없고 JVM에 특별한 지시를 하기위한 인터페이스 
		* 예) Serializable
	* Functional Interface
		* 구현해야할 함수가 한개이고 @FunctionalInterface(옵션) 가 적용되어 있음
		* Consumer 
			* forEach 에서 사용 forEach(Consumer) void accept()
		* Function
			* map 에서 사용 map(Function) Stream apply ()
			* 같은 타입을 리턴하는 identity() 
		* Predicate
			* filter 에서 사용 filter(Predicate) boolean test()
		* Supplier 
			* collect 에서 사용 collect(Collector)  T get()
		
	* 지연종결 메서드
		* findAny, findFirst, sum, count, reduce, collect 등
	* 클래스로더
		* JAVA 클래스를 JVM 에 동적으로 로드하는 런타임의 일부
			* 부트스트랩 
				* /jre/lib 에 위치한 핵심 라이브러리를 로드
			* 확장 클래스 로더
				* /jre/lib/ext 에 위치한 핵심 라이브러리를 로드 
			* 시스템 클래스 로더 
				* classpath 환경 변수에 맵핑된다.
	*  GC 알고리즘에 대한 설명
		* Heap 메모리엔 Young, Old 영역이 존재한다.
		* Young 영역에는 Eden, Survivor1,2 영역이 존재하는데 새로 생성된 객체는 Eden 에 저장되고 Minor-GC가 발생하면 Survivor1,2 로 이동하는데 해당 Survivor 영역이 가득차면 다른 Survivor 로 옮겨지고 여기서 살아남은 객체들은 Old 로 옮겨진다.
		* Old 영역에서 일어나는 GC를 Major-GC 라고 한다. 
		* Stop The World ?
			* JVM에서 GC가 발생하면 모든 스레드들은 행동을 멈추고 GC가 끝날때까지 대기하게되는데 이걸 Stop The World라고 한다. 이기간이 길어질수록 어플리케이션은 정상적으로 동작하지않느다. 
		* 종류
			* Serial GC
				* 위의 동작을 싱글로 작동하는게 Serial GC 
			* Parallel GC 
				* Serial GC가 병렬적으로 동작하는걸 말함. Java8까진 default
			* G1CC
				* old,young으로 나뉘어져있지 않고, region 이라는 영역에 객체를 할당한다.  그러다가 꽉차면 다른 region에 객체를 할당하고 정리한다.
				* Java9 부터 default가 되었음.

		
2. Spring
	* [스프링캠프 2016 발표 - Deep dive into spring boot autoconfiguration](https://www.slideshare.net/sbcoba/2016-deep-dive-into-spring-boot-autoconfiguration-61584342)
	* AOP
		* 핵심관심사란 비즈니스로직의 핵심이되는 로직을 말함 회원가입이라면 유저가 존재하는지 확인하고 유저를 create 하는게 핵심관심사
		* 횡단관심사란 비즈니스로직에서 핵심이 아닌 optional한 로직을 말함. 로깅이나 Audit , 트랜잭션등 
	* IOC
		* 제어의 역전은 애플리케이션의 라이프사이클을 프로그래머가 아니라 프레임워크에게 맡기는것. 
		* 라이브러리와 프레임워크의 차이는 IOC개념이 있고 없고의 차이라고 볼 수 있음
	* DI
		* 의존관계주입은 객체의 라이프사이클을 스프링의 IOC 컨테이너가 관리하게끔 만드는것이다. Spring의 경우 BeanFactory에 DI 를 위한 기능이 들어가있으며, ApplicationContext에서 BeanFactory를 상속받고 J2EE 관련 기능들이 더 구현되어있다.
	* AutoConfiguration
		* SpringBoot의 @EnableAutoConfiguration 은 META-INF/spring.factories 파일에있는 autoconfigure 클래스들을 읽고,
		* application.properties 에서 설정을 추가하면 autoconfigure에 있는 클래스의 설정값에 바인딩된다. 
		* 이덕분에 우리는 불필요하고 중복되는 설정을 직접 구현할 필요가없다.
		* autoconfigure클래스에는 @Conditional* 이 존재해서 어느 시점에 클래스가 로딩될지를 정한다.
		* @ConditionalOnBean,  @ConditionalOnClass, @ConditionalOnMissingBean, @ConditionalOnProperty
	* COC
		* 설정 보다 관례는 개발을 할때  어떤 프레임워크나 라이브러리를 사용하게 될때 설정을 해야하는 부담을 벗어나기 위해 자주 사용하는 부분은 관례를 정하고 이에 벗어나는 경우만 설정을 하면된다.
	* Transactional 단계
		* 트랜잭션은 ACID 를 보장해야한다.
		* 원자성 : 트랜잭션내에서 작업들은 한몸인것처럼 모두 성공 또는 모두 실패해야함
		* 일관성 : 데이터베이스에서 정한 무결성 제약 조건을 만족해야한다.
		* 격리성 : 동시 실행되는 트랜잭션들이 서로에게 영향을 미치지 않도록 격리한다.
		* 지속성 : 트랜잭션이 성공적이라면 그 결과는 영구적이어야한다.
		* READ UNCOMMITED
			* 커밋되지 않은 데이터를 읽을 수 있다.  이것을 dirty read 라 하는데 아직 커밋되지 않은 데이터가 롤백되면 그걸 참조하는 트랜잭션은 데이터 정합성 문제가 발생한다.
		* READ COMMITTED 
			* 커밋한 데이터만 읽을 수 있다. 하지만 NON-REPEATABLE READ는 발생할 수 있다. 만약 트랜잭션1이 데이터를 읽는중에 트랜잭션2가 데이터를 수정하면 수정한 데이터를 읽게된다.
		* REPEATABLE READ 
			* 한번 조회한 데이터를 반복해서 조회해도 같은 데이터가 조회된다. phantom read가 발생할 수 있다. 트랜잭션1이 조회했는데 트랜잭션2가 데이터를 추가 커밋하면 트랜잭션이1이 다시 데이터를 조회하면 데이터가 추가된 상태로 조회된다.
		* SERIALIZABLE
			* 가장 엄격한 격리수준이나, 동ㅂ시성이 급격히 떨어질 수 있다.
	
3. Test
	* Test 관련 어노테이션 이름 및 왜 사용하는지?
	* 테스트 슬라이스
		* TestEntityManager
		* DataJpaTest
		* MockMvc
	* Mock 
		* when vs given 차이
			* BDD vs TDD
				* BDD  는 좀더 사람의 언어에 가까운 접근방식
	* assertJ vs JUnit
	
4. Java mail
	* SMTP 포트 25 or 465
	* IMAP 포트 143 or 993
		* 온오프라인 모드가 지원되어 서버에서 fetch 받고 delete 하진 않고, 개인 메일함도 동기화 가능. 트래픽이 무겁고 여럿이서 쓰기 좋음
	* POP3 포트 110 or 995
		* 기본적으로 fetch delete 형식으로 서버에서 가져오면 데이터를 지우고, 메일함이 동기화되진 않음. 트래픽이 가볍고 혼자쓰기 좋음
	* Email 정규식
		* /^[a-zA-Z0-9._-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,6}$/
	* MX
		* @도메인의 mail exchange 주소 
	* SPF
		* 일종의 화이트리스트 
		* spf 에 등록된 메일을 제외하고는 모두 차단됨 
	* PTR
		* Reverse DNS 질의방식
		* 발신측의 메일서버 IP 를 기준으로 도메인을 역조회
	
5. MQ 사용해본것
	* Kafka
		* 장점
			* 발행자 구독자패턴 
			* 분산 대용량에 최적화 되있는 메세지 서버스
			* 각각의 파티션의 offset 을 기준으로 데이터를 가져온다. 
		* 단점
			* 파티션이 여러개일경우 파티션의 순서는 상관없이 데이터를 가져온다. 
			* offset 이 변경되면 데이터를 사라진다. 
		* 파티션?
			* Kafka 각각의 인스턴스를 이야기함
		* 토픽?
			* 데이터를 구분하기 위한 일종의 네임스페이스 
			* 토픽을 기준으로 발행/구독을 한다.
		* 컨슈머 그룹?
			* 컨슈머들을 그룹핑화시킨것
			* 장애극복
			* 컨슈머 그룹은 자기 그룹에 대한 OFFSET을 관리한다.
			* 컨슈머 그룹이 없다면 데이터의 정합성을 지키기 어렵다.
			* 파티션하나에 컨슈머 그룹의 대표 컨슈머 인스턴스만 접근이 가능하다.
	
		* [Kafka 운영자가 말하는 처음 접하는 Kafka | Popit](https://www.popit.kr/kafka-%EC%9A%B4%EC%98%81%EC%9E%90%EA%B0%80-%EB%A7%90%ED%95%98%EB%8A%94-%EC%B2%98%EC%9D%8C-%EC%A0%91%ED%95%98%EB%8A%94-kafka/)
		* [Kafka 운영자가 말하는 Kafka Consumer Group | Popit](https://www.popit.kr/kafka-consumer-group/)

6. JPA
	* ORM 은 객체와 테이블을 매핑해서 패러다임의 불일치 문제를 개발자 대신 해결해준다.
		* 객체를 다루듯이 값을 저장하면 INSERT 구문을 만들어준다.
		* 객체지향적인 모델링을 가능하게한다.
		* 쿼리를 직접 작성하지 않아도 되고, 데이터베이스 변경에 따른 차이점은 DIALECT로 쉽게 처리할 수 있다.
		* 캐시와 SQL구문을 재사용할 수 있다.
		
	* https://www.slideshare.net/mobile/zipkyh/ksug2015-jpa3-jpa
	* [JPA N+1 쿼리 문제와 해결 : TOAST Meetup](https://meetup.toast.com/posts/87)
	* 영속성 컨텍스트 
		* 엔터티를 영구 저장하고 관리하는 환경
		* 영속성컨텍스트는 논리적인 개념 엔티티 매니저를 생성할 때 하나 만들어진다.
		* 특징
			* 1차 캐시
			* 동일성 보장
			* 변경 감지
			* 지연 로딩
			* 트랜잭션을 지원하는 쓰기 지연
		* new, transient
			* 영속성 컨텍스트와 관계가 없는 상태
		* merged
			* 영속성 컨텍스트에 저장된 상태
		* detached
			* 영속성  컨텍스트에 저장되었다가 분리된 상태
		* removed
			* 삭제된 상태 
	*  Lazy, Eager 
	* @OneToOne, @ManyToOne
		* 기본 fetch 전략은 Eager
	* @OneToMany, @ManyToMany
		* 기본 fetch 전략은 Lazy
	* n+1 쿼리
		* 하나의 엔터티를 조회할때 연관된 다른 엔터티를 각각 조회하는 문제 
		* Parent 에 Children 에 10만개라면 Parent 쿼리는 1개이나 Children 쿼리는 10만번 발생할것이다..
	* fetch 조인
		* JPQL을 실행할 때는 LAZY Fetch나 EAGER Fetch나 차이가 없다
		* n+1 쿼리가 발생할경우 불필요한 엔터티를 가져오고 성능적으로 이슈가 발생하므로 fetch join 을 통해서 SQL 호출 횟수를 줄여주면 성능향을 꾀할 수 있음
		* fetch join 대상에는 별칭을 줄수 없다
		* 둘이상의 컬렉션을 fetch 할수 없다
		* 페이징 API 를 사용할 수 없다.
	* dialect
		* 서로 다른 DB간의 문법 차이를 말하며 JPA 에서는 dialect 클래스 교체만으로  이 문제를 해결할 수 있다.
		
7. Event bus
	* Pub/Sub 방식으로 Event를 발행 하면 해당 Event를 구독하는 Subscriber에게 전달된다.
	* 단일책임원칙대로 핵심코드와 횡단코드를 분리하여 개발할 수 있다. 

8. DDD
9. Docker
	* 컨테이너를 이용하여 애플리케이션을 신속하게 배포 ,구축할 수 있는 플랫폼
	* 이 컨테이너에는 라이브러리, 코드,런타임같은  실행환경이 저장되어있고 이를 이미지로 관리한다.
10. Kunbernates
	* 분산된 Docker 애플리케이션간의 배포,장애극복,공통기능등을 지원하는 플랫폼
11. Memcached vs Redis
	* 캐시를 쓰는 이유는 정적인 데이터들(select쿼리) in memory 저장소에 저장하기 때문에 빠르게 데이터를 가져올 수 있다.
		속도와 부하분산에 유리
	* Memcached는 최신의 기능보다는 안정성과 최적화에 주점을 두고 개발되고 있다.
	* 노드의 추가,제거가 redis 보다 유리하게 설계되 있다. 
	* Memcached는 값의 용량은 기본 1MB , 키값 250바이트 제한
	* Redis는 Key-value 를 각각 512MB 까지 사용가능 하고 바이너리로 관리된다.
	* 데이터 형식이 Memcached 에 비해 다양하다. 
	* 데이터를 스토리지에 영구적으로 저장할 수 있기때문에 Redis 서버에 장애가 발생해도 메인 디스크에서 데이터를 가져오는 부담을 덜 수 있다.
	* 리플리케이션을 지원한다.
	
13. LRU 란? (Least Recently Used)
	*  가장 최근까지 사용되지 않는 데이터는 앞으로도 사용되지 않을거라는 가정을 기반으로 한 페이징 처리 알고리즘
	
14. REST API
	* 로이 필딩이 발표한 논문에 의해 소개되었다.
	* 전통적 웹개발방식이 웹의 본래 설계의 우수성을 많이 사용하지 못하고 있다고 판단하여  웹의 장점을 최대한 활용할 수 있는 네트워크 기반의 아키텍쳐를 소개하였다.
	* REST 는 크게 리소스, 메서드, 메시지 3가지 요소로 구성된다. 
	* 장점
		* 클라이언트 서버 구조형태로 개발하기 쉽다. 
		* API 메세지만 보고도 이해하기 쉬운 구조로 되어있다.
		* 무상태을 가진다. 세션정보나 쿠키정보를 별도로 저장하지 않기때문에 API서버는 들어오는 요청만을 단순하게 처리하면된다.
	* 단점
		* URI 형식을 엄격하게 맞추는데는 제약이 있다.
		* 엄격한 표준과 강제성이 없다.
	* 리차드슨의 성숙도 모델
		* 레벨 0 : POX
			* 정통적 웹방식이나 SOAP, XML-RPC 
		* 레벨 1 : 자원
			* 명사를 사용하는 URI 로된 여러개의 API를 만든다.
		* 레벨 2 : HTTP동사 
			* 상태 코드나 HTTP 동사의 특성을 이용한 API를 만든다.
		* 레벨 3 : 하이퍼미디어 컨트롤 
			* HATEOAS 
			* 서비스가 제공하는 자원에 접근하기 위해 아무런 지식도 필요하지 않는 수준의 API를 만든다.
		
15. JWT vs OAuth vs Session
	* JWT의 구성
		* header : 토큰의 타입과 암호화 방식에 대한 기술
		* payload : 클라이언트에서 사용할 JSON data
		* signature : 토큰의 서명, 토큰검증시 필요함 
	* OAuth
		* 인증을 위한 표준중하나 인증완료 후 서버로부터 Access Token을 발급받고, 이 Access Token이 만료되기전까지 클라이언트의 인증을 식별하기 위한 토큰으로 사용한다. 
	* [spring security oauth2 jwt – 머루의개발블로그](http://wonwoo.ml/index.php/post/980)
	* [Using JWT with Spring Security OAuth | Baeldung](https://www.baeldung.com/spring-security-oauth-jwt)
	* [JWT JSON Web Token 소개 및 구조 | VELOPERT.LOG](https://velopert.com/2389)

16. CI 
	* Jenkins
	* Travis CI

17. 자바스크립트
	* 클로저 와 프로토 타입
	* 커링 
	* MVVM MVP MVC 차이
	
18. HTTP 1 vs HTTP 2
	* HTTP 2 는 HTTP 1 과 완전히 다른 프로토콜이 아니라 HTTP 1 의 성능을 향상하는데 중점을 두었다.
	* HTTP 1 단점
		* Round-Trip
			* 매 요청마다 하나의 커넥션을 만들고 3-ways Handshake 가 일어나므로 불필요한 네트워크 지연을 초래한다.
		* 무거운 Header 구조
			* 불필요 Header 가 많아도 너무 많다.
	* HTTP 2 장점
		* Multiplexed Streams
			* 한 커넥션으로 동시에 여러개의 메세지를 주고 받을수있다.
		* Server Push
			* 클라이언트의 요청없이 서버가 클라이언트에 리소스를 전송할 수 있다.  이를 PUSH_PROMISE 라고 한다.
		* Header Compression
			* HPACK 압축방식으로 헤더를 압축할 수 있다.
			
19. CORS (Cross Origin Request Sharing)
	* Javasript는 SOP라는 동일 출처 정책이 있다. 같은 도메인끼리의 자바스크립트만 공유가 가능하다.
	* 근래는 서버와 프론트를 별도로 개발하기 때문에 이경우 SOP정책에 위배되어 문제가 발생할수있다. 
	* W3C에서는 서로 다른 도메인끼리의 자바스크립트 자원도 사용할 수 있도록 표준안을 만들었다. 이게 CORS
	* Preflight
		* CORS를 위해 웹브라우저가 AJAX 로 서버를 호출할경우 OPTIONS 헤더로 우선 서버의 cross domain 룰을 확인
		* access-control-allow-origin, access-control-allow-methods 등.
		* 클라이언트에서는 해당 헤더 값을 확인하고 post,put,delete 등을 호출
	
20. Vert.x
	* 폴리글랏 프로그래밍을 지원한다. 
	* 이벤트 드리븐으로 개발할수있다.
	* 하나의 Verticle은 마치 싱글스레드를 사용하는것처럼 동기화 처리를 신경쓰지 않고 개발할 수 있다. 
	* Netty 를 내장하고 있으므로, 좀더 저수준의 NIO API를 사용할 수 있다.
	* Node.js 보다 echo system이 발전되지 못한상태이다.
	
21. 비동기 프로그래밍 장단점
	* 하나의요청이 처리되전에 제어권을 다음요청으로 넘긴다. 그리고 요청이 처리되면 콜백형태로 결과를 받는다.
	* 빠른 응답성, 블록킹을 기다리지 않기때문에 cpu에 계속 일을 시킬 수 가 있다.
	* 코드의 흐름을 이해하기 어렵고, 디버깅이 어렵다. 
	* 콜백헬의 문제가 있다. 
	* 참고자료 : http://www.nextree.co.kr/p7292/

22. MSA vs Monolithic
	* MSA 장점
		* 애플리케이션이 분산되어 있기때문에 장애극복,확장,유지보수,협업에 유리하다.
	* MSA 단점
		* 분산 애플리케이션간의 트랜잭션 처리가 어렵고, 너무 잘게 쪼개지면 오히려 서비스를 파악하는데 어려움이 있다. 
	* Monolithic 장점
		* 하나의 애플리케이션안에 여러 서비스가 존재하기때문에 개발자가 히스토리를 알기쉽고, 배포, 테스트가 간편한 장점이있다.
	* Monolithic 단점
		* 크기가 커서 빌드 및 배포 시간, 서버기동이 오래걸리며 하나의 모듈에서 장애가 나면 애플리케이션 전체에 영향을 줄 수 있다.	
		
23. SOLID 
	* 단일 책임원칙
		* 한개의 객체는 한개의 책임만 가져야 한다는 원칙
	* 개방폐쇄원칙
		* 확장에는 열려있고 기능 수정에는 닫혀 있어야 한다는 원칙
	* 리스코프 치환 원칙
		* 다형성을 기반으로 부모클래스를 하위클래스로 치환해도 부모클래스를 사용하는 코드는 정상적으로 동작하여야한다. 
	* 인터페이스 분리 원칙
		* 인터페이스는 자신과 관련있는 기능만 구현을 강제해야한다.
	* 의존관계역전 원칙
		* 저수준 구현체와 고수준 구현체의 의존관계를 역전시키는 원칙
		* DI도 일종의 의존관계역전 원칙의 구현체 

24. 언제 RDB 를 사용하고 언제 NoSQL 을 사용하는가?
	* RDB : SQL 을 통해 질의한다. ANSI SQL 이기 때문에 MySQL -> Oracle 로 넘어가더라도 
		마이그레이션과 학습이 용이하고 구조화된 데이터에 최적화 되어있고, 데이터간의 관계를 정의할 수 있기 때문에 
		조인이 용이함 (정규화되고 한정된 규모의 데이터처리에 용이함)
	* NoSQL : 데이터 간의 관계를 정의하지 않기 때문에 일반적으로 테이블간 조인이 불가능함. 
		RDB 에 비해 훨씬 대용량의 데이터를 저장할 수 있고, 분산 구조이기 때문 (scaleout) 분산시 장애가 발생하여도
		데이터 유실이나 서비스 중지가 없는 형태의 구조이다. RDB는 AnsiSQL등의 표준화 작업이 있으나,
		NoSQL은 이런 표준화 인터페이스가 거의 없음 (단순 대규모 데이터 처리에 용이함)
	* CAP 이론
		* Consistency (일관성) : 분산된 노드중 어느 노드로 접근하더라도 데이터가 동일해야한다.
		* Availability (가용성) : 클러스터링된 노드 중 하나 이상의 노드가 실패하더라도 정상적으로 요청을 처리해야한다.
		* Partition Tolerance (분산 허용) : 클러스터링 노드 간에 네트워크 장애가 나더라도 서비스를 수행할 수 있어야한다.
    
25. 암호화 공개키,대칭키 차이
	* 대칭키 방식 : 암호화와 복호화시 똑같은 비밀번호를 사용함. 계산이 빠르나 해킹에 약함.	
	* 공개키 방식: 메시지를 임의로 만들어진 비밀 키를 이용해 암호화한 다음 
		이 비밀 키를 다시 수신자의 공개 키로 암호화하여 메시지와 함께 전송하는 것이다
		
26. 블로킹 I/O, 넌-블로킹 I/O 차이점 (https://alwayspr.tistory.com/44)
	* Block I/O : IO system call을 하였을때, 해당 스레드는 read response 가 올때까지 무한정 대기한다.
	              일반적인 웹 어플리케이션은 멀티 스레딩 구조라 단점을 커버하지만, 스레드 생성/전환시 발생하는 context-switching 은
		      비용이 발생하므로 비효율적임
	* synchronous Non-Block I/O : polling 방식으로 특정 시간 간격으로 데이터의 수신이 완료되었는지를 체크한다.
			 	      주기적으로 호출하기때문에 불피요한 자원이 발생한다.
	* asynchronous Non-Block : IO system call을 하고 그 즉시 스레드가 리턴된다. 이후 read response가 완료되면 
				   이벤트를 발생시켜 미리 등록된 콜백을 통해 작업을 수행하므로 효율적이다.
				   
27. Reactive 프로그래밍
 	* Spring WebFlux 장점 
		* 모든 웹 요청 처리 작업을 명시적인 코드로 작성
			* 메서드 시그니처 관례와 타입 체크가 불가능한 애노테이션에 의존하는 @MVC 스타일 보다 명확
			* 정확한 타입 체크 가능 
		* 함수 조합을 통한 편리한 구성, 추상화에 유리
		* 테스트 작성의 편리함
			* 핸들러 로직은 물론이고 요청 매핑과 리턴 값 처리까지 단위테스트로 작성 가능
 	* Spring WebFlux 단점
		* 함수형 스타일의 코드 작성이 편하지 않으면 코드 작성과 이해 모두 어려움
		* 익숙한 방식으로도 가능한데 뭐하러

28. 낙관적 동시성과 비관적 동시성 (https://docs.microsoft.com/ko-kr/dotnet/framework/data/adonet/optimistic-concurrency)
	* 낙관적 동시성 (Optimistic concurrency)
		* 낙관적 락은 특정 행을 읽을 때 행을 잠그지 않는다.
		* 이때문에 사용자가 행을 업데이트할 때 행을 읽은 후 다른 사용자가 해당 행을 변경했는지 여부를 응용프로그램에서 확인해야함
		* 주로 데이터 경합이 낮은 환경에서 사용된다. 
		* 행을 잠그려면 리소스가 추가로 소요되기때문에, 해을 잠글 필요가 없어 성능이 향상된다.
		* 또한, 잠금을 유지하기위해 데이터베이스 서버와 연결이 유지될 필요가 없으므로 더 많은 클라이언트가 접속할 수 있다.
	* 비관적 동시성 (Pessimistic concurrency)
		* 데이터 소스의 행을 잠가 다른 사용자가 데이터를 수정하더라도 현재 사용자에게 영향을 미치지 못하게 한다.
		* 락을 걸면 락이 풀릴때까지 다른 사용자는 접근할 수 없다. 
		* 트랜잭션 롤백에 필요한 비용보다 잠금을 통해 데이터를 보호하는 비용이 적게 들도록 데이터 경합이 높은 환경에서 주로 사용된다.
		* 업데이트 작업을 마치고 락을 해지할 때까지 다른 사용자는 해당 행을 변경할 수 없다.
		* 잠금시간이 짧은 경우에 가장 잘 구현되고, 사용자가 데이터와 상호 작용하여 비교적 오랜 시간 레코드를 잠그는 경우에는 사용하지 않는것이 유리함
		
29. 코틀린
	* with
		* 객체에 대한 참조를 반복해서 언급하지 않으면서 그 객체의 메소드를 호출할 수 있다.
		* with 내부에서 메서드 이름 충돌이 발생하면 this@OuterClass.toString() 같은 방식으로 사용해야 한다.	
	* apply
		* with와 기본적으론 동일하지만 apply는 항상 자신에게 전달된 객체(수신 객체)를 반환한다. 
		* apply를 사용하면 어떤 객체라도 빌더 스타일의 API를 사용해 생성하고 초기화할 수 있다. 
	* 코루틴과 Thread 차이
		* Thread는 하나의 프로세스에 여러개의 Thread가 존재 할 수 있다. 다만, 생성과 전환시 컨텍스트 스위칭이 발생하며 그 만큼의 비용이 필요하다.
		* 코루틴은 즉시 실행하지 않고, Thread와는 다르게 OS의 영향을 받지 않으니 컨텍스트 스위칭이 발생하지 않는다. 
		또한 개발자가 루틴에 대한 생명주기를 지정 할 수 있다. 
		
		
 
		
			
		
				   
	
	
