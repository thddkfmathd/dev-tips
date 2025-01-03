
######  적용스펙관련해서 문서 작성 해야함 25.1 까지

###### 적용 스펙

java 21 (가상스레드 적용예정이라 버전다운안됨)

jpa (mybatis -> ORM 적용 ) (쿼리 최적화가 어려웠음.. java aplication단에서 최적화할수 있게될것 같음)

querydsl (동적쿼리생성) (기존jpa와 비교해서 장단점 및 선택이유를 좀더 명확하게 정리하는것이 필요함)

lommbok (개발 생산성 증가)

mapstruct (entity <-> dto 변환) 

(코드로 변환메서드를 작성하는것보다 자동매핑 라이브러리를 사용하는게 가독성,생산성,오류방지 등의 이유에서 좋다고 판단)

(dto로 변환해야하는 이유, 현시점 적용이유 정리 필요함)




##### Issue 

1. lommbok, querydsl, mapstruct -> build.gradle 설정 문제로 build 안됨.
   - 세개의 라이브러리 모두 anotationProcessor를 통해 빌드과정에서 코드를 자동생성해야하는 라이브러리임
   - anotationProcessor로 만들어지는 코드의 위치가 셋다 다름
   - lommbok은 기존 경로인거같고 querydsl은 java/main/generated, mapsturct는 java/main/build 경로로 코드생성하는 것이 권장방법인 듯
2.   mapsttruct와 lommbok과 같은걸 사용하려면 ide(eclipse)내에서AnnotationProcessing 설정 Factory Path 에서 Add External JARs에 라이브러리를 직접 지정... 



task지정을 각각해줘야하나?, anotationProcessor관련설정이 호환이안되는것같은데 각각 task를 만들어야하나?
apt 라이브러리가 뭐길래 anotationProcessor에 해당라이브러리를 연결?하는지, plugins에 추가하는 라이브러리 왜 apt인지

plugins ?


  









reference :

[필수참고:eclipse_sts_mapstruct적용과정_블로그](https://jong-bae.tistory.com/8)
    
[]()

[]()
    

2. 
