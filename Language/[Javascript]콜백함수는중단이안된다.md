### [Javascript]
forEach,any,every등의 콜백함수는 루프중단이 안되기 때문에 async await 도 안될뿐더러<br>
콜백함수이기 때문에 루프중단이 안되어 불필요한 연산을 제외하는 등의<br>
연산최적화는 어렵다.<br>
- => 그렇다면 for문을 사용해서 루프중단 조건을 걸것
----
- javascript나 java나 python에서 itorable한 객체의 연산을 <br>
어떻게 최적화 하고있는지 3가지 언어의 공통점과 차이점을 궁금해하는 관점으로 공부해봐도 좋을 것 같다<br>

- 관심키워드는 지연연산

