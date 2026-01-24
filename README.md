<!-- README.md -->

<p align="center">
  <img src="https://readme-typing-svg.demolab.com?font=Noto+Sans+KR&size=34&pause=1200&color=111111&center=true&vCenter=true&width=1000&lines=CodeAdventure+Daelim;Stage+Based+Coding+Quiz+Platform" alt="CodeAdventure Typing" />
</p>

<p align="center">
  <img src="https://img.shields.io/badge/React-CRA-61DAFB?style=for-the-badge&logo=react&logoColor=black" />
  <img src="https://img.shields.io/badge/Node.js-Express-339933?style=for-the-badge&logo=node.js&logoColor=white" />
  <img src="https://img.shields.io/badge/MySQL-DB-4479A1?style=for-the-badge&logo=mysql&logoColor=white" />
  <img src="https://img.shields.io/badge/Netlify-toml-00C7B7?style=for-the-badge&logo=netlify&logoColor=white" />
</p>

---

# CodeAdventure Daelim
여러 언어(C / Java / JavaScript / HTML / CSS)를 **스테이지 기반 퀴즈(전투)**로 학습하는 웹 프로젝트입니다.  
로그인 → 언어 선택 → 스테이지 진행 → 정답 제출 → 보상(코인/경험치) → 상점/힌트 구매 흐름으로 구성했습니다.

<br/>

## 🖼️ 프로젝트 포스터
<p align="center">
  <img src="./코드어드벤처포스터.png" width="820" alt="CodeAdventure Poster" />
  <br />
  <sub><b>길드 게시판(현상금/토벌) 컨셉으로 '학습 = 퀘스트' 경험을 시각화</b></sub>
</p>

<br/>

## 🔗 Quick Links
- 서비스 도메인(서버 CORS 설정 기준): `https://www.codeadventure.shop`
- 발표자료(PDF): `./코드어드밴쳐.pdf`
- 포스터(PNG): `./코드어드벤처포스터.png`
- 서버 엔트리: `server.js`
- 배포 설정(Netlify): `netlify.toml`

<br/>

## ⚠️ 레포 상태(실행 전 확인)
현재 레포 구성(업로드된 파일 기준)은 아래와 같습니다.

- ✅ 포함: `server.js`, `package.json`, `netlify.toml`, 포스터/발표자료(PDF)
- ❗ 확인 필요:
  - `server.js`에서 `./lib/sessionOption`을 `require`하고 있는데, **현재 레포에는 `lib/` 폴더가 없습니다.**
  - CRA 프로젝트라면 일반적으로 존재하는 `src/`, `public/` 폴더가 **현재 레포에는 없습니다.**
  - `server.js`는 `build/`를 정적 서빙하도록 되어 있으나, **현재 레포에는 `build/` 폴더가 없습니다.**

즉, **이 레포 상태 그대로는 로컬 실행/빌드가 실패할 가능성이 높습니다.**  
(코드/설정 파일을 별도 위치에서 관리 중이면, 이 레포에 합쳐서 올리거나 README에 “코드 위치/배포 방식”을 명시하는 게 안전합니다.)

<br/>

