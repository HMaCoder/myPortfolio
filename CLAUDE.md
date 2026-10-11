# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 개요

정적 단일 페이지 개인 포트폴리오입니다 (순수 HTML/CSS/JS, 프레임워크·package.json·빌드 도구·테스트 없음). Firebase Hosting(프로젝트 `acoder-portfolio`)에 배포됩니다. 콘텐츠와 코드 주석은 대부분 한국어입니다.

## 소스 파일과 배포 파일

- 수정은 **루트** 파일에서 합니다: `index.html`, `style.scss`, `script.js`, `404.html`, `images/`.
- `dist/`는 Firebase Hosting의 public 폴더입니다 (`firebase.json` → `"public": "dist"`). 빌드 결과물이 아니라 루트 파일을 수동으로 복사한 것이므로, 루트 파일을 수정해도 `dist/`에 복사하기 전까지는 실제 사이트에 반영되지 않습니다.
- `style.css`는 외부 Sass 컴파일러로 `style.scss`에서 컴파일됩니다 (파일 끝에 `sourceMappingURL` 주석이 있고, `style.css.map`은 gitignore 대상). 스타일을 바꿀 때는 `style.scss`를 수정하고 다시 컴파일합니다. 컴파일러가 없으면 `style.css`도 직접 맞춰 수정합니다:
  ```
  npx sass style.scss style.css
  ```
- `style.css-reference`, `p5.html`, `p5-1.html`은 연습/참고용 파일이며 사이트에 포함되지 않습니다.

## 배포

```
deploy.bat          # index.html, style.css, script.js, 404.html, images/를 dist/로 복사 (마지막에 `pause` 실행)
firebase deploy     # dist/를 Firebase Hosting에 업로드
```

`deploy.bat`은 `p5*.html`을 복사하지 않으며, `my-cv.pdf`를 루트에서 찾지만 실제 CV는 `images/my-cv.pdf`에 있습니다 (`images/`와 함께 복사됨). `deploy.bat`, `404.html`, `study참고.md`(배포 가이드 메모)는 gitignore 대상입니다.

로컬 미리보기는 `index.html`을 직접 열거나 폴더를 서빙합니다 (예: `npx serve .`, 또는 `dist/` 기준으로 `firebase serve`).

## 페이지 구조

`script.js`는 `<body>` 끝에서 로드되며 전역 함수/변수를 사용합니다. HTML은 인라인 `onclick` 속성으로 이 함수들을 호출합니다:
- 연락처 폼 `submit-to-google-sheet`은 `FormData`를 Google Apps Script URL로 POST합니다. `scriptURL`이 아직 placeholder(`"<add your own link here>"`)라서 폼 전송은 동작하지 않습니다.
- p5.js는 **전역 모드**로 동작합니다: 최상위 `setup()`, `draw()`, `windowResized()`와 `Star` 클래스가 `#p5Container`(fixed, `z-index: -1`, 모든 콘텐츠 뒤)에 전체 화면 별 배경을 그립니다. 전역 모드이므로 `script.js`에 p5 전역 이름(`setup`, `draw`, `color`, `map`, `dist` 등)과 겹치는 최상위 함수/변수를 추가하지 않아야 합니다. 또한 `noCursor()`로 사이트 전체에서 마우스 커서가 숨겨지고, 대신 색이 바뀌는 원이 마우스를 따라다닙니다.
