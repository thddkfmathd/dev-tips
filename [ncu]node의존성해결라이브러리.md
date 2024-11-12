```bash
npm-check-update
```
- 해당라이브러리를 global로 설치

```bash
ncu -u
```
- 전체 의존성 라이브러리를 최신버전으로 올려줌
- 최신버전이 없어서 upgrade가 안되는 라이브러리가 있으면 해결이 안될수도있음
- --regacy-peer-deps 옵션을 사용해보자
  - 동일한 peer dependency 충돌 문제를 무시하고 패키지를 설치 