## 📌 목차
1. [핵심 컨셉](#1-핵심-컨셉)
2. [사용자 흐름](#2-사용자-흐름)
3. [주요 기능](#3-주요-기능)
4. [전체 구조](#4-전체-구조)
5. [레포 구조](#5-레포-구조)
6. [기술 스택](#6-기술-스택)
7. [API 요약](#7-api-요약)
8. [실행 방법](#8-실행-방법)
9. [배포](#9-배포)
10. [문서](#10-문서)

<br/>

## <a id="1-핵심-컨셉"></a> 1. 핵심 컨셉
- 학습을 ‘퀘스트’로 변환
  - 문제 = 전투(스테이지)
  - 결과 = 보상(코인/경험치) + 진행도 저장
- 반복 학습 유도
  - 진행도/보상 누적으로 성취감 강화
- 최소 진입 장벽
  - 로그인 후 언어/스테이지 선택만으로 즉시 학습 진행

<br/>

## <a id="2-사용자-흐름"></a> 2. 사용자 흐름
- 회원가입/로그인
- 언어 선택
- 스테이지 목록 확인
- 스테이지 퀴즈 진행(정답 제출)
- 진행도 업데이트 + 보상 지급(코인/경험치)
- 상점에서 아이템/힌트 구매 (재고/관리 기능 포함)

<br/>

## <a id="3-주요-기능"></a> 3. 주요 기능
### 계정/세션
- 회원가입/로그인, 로그인 상태 확인(`/authcheck`)
- 세션 기반 로그인 유지 (세션 스토어: MySQL)

### 스테이지 학습
- 언어별 스테이지 진행도 관리(유저 진행 필드 기반)
- 스테이지별 퀴즈 조회 및 정답 제출(`/quiz`, `/submit-answer`)
- 결과 즉시 피드백/진행 반영

### 유저 정보/게임화 지표
- 유저 정보 조회(`/userinfo`)
- 코인/경험치/레벨 등 “게임형 지표” 포함

### 상점/힌트
- 상품 목록 조회(`/shop`), 구매(`/purchase`)
- 상품 수량 변경(`/update-quantity/:productId`)
- 힌트 구매(`/purchase-hint`)
- 구매 로그(`/purchase-log`)

### 관리자
- 관리자 권한 확인(`/managercheck`)

<br/>

## <a id="4-전체-구조"></a> 4. 전체 구조
```txt
[ Frontend (React/CRA) ]
        ↓ (REST, withCredentials)
[ Backend (Node/Express) ]
        ↓
[ MySQL (users / quiz / shop / session ... ) ]
```

- 서버 기본 포트: `3001` (server.js 기준)
- CORS origin: `https://www.codeadventure.shop` (server.js 기준)

<br/>

## <a id="5-레포-구조"></a> 5. 레포 구조
```txt
/
  .gitignore
  README.md
  netlify.toml
  package.json
  package-lock.json
  server.js
  코드어드밴쳐.pdf
  코드어드벤처포스터.png
```

### 파일별 역할(업로드 기준)
- `server.js`: Express 서버 + 세션(MySQLStore) + API 라우트
- `netlify.toml`: Netlify 빌드/배포 설정(프론트 빌드 기준)
- `코드어드밴쳐.pdf`: 프로젝트 소개/화면 캡처 자료
- `코드어드벤처포스터.png`: 포스터 이미지

<br/>

## <a id="6-기술-스택"></a> 6. 기술 스택
### Frontend (package.json 기준)
- React (Create React App: `react-scripts`)
- react-router-dom
- styled-components
- react-code-blocks
- react-player

### Backend (package.json + server.js 기준)
- Node.js, Express
- mysql2
- express-session, express-mysql-session
- bcrypt
- cors, body-parser

<br/>

## <a id="7-api-요약"></a> 7. API 요약
아래 라우트 목록은 `server.js`에 정의된 기준입니다.

### Base
- GET `/` (빌드된 프론트가 있을 경우 `build/index.html` 서빙)

### Auth / User
- GET  `/authcheck`
- GET  `/users`
- GET  `/userinfo`
- POST `/signin`
- POST `/login`
- GET  `/logout`

### Stage / Quiz
- GET  `/check-language-start`
- GET  `/stages?language={c|java|js|html|css}`
- GET  `/quiz/:stageId?language={...}`
- POST `/submit-answer`

### Shop / Admin
- GET  `/managercheck`
- GET  `/purchase-log`
- GET  `/shop`
- POST `/purchase`
- POST `/update-quantity/:productId`
- POST `/purchase-hint`

<br/>

## <a id="8-실행-방법"></a> 8. 실행 방법
> ⚠️ 현재 레포 상태 그대로는 `lib/sessionOption`, `src/public/build` 부재로 실행이 실패할 수 있습니다.  
> 아래는 “코드가 정상 포함된 상태”를 전제로 한 실행 절차입니다.

### 8.1 설치
```bash
npm install
```

### 8.2 프론트 실행(CRA)
```bash
npm start
```

### 8.3 백엔드 실행
```bash
node server.js
```

### 실행 실패 원인(감점 포인트)
- `./lib/sessionOption` 미존재 → 서버 즉시 크래시
- DB 접속정보(호스트/유저/비번/DB명) 불일치
- 세션 스토어(MySQL) 테이블/권한 미설정
- CORS origin이 고정(`https://www.codeadventure.shop`) → 로컬 테스트 시 차단 가능
- `build/` 미존재 상태에서 `/` 접근 시 프론트 서빙 실패

<br/>

## <a id="9-배포"></a> 9. 배포
### 9.1 Netlify(프론트)
`netlify.toml` 기준:
```txt
[build]
  base = ""
  command = "CI=false npm run build"
  publish = "build"
```

### 9.2 서버(API)
- `server.js`는 CORS origin이 `https://www.codeadventure.shop`로 고정되어 있습니다.
- 배포 환경에서는 다음 중 하나로 정리하는 게 안전합니다.
  - 배포 도메인/포트를 CORS origin과 동일하게 맞춤
  - 환경변수로 origin을 분리(로컬/운영)

<br/>

## <a id="10-문서"></a> 10. 문서
### 발표자료(PDF)
- `./코드어드밴쳐.pdf`

<details>
<summary><b>PDF에 포함된 주요 내용(요약)</b></summary>

- 프로젝트 개요/목표
- 시작화면, 회원가입/로그인
- 언어 선택, 스테이지 선택
- 전투(퀴즈) 화면
- 개선 포인트 정리

</details>

---
