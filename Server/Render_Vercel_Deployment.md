# Render(Back)_Vercel(Front) 배포하기
```
🔍️ React,node.express로 간단한거 만들건데 배포 날먹하는 방법 없냐고 물어보길래 알아봄
```


<br><br>

## :pushpin: Render (Back) 

###### 풀스택 애플리케이션을 간편하게 배포할 수 있는 클라우드 호스팅 플랫폼
###### Heroku 대안으로 자주 언급
<br>

### 장점
| 항목                     | 설명 |
|--------------------------|------|
| **무료 플랜**  :sparkles:          | 소규모 프로젝트에 적합한 무료 요금제 제공 |
| **CI/CD 자동 배포 연동** :sparkles: | GitHub, GitLab, Bitbucket 연동으로 자동 배포 지원  |
| **풀스택 지원**          | 웹 서버, 백엔드 API, 정적 파일, 웹소켓, Cron, Background Worker 등 모두 지원 |
|                          | - Express, FastAPI, Spring 등 다양한 백엔드 프레임워크 가능 |
| **내장 데이터베이스 지원** :sparkles: | PostgreSQL 직접 생성 가능 (Render 콘솔에서) |
|                          | `.env`에 DB URL만 입력하면 간단 연동 가능 |
| **배포 용이성** :sparkles:          | 빠른 배포 속도, 무중단에 가까운 배포 (이전 버전 유지) |
| **HTTPS / SSL 자동 적용**| 기본 URL 또는 도메인 연결 시 자동 HTTPS 적용 및 인증서 갱신 |
| **도메인 연결 쉬움**     | Custom 도메인 등록 및 DNS 설정 쉬움 |
| **롤백 기능 지원**  :sparkles:     | 이전 배포 버전으로 쉽게 복구 가능 (UI로 로그/버전 관리 가능) |
| **모니터링 UI 제공**   :sparkles:  | 빌드 로그, 서버 로그, 배포 이력 등을 실시간 확인 가능 |
| **다양한 언어/프레임워크**| Node.js, Python(FastAPI, Flask), Ruby, Go, Java(Spring), Rust 등 폭넓게 지원 |
<br>

### Render에서 사용 가능한 DB 비교 (추천도 순)

| DB 종류 | Render 호스팅 | 외부 연동|  특징 및 설명 |
|---------|----------------|-------------------|----------------|
| **PostgreSQL** | 가능 | 쉬움 | Render에서 직접 생성 가능. 가장 안정적이고 연동 쉬움 |
| **MongoDB (Atlas)** | 외부만 | 매우 쉬움  | Atlas 연결만 하면 끝. NoSQL이 필요한 경우 적합 |
| **Supabase** *(PostgreSQL 기반)* |외부 | 매우 쉬움| PostgreSQL 기반 + Auth, Storage 내장된 올인원 백엔드|
| **NeonDB** *(서버리스 PostgreSQL)* |  외부 | 매우 쉬움  | 서버리스 PostgreSQL, Render와 호환성 우수 |
| **Firebase** *(Firestore, Realtime DB)* |외부 |쉬움 | 서버리스 백엔드로 프론트엔드 연동 쉬움 (React 등과 호환 좋음)  |
| **PlanetScale** *(MySQL 기반)* | 외부 | 중간 | MySQL 기반, 연결 수 제한 있음 (서버리스 구조) |
| **Redis** *(Upstash 등)* | 외부 | 중간 (TLS 필요) | 캐시 용도로 훌륭, 초보자에겐 설정 다소 어려움  |
| **MySQL** *(직접 연결)* |  외부 |다소 복잡 | Render에서는 직접 호스팅 불가. 외부 연동만 가능  |
| **SQLite** | 가능 (로컬 파일) | 서버에는 부적합 | 테스트 용도에 적합. 서버 재시작 시 데이터 유지 안됨 |
<br>

### 상황별 추천 DB


