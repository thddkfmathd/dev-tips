
1. 세션 발급 과정 
- anySign4PC 보안프로그램 사용중 -> 브라우저로 로그인해야함 -> 코드레벨에서 브라우저에서 로그인하는것과 같은 흐름으로 구현안됨
  - 해결방법이 무엇이었나.
  - 302 request 역할, 구성 (로그인 request)
- postman, http 구성요소
  - origin, host, referer 중요성(보안상 요청 정당성 확인 헤더 요소) ->  403 forbidden
  - request body : x-www-form-urlencoded / form-data
- anySign4PC과 같은 보안프로그램
  - 분석 내용
- 코드작성중 추가내용 http 관련
  - org.springframework.web.client.RestTemplate,  org.springframework.http.*
  - RestTemplate 재사용가능, 재사용하지 않아야하는 요소는
  - RestTemplate -> RestClient (v?)
    - reference : https://poalim.tistory.com/59
    - httpClient,RestTemplate,RestClient,WebClient,HTTP Interface
    - reference : https://docs.spring.io/spring-framework/reference/integration/rest-clients.html#rest-http-interface
    - MultiValueMap 사용이유 (formdata)(공식적인 방식?)
    - Form 전송 포맷 매핑할 수 있는 구조(내부적으로 Map<String, List<String>> 구조) 
<br> 

2.  active mq(JMS) + Artemis
 - 기존 : 저장된 로그파일, 로그파일 읽어와서 INSERT
 - 변경 : 큐에 로그를 쌓고, 해당파일을 COMSUMER로 읽어와서 INSERT
   - 파일로 관리시 성능저하 예상, target server에서 prosumer,borker로 큐에 로그저장
   - target server에 접근하지않아도 comsumer로 로그읽어올수 있음
   - 소모성 데이터
   - 단점 : 해당 라이브러리 jar 반입해야함, 추가 개발 및 기간 필요, 데이터 소실 가능성

3. Mq 정리
4. Mq 종류별 장단점 및 각각의 필요한 환경 :  activemq, rabbitmq, kafka, redis
- rabbitmq 레퍼런스 참고
- activemq + artemis
- kafka + zookeeper
5. html docs 를 java로 parsing
6. svn
7. 오프라인 스프링 빌드 (maven) 설정
8. 오프라인 posgres 구축
9. 파일 반입 및 환경세팅 , 필요문서 (노트 참고)
- ssh 서버접속이후 TMOUT=0 설정
10. 
