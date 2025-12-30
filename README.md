<p align="center">
  <img src="https://readme-typing-svg.demolab.com?font=Noto+Sans+KR&size=34&pause=1200&color=111111&center=true&vCenter=true&width=1000&lines=CodeAdventure+Daelim;Stage+Based+Coding+Quiz+Platform" alt="CodeAdventure Typing" />
</p>

<p align="center">
  <img src="https://img.shields.io/badge/React-CRA-61DAFB?style=for-the-badge&logo=react&logoColor=black" />
  <img src="https://img.shields.io/badge/Node.js-Express-339933?style=for-the-badge&logo=node.js&logoColor=white" />
  <img src="https://img.shields.io/badge/MySQL-DB-4479A1?style=for-the-badge&logo=mysql&logoColor=white" />
</p>

---

# CodeAdventure Daelim
여러 언어(C / Java / JavaScript / HTML / CSS)를 **스테이지 기반 퀴즈**로 학습하는 웹 프로젝트입니다.  
사용자는 로그인 후 언어를 선택해서 스테이지를 진행하고, 정답 제출로 진행도를 올리며 보상(코인/경험치)을 얻는 흐름으로 구성했습니다.

- 서비스 도메인(서버 CORS 설정 기준): `https://www.codeadventure.shop`
- 발표자료: `코드어드밴쳐.pptx`

<br/>

## 📌 목차
1. [핵심 흐름](#1-핵심-흐름)  
2. [주요 기능](#2-주요-기능)  
3. [전체 구조](#3-전체-구조)  
4. [기술 스택](#4-기술-스택)  
5. [API 요약](#5-api-요약)  
6. [실행 방법](#6-실행-방법)  
7. [배포](#7-배포)

<br/>

## <a id="1-핵심-흐름"></a> 1. 핵심 흐름
- 회원가입/로그인 → 언어 선택  
- 스테이지 목록 확인 → 스테이지 퀴즈 진행  
- 정답 제출 → 진행도 업데이트 + 보상(코인/경험치)  
- 코인으로 상점에서 아이템/힌트 구매 (관리자/재고 관리 포함)

<br/>

## <a id="2-주요-기능"></a> 2. 주요 기능
- **계정/세션**
  - 회원가입/로그인, 로그인 상태 확인(authcheck)
  - 세션 기반 로그인 유지(세션 스토어: MySQL)

- **스테이지 학습**
  - 언어별 스테이지 진행도 관리(유저 진행 필드 기반)
  - 스테이지별 퀴즈 조회 및 정답 제출

- **유저 정보**
  - 유저 정보 조회(userinfo)
  - 코인/경험치/레벨 등 게임형 지표 포함

- **상점/힌트**
  - 상품 목록 조회(shop), 구매(purchase)
  - 상품 수량 변경(update-quantity)
  - 힌트 구매(purchase-hint)
  - 구매 로그(purchase-log)

- **관리자 체크**
  - 관리자 권한 확인(managercheck)

<br/>

## <a id="3-전체-구조"></a> 3. 전체 구조
<pre>
[ Frontend (React) ]
        ↓
[ REST API ]
        ↓
[ Backend (Node/Express) ]
        ↓
[ MySQL (users / quiz tables / shop ... ) ]
</pre>

- 클라이언트는 React로 화면/학습 흐름을 제공합니다.
- 서버는 Express로 API + 세션/권한/DB 처리를 담당합니다.
- 서버 기본 포트는 `3001`(server.js 기준)입니다.

<br/>

## <a id="4-기술-스택"></a> 4. 기술 스택
- **Frontend**
  - React (Create React App)
  - react-router-dom
  - styled-components
  - react-code-blocks (코드 표시)
  - react-player (콘텐츠 재생)

- **Backend**
  - Node.js, Express
  - mysql2
  - express-session, express-mysql-session
  - bcrypt (비밀번호 해시)
  - cors, body-parser

<br/>

## <a id="5-api-요약"></a> 5. API 요약
server.js 기준 주요 라우트(요약)

### Auth / User
- GET  `/authcheck`
- GET  `/users`
- GET  `/userinfo`
- POST `/signin` (회원가입)
- POST `/login`
- GET  `/logout`

### Stage / Quiz
- GET  `/check-language-start`
- GET  `/stages?language={c|java|js|html|css}`
- GET  `/quiz/:stageId?language={...}`
- POST `/submit-answer`

### Shop / Admin
- GET  `/shop`
- POST `/purchase`
- POST `/update-quantity/:productId`
- POST `/purchase-hint`
- GET  `/purchase-log`
- GET  `/managercheck`

<br/>

## <a id="6-실행-방법"></a> 6. 실행 방법
> 이 레포는 **프론트(React) + 백엔드(server.js)**가 함께 들어있는 형태입니다.  
> 실행은 보통 “프론트 dev 서버”와 “백엔드 API 서버”를 각각 띄우는 방식으로 진행합니다.

### 6.1 설치
<pre>
npm install
</pre>

### 6.2 프론트 실행
<pre>
npm start
</pre>

### 6.3 백엔드 실행
server.js는 별도 터미널에서 실행합니다.
<pre>
node server.js
</pre>

> DB/세션 옵션은 프로젝트 설정 파일(예: `lib/sessionOption`) 또는 환경설정에 맞춰 준비해 주세요.

<br/>

## <a id="7-배포"></a> 7. 배포
- `netlify.toml` 기준으로 프론트는 Netlify 빌드/배포 흐름을 사용합니다.
<pre>
command: CI=false npm run build
publish: build
</pre>
- 서버는 CORS가 `https://www.codeadventure.shop`로 설정되어 있어, 배포 시 도메인/포트 정책을 맞춰야 합니다.

---
