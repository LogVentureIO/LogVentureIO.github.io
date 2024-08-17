---
title: "[Next.js] 성능 최적화하기"
excerpt: "Next.js에서 Lighthouse, next/image, next/font, bundle-analyzer를 사용해 최적화하기"
categories:
  - frontend
tags:
  - nextjs
  - optimization
---


프로젝트가 얼추 진행됐다면 최적화 작업이 필요한 시점이 옵니다. 개발을 진행하면서 최적화도 고려하면 좋지만 쉽지는 않지요.

# 1. Lighthouse

1차적으로 Lighthouse를 통해 성능 점수를 확인할 수 있습니다. lighthouse는 웹페이지의 성능, 접근성, SEO 등을 평가하고 개선할 수 있는 도구입니다. 

# 2. 이미지 최적화

next.js에서는 next/image를 제공합니다. 이를 이용해 최적화 작업을 할 수 있습니다.

next/image는 lazy loading를 사용해 초기 이미지 렌더링 속도를 높입니다. 또 webP와 같은 이미지 포맷을 사용해 이미지 파일 크기를 줄입니다. 

> lazy loading=지연로딩은 사용자가 스크롤해 이미지가 화면에 보이기 시작할 때 이미지를 로드합니다. 즉 현시점에 필요한 부분만 로드하도록 한다.
> 

# 3. 폰트 최적화

next/font를 사용할 수 있습니다. next.js에서 초기 서버를 렌더링할 때 폰트도 함께 적용되기 때문에 사용자는 폰트가 적용된 텍스트를 볼 수 있습니다.

# 4. 번들 사이즈 최적화

개발 환경에서 사용하기 때문에 **개발 의존성 모드**로 설치합니다.

```jsx
npm install --save-dev @next/bundle-analyzer
```

next.config.mjs 파일을 수정합니다.

![image](https://github.com/user-attachments/assets/91ffb695-a4e2-4c83-9cb3-0f61d76c84c3){: width="80%" height="80%"}{: .align-center}

설정했다면 다음 명령어로 실행합니다.

```jsx
ANALYZE=true next build
```

그러나 오류가 발생합니다. 살펴보니 현재 mjs파일이므로 require 대신 import를 사용해야 한다고 합니다. 즉 ES 모듈 방식으로 수정해야 합니다.

![image2](https://github.com/user-attachments/assets/7ccaad7e-85c8-4f20-937d-54ff63ca7a71){: width="80%" height="80%"}{: .align-center}

package.json에 명령어를 추가합니다

```jsx
"analyze": "ANALYZE=true next build"
```

이제 `npm run analyze` 를 통해 실행합니다.

실행하면 새탭으로 번들링을 분석한 html 파일이 열립니다. `.next/analyze` 폴더 안에 해당 html이 들어있습니다. 클라이언트와 서버 각각에 대해 html 파일이 생성됩니다.

![image](https://github.com/user-attachments/assets/10be1f29-0201-4c8b-8345-89f7da3549e6){: width="80%" height="80%"}{: .align-center}

터미널에도 번들 사이즈 크기가 출력됩니다. 테스트 용으로 만든 빈 프로젝트라 별 내용은 없습니다.

이를 기반으로 번들 사이즈를 줄여 성능을 개선할 수 있습니다.

---

[참고1](https://velog.io/@bjy100/Next.js-bundle-size)

[참고2](https://cheolsker.tistory.com/87)