- 정형화된 데이터 저장 (RDBMS) -> **PostgreSQL**  |  Render에서 직접 생성 가능, ORM 지원 풍부 (Prisma, Sequelize 등) 
- 빠른 NoSQL 개발 -> **MongoDB (Atlas)**  |  스키마 자유롭고 연동 매우 쉬움 
- 인증 + 스토리지 + DB 통합 -> **Supabase**  |  로그인, 파일 업로드, PostgreSQL을 한 번에 제공 
- 서버리스 + PostgreSQL 지향 -> **NeonDB**  |  무료 요금제, Render와 연결 시 속도 빠름 
- React/SPA 중심 프론트엔드 -> **Firebase**  |  클라이언트 SDK 중심, 백엔드 없이도 사용 가능 
- 캐시 처리 필요 -> **Redis (Upstash)**  |  빠른 응답과 상태 저장에 적합, 외부 캐시 연결 가능 
-  (실서비스 부적합) 복잡한 설정 없이 로컬 테스트 -> **SQLite**  |  DB 설치 없이 빠른 테스트 가능 
###### NeonDB (23년 서비스시작) Heroku Postgres 대안, 서버리스 PostgreSQL의 대표 / NoSQL인 PlanetScale이 MySQL 기반 서버리스라면, Neon은 PostgreSQL 기반 서버리스 DB

<br>

### Render 추천 조합 (추천도 순)
- **Render + PostgreSQL** : 백엔드 API + RDB | Render 내장 DB로 가장 쉬우며, ORM 연동도 쉬움 
- **Render + Supabase** : 서버 + 인증/DB 통합 | PostgreSQL 기반 + 인증/스토리지 통합된 풀스택 백엔드 
- **Render + MongoDB Atlas** : 백엔드 + NoSQL | Node.js + Mongo 조합으로 빠른 개발 가능 
- **Render (백엔드) + Vercel (프론트)** : API 서버 + 정적 사이트 | 프론트/백 분리한 SSR 구조에 적합 
- **Render + Firebase** : 서버 + 실시간 데이터 | 실시간 기능과 클라이언트 SDK 활용, 빠른 연동 가능 
- **Render + Redis (Upstash)** : API + 캐시 서버 | 고속 캐싱, JWT 세션 관리 등에 유용 
- **Render + PlanetScale** : 서버리스 MySQL | MySQL 사용 시 유용하나 설정 복잡, 연결 제한 있음 

<br>

### Vercel, Netlify, Heroku 등과 비교
| 플랫폼 | 프론트엔드 | 백엔드 | DB | 특징 |
|--------|------------|--------|-----|------|
| Render | ✅ | ✅ |  PostgreSQL | 풀스택 통합에 강점 |
| Vercel | ✅ | 서버리스 한정 |  외부 필요 | 정적 웹에 최적화 |
| Netlify | ✅ | 서버리스 한정 |  외부 필요 | JAMstack 중심 |
| Heroku | ✅ | ✅ | (PostgreSQL) | 오래된 구조, 유료화됨 |
<br>

### 실전 배포 예시
- Node.js + PostgreSQL 앱 배포하기
- React + FastAPI 프로젝트 Render + Vercel 조합으로 배포하기
<br>

