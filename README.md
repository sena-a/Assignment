# Vue를 활용한 게시판 프로젝트

## Project localhost 구동

```
npm install
```

```
npm run serve
```

## 반응형 웹

480px 이하 : 모바일 버전

481px 이상 : 데스크탑, 테블릿 버전

## 사용 스택

- Vue.js
- less
- bootstrap
- axios

#### 문제 해결

netlify로 배포하여 https 프로토콜을 사용중이나 ajax 통신 api 주소가 http 프로토콜로 이루어져 있어 헤더에 관련 메타 태그 추가.

```
<meta http-equiv="Content-Security-Policy" content="upgrade-insecure-requests"> 
```
