
# 스프링


## 스프링 클라우드
- MSA : 기존 모놀리식 서비스 방식에서 비즈니스 로직을 독립적인 프로젝트로 분리하여 개발하고 배포, 가장 앞단에서 API Gateway를 통해 요청을 분산하여 관리하는 구조
- 모놀리식 : 하나의 프로젝트에 모든 비즈니스 로직과 설정 데이터들을 넣어 개발하고 배포하는 방식

### 구성요소
- Spring Cloud Gateway
- Spring Cloud Eureka Server (모니터링 서버 역할)
- Spring Cloud Eureka Client
- Spring Config Server (application.properties 역할)
- Config Repository
- Spring Config Client

### Config Repository 깃허브 설정 방법
1. 깃허브 레포지토리 생성 
2. 설정 파일 생성 (설정 파일명 : 이름-환경.properties / 이름-환경.yml / ex. hongsok-dev.properties)
3. 리포지토리 외부 접속을 위한 비대칭 키 생성
```bash
ssh-keygen -m PEM -t rsa -b 4096 -C "코멘트(계정명 넣어도 됨)"
```
4. 생성된 비대칭 키 중 public 키 내용을 복사하여 깃허브 레포지토리에 등록
```bash
cd ~/.ssh
vi id_rsa.pub
```
깃허브 레포지토리 - Settings - Depoly keys - Add Depoly key

### Config Server


### Config Client 


### Eureka Server


### Eureka Client


### Spring Cloud Gateway





## 스프링 시큐리티
- Spring Security는 '인증'과 '권한'에 대한 부분을 Filter 흐름에 따라 처리하도록 도와주는 하위 프레임워크
> [참고자료](https://velog.io/@sago_mungcci/Spring-Security%EB%9E%80)



## 객체 지향 설계 원칙

- 단일 책임 원칙 (Single responsibility principle) : 한 클래스는 하나의 책임만 가져야 한다.

- 개방-폐쇄 원칙 (Open/closed principle) : 소프트웨어 요소는 확장에는 열려 있으나 변경에는 닫혀 있어야 한다.

- 리스코프 치환 원칙 (Liskov substitution principle) : 객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야 한다.

- 인터페이스 분리 원칙 (Interface segregation principle) : 특정 클라이언트를 위한 인터페이스 여러 개가 범용 인터페이스 하나보다 낫다.

- 의존관계 역전 원칙 (Dependency inversion principle) : 프로그래머는 추상화에 의존해야지, 구체화에 의존하면 안된다. (의존성 주입)



## 서블릿 내장객체
> [참고자료](https://dinfree.com/lecture/backend/javaweb_2.3.html)

|내장객체|타입|주요 제공 기능|
|------|---|---|
|out|javax.servlet.jsp.JspWriter|클라이언트(웹브라우저)로 출력 전달|
|request|javax.servlet.http.HttpServletRequest|HTTP 요청 처리|
|response|javax.servlet.http.HttpServletResponse|HTTP 응답 처리|
|config|javax.servlet.ServletConfig|서블릿 관련 정보 처리|
|application|javax.servlet.ServletContext|웹애플리케이션 정보 처리|
|session|javax.servlet.http.HttpSession|세션 정보 처리|
|pageContext|javax.servlet.jsp.PageContext|다른 내장 객체의 참조나 forward에 사용|
|page|javax.servlet.jsp.PageContext|현재 JSP 문서 정보 처리, 현재 페이지의 서블릿 객체를 가리킴|
|exception|java.lang.Throwable|예외 처리|


## 스프링 코딩 규칙

### 페이지 요청 처리는 @Controller에서 처리
```java
@Controller
public class SampleController {
  ...

  @RequestMapping("/sample/blank")
  public ModelAndView blank(ModelAndView mav) {
    log.trace("[PAGE]/sample/blank");
    mav.setViewName("blank");
    return mav;
  }

  @RequestMapping("/sample/500")
  public ModelAndView p500(ModelAndView mav) throws Exception {
    throw new Exception();
  }
}
```

### Rest 요청 처리(Ajax 요청)는 @RestController에서 처리
```java
@RestController
public class SampleRestController {
  ...

  @RequestMapping(value = "/sample/properties", method = RequestMethod.GET)
  public List<AbleProperties> get(AbleProperties properties) {
  	return sampleService.find();
  }

  @RequestMapping(value = "/sample/properties", method = RequestMethod.POST)
  public ResponseEntity<AbleRestResponse> post(@RequestBody AbleProperties properties) {
  	log.trace("[POST]/sample/properties - {}", properties);
  	return sampleService.insert(properties);
  }

  @RequestMapping(value = "/sample/properties", method = RequestMethod.PUT)
  public ResponseEntity<AbleRestResponse> put(@RequestBody AbleProperties properties) {
  	log.trace("[PUT]/sample/properties - {}", properties);
  	return sampleService.update(properties);
  }

  @RequestMapping(value = "/sample/properties", method = RequestMethod.DELETE)
  public ResponseEntity<AbleRestResponse> delete(AbleProperties properties) {
  	log.trace("[DELETE]/sample/properties - {}", properties);
  	return sampleService.delete(properties);
  }
}
```

### @Mapper는 ibatis에서 가져올 것
```java
import org.apache.ibatis.annotations.Mapper;
```

### @RequestMapping 사용(@GetMapping, @PostMapping 금지)
```java
@RequestMapping("/sample/blank")
```

### 의존성 주입은 생성자 주입 방식 사용(@Autowired 금지)
```java
@Service
public class SampleServiceImpl implements SampleService {
  private SqlSession sqlSession;
  private CrudMapper mapper;

  public SampleServiceImpl(SqlSession sqlSession) {
  	this.mapper = sqlSession.getMapper(CrudMapper.class);
  	this.sqlSession = sqlSession;
  }

  ...
}
```

### ModelAndView만 사용 (Model, ModelMap 금지)
```java
@Controller
public class SampleController {
	...

	@RequestMapping("/sample/blank")
	public ModelAndView blank(ModelAndView mav) {
		log.trace("[PAGE]/sample/blank");
		mav.setViewName("blank");
		return mav;
	}

	@RequestMapping("/sample/500")
	public ModelAndView p500(ModelAndView mav) throws Exception {
		throw new Exception();
	}
}
```

### LocalDateTime만 사용 (Date, Calendar 금지)
```java
@Data
@Table(name = "TB_ABLE_PROPERTIES")
public class AbleProperties {
	...
	@Column(name = "PROPERTY_DT")
	private LocalDateTime propertyDt;
}
```

### 참고자료
- [RequestMapping](https://itworldyo.tistory.com/32)
- [Autowired](https://madplay.github.io/post/why-constructor-injection-is-better-than-field-injection)
- [ModelMap](https://javaoop.tistory.com/56)
- [LocalDateTime](https://java119.tistory.com/52)

