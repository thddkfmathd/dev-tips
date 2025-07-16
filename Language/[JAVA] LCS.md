
#### 1. LCS 기반 비교 로직 적용

 <br>
 
1\) 레퍼런스  BeyoundCompare

2\) 목적 

- 단순 루프 비교가 아닌 순서를 고려한 정렬 및 비교 정확도 향상

2\) 적용

- pk asc 정렬로 조회한 데이터 리스트 두개를 msg_id의 값을 기준으로 비교
- LCS DP 테이블
- 비교결과 데이터를 리스트로 만들어 반환 (누락요소도 파악되야함)

  #예시
  
| msgId_a| msgId_b| 
| --------- | - |
| A | A |
|  B    |  |
|   C   |  | 
| D | D |
|E | E|
|F|  |
|| G |
  




- pk순서를 전제, msg_id 기준으로 두 리스트를 Longest Common Subsequence(LCS) 기반으로 정렬
   - 성능개선 
     - msg_id 일치 여부로 lcs 테이블 만드는 loop / lcs 테이블 기반 비교결과 리스트 생성 loop
     - 단순 loop 비교보다 순서보존 및 비교 매칭 정확도 개선
     -    
