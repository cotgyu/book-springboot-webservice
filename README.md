[![Build Status](https://travis-ci.org/cotgyu/book-springboot-webservice.svg?branch=master)](https://travis-ci.org/cotgyu/book-springboot-webservice)

소개
----

-	스프링 부트가 권장하는 방식을 사용하면 서버에 톰캣과 같은 웹 어플리케이션 서버를 설치할 필요도 없고 오로지 Jar 하나만 있으면 서비스 운영을 할 수 있다.
	-	거추장스럽던 수많은 설정이 자동화되어 비즈니스 로직에만 집중할 수 있다.

1장
---

-	build.gradle

	-	ext라는 키워드는 build.gradle 에서 사용하는 전역변수를 설정한다는 의미
	-	원격저장소 : 기본적으로 mavenCentral을 많이 사용하지만 업로드 난이도 때문에 jcenter 도 많이 이용함
	-	개인 개발자가 라이브러리 업로드를 쉽게할 수 있음
	-	jcenter에 업로드하면 mavenCentral에 업로드될 수 있도록 자동화를 할 수 있음

-	.gitignore : 플러그인으로 생성가능함 (템플릿도 만들어 놓을 수 있음)

	-	ignore 에 디렉토리를 추가하였음에도, 디렉토리 내 파일이 자꾸 커밋리스트에 뜨는 경우
	-	gitignore 파일을 늦게 생성해서 파일이 트래킹 list에 남아있는 경우임
	-	git ls-files > git rm --cached 파일명

2장
---

-	단위테스트 : 기능 단위의 테스트 코드를 작성하는 것

-	Apllication.java

	-	@SpringBootApplication : 부트의 자동 설정, 스프링 bean 읽기와 생성을 모두 자동으로 설정

		-	@SpringBOotApplication 있는 위치부터 설정을 읽어가기 때문에 항상 프로젝트 최상단에 위치해야함

	-	main 메서드에서 실행하는 SpringApplication.run 으로 인해 내장 WAS를 실행

		-	부트로 만들어진 Jar 파일로 실행 가능
		-	부트에서는 내장 WAS를 사용하는 것을 권장 (같은환경에서 배포가능)

-	@RestController

	-	컨트롤러를 JSON으로 반환하는 컨트롤러로 만들어줌
	-	@ResponseBody를 각 메소드마다 선언했던 것을 한번에 사용할 수 있게 해줌

-	@RunWith(SpringRunner.class)

	-	테스트를 진행할 때 Junit에 내장된 실행자 외에 다른 실행자를 실행시킴
	-	여기서는 SpringRunner라는 스프링 실행자를 사용

-	@WebMvcTest

	-	선언할 경우 @Controller, @ControllerAdvice 등을 사용할 수 있음
	-	단, @Service, @Component, @Repository 등은 사용할 수 없음

-	절대 수동으로 검증하고 테스트 코드를 작성하지 말 것

3장
---

-	JPA를 통해 SQL에 종속적인 개발을 하지 않아도 됨

-	Spring Data JPA

	-	구현체 교체의 용이성
	-	저장소 교체의 용이성

-	@Entity

	-	테이블과 링크될 클래스
	-	기본 값으로 카멜케이스 이름을 언더스코어 네이밍으로 매칭함
	-	왠만하면 PK는 Long타입의 Auto_increment 추천

-	@GeneratedValue

	-	부트 2.0에서는 Generation.Type.IDENTITY 옵션을 추가해야 auto_increment 가 됨

-	생성자 대신에 @Builder 를 통해 제공되는 빌더 클래스 사용

	-	생성자나 빌더나 생성 시점에 값을 채워주는 역할은 똑같음
	-	하지만, 생성자의 경우 채울 필드가 무엇인지 명확히 지정할 수 없음

-	Entity 클래스와 기본 Entity Repository 는 함께 위치해야함

	-	프로젝트 규모가 커저 도메인별로 분리해야한다면 Entity 클래스와 기본 Repository는 같이 관리되어야 하기 때문에 도메인 패키지에서 함꼐 관리한다.

-	Service에서 트랜잭션, 도메인 간 순서보장의 역할을 한다. (비즈니스 로직처리 X)

	-	비즈니스처리를 담당해야할 곳은 도메인!
	-	서비스 메서드는 트랜잭션과 도메인 간의 순서만 보장해준다.

-	Bean을 주입받는 방법

	-	가장 권장하는 방식은 생성자로 주입하는 방식

-	절대로 Entity클래스를 Request/Response 클래스로 사용하지 말 것

	-	Entity 클래스는 데이터베이스와 맞닿은 핵심 클래스임
	-	Entity 클래스 기준으로 테이블이 생성되고, 스키마가 변경됨
	-	위험도가 크니 dto를 통해 분리해서 처리하자

-	@WebMvcTest 의 경우 JPA 기능이 작동하지 않음

	-	Controller와 ControllerAdvice 등 외부 연동과 관련된 부분만 활성화되기 때문
	-	JPA 기능까지 한번에 테스트할때는 @SpringBootTest 와 TestRestTemplate 사용

-	@MappedSupperClass

	-	JPA Entity 들이 상속할 경우 필드들도 컬럼으로 인식하도록함

4장
---

-	템플릿 엔진 : 지정된 템플릿 양식과 데이터가 합쳐져 HTML 문서를 출력하는 소프트웨어

-	머스테치 : 수 많은 언어를 지원하는 가장 심플한 템플릿 엔진

-	머스테치 스타터 를 추가함으로써 컨트롤러에서 문자열을 반환할때 앞의 경로와 확장자는 자동으로 지정됨.

-	페이지 로딩속도를 높이기 위해 css는 header에 js는 footer 에 위치

	-	HTML은 위에서부터 코드가 실행되기 때문에 head가 다 실행되고서 body 실행

-	index.js 에 var main = {...} 으로 선언하는 이유

	-	브라우저의 스코프는 공용공간으로 쓰이기 때문에 나중에 로딩된 js의 함수가 덮어씌워질 수 있다.
	-	여러 사람이 참여하는 프로젝트에서는 중복된 함수 이름은 자주 발생할 수 있음

-	부트는 기본적으로 src/main/resources/static 에 위차한 자바스크립트, CSS, 이미지 등 정작 파일들은 URL에서 /로 설정됨

-	머치테치 문법

	-	{{#posts}} : posts라는 List를 순회함
	-	{{id}} : List에서 뽑아낸 겍체의 필드를 사용

-	@Transactional(readOnly = true)

	-	트랜잭션 범위는 유지하되 조회기능만 남겨두어 조회속도가 개선됨. 등록, 수정,삭제 기능이 없는 서비스 메스드에서 사용하는 것을 추천

5장
---

-	인터셉터, 필터 기반의 보안기능을 구현하는 것보다 스프링 시큐리티를 통해 구현하는 것을 권장

-	OAuth2 연동방법

	-	1.5 에서는 url을 모두 명시해야하지만 2.0 에서는 client 인증방식만 입력하면 됨
	-	2.0 으로 오면서 enum으로 대체 (구글, 깃허브, 옥타)
	-	네이버, 카카오는 직접추가해야 함

-	@Enumerated(EnumType.STRING)

	-	JPA로 데이터베이스에 저장할 때 Enum 값을 어떤 형태로 저장할지 결정
	-	숫자로 저장되면 데이터베이스로 확인할 때 그 값이 무슨 코드를 의미하는 지 알 수 없어서 String 설정

-	SecurityConfig

	-	userInfoEndPoint : OAuth2 로그인 성공 후 사용자 정보를 가져올 설정

-	엔티티에는 직렬화를 하지말 것

	-	다른 엔티티와의 관계가 형성될 경우 성능 이슈, 부수효과가 발생할 수 있으므로 직렬화 기능을 가진 dto를 추가하여 사용할 것

-	어노테이션 생성

	-	@Target(ElementType.PARAMETER)

		-	어노테이션이 생성될 수 있는 위치 지정
		-	PARAMETER 이니 메소드의 파라미터로 선언된 객체에서만 사용할 수 있음

	-	@interface

		-	이 파일을 어노테이션 클래스로 지정함

	-	HandlerMethodArgumentResolver

		-	supportsParameter()

			-	컨트롤러 메서드의 특정 파라미터를 지원하는지 판단함

		-	resolveArgument()

			-	파라미터에 전달할 객체를 생성

-	현업에서의 세션저장소 사용 방법

	-	톰캣 세션

		-	별다른 설정을 하지 않았을 때 기본

		-	2대 이상의 WAS의 경우 추가적인 설정이 필요함

	-	MySQL 같은 데이터베이스를 세션 저장소로 사용

		-	여러 WAS 간 공용세션을 사용할 수 있는 가장 쉬운 방법

		-	로그인 요청마다 DB IO가 발생하여 성능상 이슈가 발생할 수 있음

	-	Redis, Memcached 와 같은 메모리 DB를 세션 저장소로 사용한다.

		-	B2C 서비스에서 가장 많이 사용하는 방식

		-	실제 서비스로 하기위해서는 Embedded Redis와 같은 방식이 아닌 외부 메모리 서버가 필요함

-	security 테스트

	-	@WithMockUser(roles="USER")
	-	인증된 모의 사용자를 만들어서 사용함
	-	roles에 권한을 추가할 수 있음
	-	이 어노테이션으로 ROLE_USER 권한을 가진 사용자가 API를 요청하는 것과 동일한 효과를 가짐

	-	216p 부터 목 테스트 설명임

6장
---

-	AMI (Amazon Machine Image)

	-	EC2 인스턴스를 시작하는 데 필요한 정보를 이미지로 만들어 둔 것

-	아마존 리눅스를 사용하는 이유

	-	aws의 각종 서비스와의 상성이 좋음
	-	amazon 독자적인 개발 리포지토리를 사용하고 있어 yum이 매우 빠름  

7장
---

-	aws에서는 모니터링, 알람, 백업, HA 구성 등을 지원하는 관리형 서비스인 RDS 를 제공함

-	RDS AWS에서 지원하는 클라우드 기반 관계형 데이터베이스

	-	하드웨어 프로비저닝, 데이터베이스 설정, 패치 및 백업과 같이 잦은 운영 작업을 자동화하여 개발자가 개발에 집중할 수 있게 지원하는 서비스
	-	조정가능한 용량 지원

-	RDS - DB엔진 - MariaDB 추천

	-	가격
	-	Amazon Aurora 교체 용이성

		-	Amazon Aurora는 AWS에서 MySQL과 PostreSQL을 클라우드 기반에 맞게 재구성한 데이터베이스
		-	성능 좋음

	-	MySQL 대비 장점

	-	동일 하드웨어 사양으로 MySQL보다 향상된 성능

	-	다양한 기능

	-	다양한 스토리지 엔젠

8, 9장
------

-	CI: VCS 시스템에 PUSH가 되면 자동으로 테스트와 빌드가 수행되어 안정적인 배포 파일을 만드는 과정
-	CD: 이 빌드 결과를 자동으로 운영서버에 뭊우단 배포까지 진행되는 과정

-	CI의 4가지 규칙

	-	모든 소스 코드가 살아있고 누구든 현재의 소스에 접근할 수 있는 단일 지점을 유지 할 것
	-	빌드 프로세스를 자동화해서 누구든 소스로부터 시스템을 빌드하는 단일 명령어를 사용할 수 있게 할 것
	-	테스팅을 자동화해서 단일 명령어로 언제든지 시스템에 대한 건전한 테스트 수트를 실행할 수 있게 할 것
	-	누구나 현재 실팽 파일을 얻으면 지금까지 가장 완전한 실행 파일을 얻었다는 확신을 하게 할 것

-	AWAS 서비스에 외부서비스가 접근하려면 접근가능한 권한을 가진 Key을 생성해서 사용해야함

	-	IAM (인증 관련 기능)
	-	IAM을 통해 Travis CI가 AWS의 S3와 CodeDeploy에 접근할 수 있도록 가능

-	AWS S3

	-	일종의 파일서버
	-	파일을 저장하고 접근권한을 관리, 검색 등을 지원하는 파일 서버의 역할을 함

-	CodeDeploy

	-	AWS의 배포 기능

		-	Code Commit

			-	깃허브와 같은 코드 저장소 역할

	-	Code Build

		-	빌드용 서비스 (CI)
		-	멀티 모듈을 배포해야하는 경우 사용해 볼만하지만, 규모가 있는 서비스에서는 대부분 젠킨스나 팀시티 등을 이용한다고함

	-	Code Deploy

		-	AWS 배포서비스
		-	CodeDeploy는 대체재가 없음
		-	오토 스케일링 그룹 배포, 블루 그린 배포, 롤링 배포, EC2 단독 배포 등 많은 기능을 지원함
		-	로그파일위치

			-	/opt/codedeploy-agent/deployment-root

10장
----

-	무중단 배포 방식

	-	AWS에서 불루 그린 무중단 배포
	-	도커를 이용한 웹서비스 무중단 배포

-	책에선 엔진엑스를 이용한 무중단 배포 사용

	-	엔진엑스는 웹서버, 리버스 프록시, 캐싱, 로드 밸런싱, 미디어 스트리밍 등을 위한 오픈소스 소프트웨어

-	리버시 프록시 : 엔진엑스가 외부의 요청을 받아 백엔드 서버로 요청을 전달