### reference
- [Render 공식 문서](https://render.com/docs)
- [Render PostgreSQL 설정](https://render.com/docs/postgresql)

<br><br>

## :pushpin: Vercel (Front)
###### 정적 웹사이트 및 프론트엔드 프레임워크(SPA/SSR)를 위한 서버리스 호스팅 플랫폼  
###### Next.js의 공식 배포 플랫폼으로 유명하며, React, Vue, Svelte, Astro 등 지원  
<br>

### 장점
| 항목                      | 설명 |
|---------------------------|------|
| **무료 플랜**  :sparkles:           | 개인 개발자와 소규모 프로젝트에 적합한 무료 요금제 제공 |
| **정적 사이트 최적화** :sparkles:   | 빌드된 정적 자산(CSS/JS/HTML 등)을 글로벌 CDN으로 배포 |
| **Next.js 공식 플랫폼** :sparkles: | SSR, ISR, API Routes 등 Next.js 기능과 완벽히 호환 |
| **자동 배포**             | GitHub, GitLab, Bitbucket 연동으로 CI/CD 가능 |
| **Custom 도메인 지원**    | 도메인 연결이 매우 간편하며, 자동 SSL(HTTPS) 인증 적용 |
| **서버리스 함수 제공**    | `api/` 디렉토리 기반의 간단한 백엔드 구현 가능 (lambda) |
| **Preview Deployment** :sparkles: | 브랜치마다 자동으로 URL 생성 → PR 검토 시 유용 |
| **Vercel Analytics**      | 성능 분석 도구 내장 (유료 기능 일부 존재) |
<br>

### 지원 프레임워크 / 빌드 시스템

| 프레임워크 | 상태 |
|------------|------|
| **Next.js** | 공식 지원 (SSR, ISR, API Route 등 완벽 호환) |
| React + Vite | 정적 빌드로 사용 가능 , dist/로 배 |
| Vue / Nuxt | 정적 빌드 가능, Nuxt SSR은 별도 설정 필요 |
| SvelteKit / Astro | 정적 + 서버리스 조합 가능 |
| Gatsby | 정적 빌드 기반, react 기반 정적사이트 생성기 |
| HTML/CSS | 정적 파일 업로드만으로도 배포 가능 |
###### vercel은 정적 사이트에 최적화되어있다 
<br>

### 단점 / 제약사항

| 항목 | 설명 |
|------|------|
| **백엔드 기능 제한** | 서버리스 함수는 제한적, DB 연결 유지 어려움 |
| **DB는 외부 연동 필수** | 자체 DB 없음 → Supabase, Firebase, PlanetScale 등 사용 |
| **사용량 제한** | 초과 시 과금 또는 차단 (특히 서버리스 함수 호출 등) |
| **Next.js 외 프레임워크는 일부 기능 제한** | Preview URL, ISR 등은 Next.js에 최적화됨 |
<br>

### Vercel 추천 조합 (프론트 중심)

- **Vercel + Render** : 정적 프론트 + API 서버 완벽 분리 조합
- **Vercel + Supabase** : 클라이언트 + 백엔드(BaaS) 통합
- **Vercel + Firebase** : 실시간 데이터와 인증이 필요한 SPA
- **Vercel + PlanetScale** : 서버리스 MySQL 기반 앱 (ORM은 Prisma 등)
- **Vercel + NeonDB + Prisma** : 서버리스 PostgreSQL 기반 풀스택 조합

<br>

### Render와 비교 요약

| 항목 | Render | Vercel |
|------|--------|--------|
| **주요 용도** | 백엔드 중심 풀스택 | 프론트엔드 및 SSR 중심 |
| **DB 제공** | PostgreSQL 직접 생성 가능 | ❌ 없음 (외부 DB 연동 필요) |
| **정적 사이트 배포** | 지원하지만 CDN 최적화는 낮음 | 매우 강력 (CDN 기반) |
| **서버리스 함수** | Cron, Worker 등 포함 가능 | 제한된 lambda 구조 |
| **Next.js 통합** | 가능하지만 수동 설정 필요 | 공식 지원, 무설정 가능 |
| **추천 조합** | React + Express + PostgreSQL | Next.js + Supabase or Firebase |

<br>

### 실전 배포 예시
- Next.js 프로젝트를 GitHub 연동으로 Vercel에 자동 배포
- React + Vite 프로젝트를 빌드 후 정적 배포
- 프론트는 Vercel, 백엔드는 Render/Supabase 조합으로 분리 배포

<br>

### reference
- [Vercel 공식 문서](https://vercel.com/docs)
- [Vercel에 React 배포하기](https://vercel.com/docs/frameworks/react)
- [Vercel + Supabase 시작 가이드](https://supabase.com/docs/guides/hosting/vercel)
