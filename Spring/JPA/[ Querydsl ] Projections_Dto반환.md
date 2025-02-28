/*
 * 
 * [ entity , dto 변환 ]
 * 1.  Projections(QueryDSL)이 성능이 더 좋음, 변환 라이브러리(reflection사용라이브러리 또는 maptruct)나 stream으로 직접 변환하는것보다 
 * 		(new MessageLayoutDto(...)처럼 생성자를 직접 호출하는 원리임)
 * 		- Dto에 변환메서드 작성하지 않으려는 이유
 * 		  - Dto에 변환메서드 작성하는건 querydsl과 dto의 강한결합, 의존성이 높아짐
 * 		  - 데이터 전송과 변환로직 단일책임 위배
 * 		  - Tuple이 index기반이라 querydsl작성순서 바뀌면 변환메서드도 변경해야함
 *      - Projections은 필드명으로 자동으로 매핑가능 setter를 안쓰는 옵션도 가
 * 
 * 2.  Projections.constructor() + @AllArgsConstructor
 * 		(전체필드를 초기화하는 constructor 생성 :: 런타임 NPE 가능성 )
 * 
 * 3.  Projections.constructor() + @RequiredArgsConstructor  : 필수생성자만(final,not null필드) 포함하는 생성자 생성 
 * 
 * 4.  Projections.fields() or Projections.bean() 와 @Builder
 * 		(DTO의 기본 생성자 이용)
 * 		(전체필드,필수값필드가아닌 선택적 필드로 생성자 생성할때)
 * 		(성능은 fields()가 약간더 빠르다고함)
 * 
 * 5.  fields() vs bean()
 * 		bean() : 기본 생성자를 호출, Setter로 필드값 설정 (mutable해야함, final안됨)
 * 		fields() : 필드명매핑 (immutable DTO 가능)
 * 			(DTO의 필드명 기준 기본 생성자 호출, Reflection,직접 값 주입)
 * 			(Setter없어도되고 final이어도 동작함, 필드명 일치해야함) 
 * 
 * 6.  성능비교 constructor() > fields() > bean()
 * 		constructor() : QueryDSL이 Reflection없이 dto생성자 직접호출 가장 빠름, immutable dto가능
 * 		fields() : Reflection 직접값 주입 Setter를 탐색하지 않아서 bean()보다 빠름 ,immutable dto가능
 * 		bean() : Reflection, Setter를 찾고 호출, 메서드호출로 인한 오버헤드 발생 가능성 ,immutable dto 불가능
 * 
 * 7.  Projections은 SELECT 결과를 특정 DTO로 매핑하는 기능을 제공하는 querydsl의 유틸리티 클래스
 * 		constructor() , fields() , list() , map() , tuple() , bean() 등 을지원
 *
 * 8.  참고
 * 	    ExpressionUtils.as() - 필드명을 변경하여 매핑 빠름 
 * 		Transformers.aliasToBean() - Hibernate ResultTransformer 사용 중간빠름
 * 		(deprecated)QueryResults<T>	페이징과 함께 사용 가능 : 
 *		ProjectionSerializer - JSON 변환 지원
 * */
