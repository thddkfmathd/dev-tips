- JsonNode란?
- JsonNode의 특징 - 불변클래스
  - JsonNode는 읽기 전용 API만 제공 
- JsonNode의 장단점
- JsonNode의 필요성
- JsonNode의 적용 환경
- 최상위 객체 JsonNode - 하위객체 : ObjectNode (객체형 mutable),  ArrayNode  (배열형 mutable) ,MissingNode , ValueNode (텍스트, 숫자, 불리언 등)
- JsonNode메서드
  - get vs path
  - get (엄격 npe 가능성)
    - return값 JsonNode 또는 없을때 null ->  null체크하고 asText() 하는로직으로 구성
    - 뽑아서 체크 :JsonNode dataLengthNode = item.get(Layout.dataLength.name());
    -  if ( dataLengthNode != null && dataLengthNode.isInt() )  ,,,, if( dataValueNode != null && dataValueNode.isTextual() )
  - path : (npe 없음)
    - return값 JsonNode (없을때 MissingNode, NPE발생 안함을 보장 )(MissingNode.asText() == "")
  - asText(null),asText("") 로 기본값
  - 
- JsonNode 불변객체 이유 / 그럼에도 원본을 변경해야한다면?
  - ObjectNode ,ArrayNode 사용 put,set?
  - get이나 path로 뽑은 JsonNode를 다운캐스팅 (ObjectNode ...) 해서 변경 -> 원본 객체 JsonNode에 변경 적용
  - 모든트리?를 JsonNode타입으로 받을수있으나 (최상위) instanseof ObjectNode 와 같이 ObjectNode로 만들어진 객체인지 타입체크이후 다운캐스팅해야함
  - ObjectNode로 강제 캐스팅할때 ArrayNode와 같은 다른 JsonNode 하위 객체로 만들어진 거면 ClassCastException 남
- JsonNode deepCopy 어디까지 deepCopy? depth가 깊다면?
  
```JAVA
asisChildren.forEach(asisChild -> {
	   ObjectNode asisChildObjectNode = (ObjectNode)asisChild
    // asisChildObjectNode 값 변경코드
    // asisChildren 에도 변경 반영 ( asisChild는 asisChildren 내부의 객체 참조값을 공유 ) 
});

```
- 위코드의 forEach 루프 내부에서는 asisChildren에 변경이 반영
- stream은?
  - Stream API는 불변성(immutability)과 side-effect-free 처리를 지향
  - 원본의 불변을 보장하고 새로운 객체를 리턴하는게 기본철학 (순수 함수형 패턴'을 지향 -> 입력만 보고 출력 결정, 같은걸넣으면 같은 값이 나와야함)
  - 수정은가능하나 권장하지않음
```JAVA
// List<JsonNode>로 변환해서 .stream() 쓰는 방법도 있음
StreamSupport.stream(asisChildren.spliterator(), false).forEach(node -> {
    if (node instanceof ObjectNode) {
        ((ObjectNode) node).put("check", "Y");
    }
});
```
- StreamSupport.stream
  -  Iterable한 자료구조에서 Stream을 만듬
  -  .iterator() 처럼 순회가능한 ArrayNode의 Spliterator제공
  -  Spliterator를 StreamSupport.stream(...) 로 감싸면 Stream Api 사용가능

- 비교
순회하면서 수정하거나 검사만 할 때
→ StreamSupport.stream(...).forEach(...)
→ 가장 가볍고 빠름.

필터링하거나 특정 조건 추출 후 다른 스트림 작업 계속해야 할 때
→ List<JsonNode> list = StreamSupport.stream(...).collect(...) 후 .stream()
→ 가독성과 편의성 ↑, 단 중간 복사 비용 존재. 스트림 API 유연하게 사용 가능

