---
title: "[Get Started] Next.js 14 프로젝트 시작하기"
excerpt: "Next.js 14 프로젝트 초기 세팅"
categories:
  - FE
tags:
  - nextjs
  - get-started
---

배운지 얼마나 됐다고 벌써 머리속이 새하얗습니다. 더 늦기 전에 next.js14의 프로젝트 세팅법을 포스팅해서 정리해둡시다.

# 1. Next.js 14 시작하기

리액트는 vite를 주로 썼는데요. next.js도 여러 방법이 있지만 결국 공식 사이트에서 유도하는 내용을 따라가는 것이 맞습니다. 보일러 플레이트라고 하지요.

프로젝트를 진행할 새폴더를 만들고 해당 경로에서 진행합시다.

```jsx
npx create-next-app@latest
```

설정들은 전부 yes하면 타입스크립트도 사용하고 테일윈드도 사용합니다.

프로젝트 이름을 설정하면 폴더 안에 새로운 폴더로 프로젝트가 설치됩니다. 이미 새폴더라면 생략합시다.

적당히 목적에 맞게 설정합니다. 이후 다시 프로젝트를 실행할 때는 

```jsx
npm i // 개발하면서 추가된 package.json에 따라 라이브러리 등 설치
npm run dev // 개발모드로 프로젝트가 시작, 기본포트인 3000포트에서 시작
```

## 1-1. 개발모드

`package.json` 의 scripts 섹션에서 정의된 명령어들은 `npm run <script-name>` 형식으로 실행됩니다. 여기서 dev를 next dev와 연결해두었기에 개발모드가 실행됩니다. 마찬가지로 빌드하거나 start도 디폴트로 정의되어 있습니다.

![image](https://github.com/user-attachments/assets/856fae40-7348-4480-a6f3-318f0f972fdd){: width="80%" height="80%"}{: .align-center}

개발모드는 HMR(Hot module replacement)을 지원합니다. (hmr짱)

# 2. 추가 라이브러리

## [통신] Axios

fetch나 async/await를 써도 되지만 axios를 사용하면 서버 통신이 조금 더 편리해진다. 코드 가독성 면에서도 괜찮다.

```jsx
npm install axios
```

## [전역상태관리] Zustand

전역상태관리 라이브러리에는 Redux, Recoil 등이 있지만 zustand가 가장 간단하고 편리한 편이다. 타입스크립트로 작성된 라이브러리이다.

```jsx
npm i zustand
```

## [라우터] react-router-dom

클라이언트 측 라우팅 구현을 용이하게 해준다. 

```jsx
npm install react-router-dom
```

## [스타일] 리액트 아이콘

리액트 아이콘을 이용해 간편하게 아이콘을 사용할 수 있다. 이 때 아이콘은 텍스트 기반이기 때문에 테일윈드로 스타일 수정이 가능하다.

+. 이전에 진행한 프로젝트에서 리액트 아이콘이 청크가 매우 커 초기 로딩에 지대한 영향을 끼쳤다. 이후 svg파일로 수정해 진행했었다. 

```jsx
npm install react-icons
```

## [DB] mongoose

mongoDB를 사용하기 위해 설치합니다. 스키마를 정의할 수 있습니다.

```jsx
npm install mongoose
```

# 3. 마무리

![image](https://github.com/user-attachments/assets/1c1f745e-3ce9-4d1f-86f5-9ca569625c85){: width="80%" height="80%"}{: .align-center}

[라이브러리 랭킹](https://ght.creativemaybeno.dev/)