---
title: "[Github Blog] minimal mistakes - 코드 스니펫 커스텀"
excerpt: "minimal mistakes 인라인 코드와 코드블럭 커스텀하기"
categories:
  - blog
tags:
  - blog
  - github-blog
---

인라인 코드가 흰색 배경에 검정 글씨, 약간 폰트 크기가 작아지게 나오고 있다.

잘 눈에 띄지 않아서 바꾸려고 한다.

sass의 `syntax.scss` 파일을 살펴본다. 살펴보니 `<code>` 태그 스타일을 정의한 부분이 없다. 아마 브라우저의 기본 스타일이나 테마의 css파일을 따라간 것이 아닌가 싶다.

`<code>` 태그에 스타일을 추가해주기로 했다. 노션에서 사용하는 것처럼 글씨색을 붉은색과 분홍색? 사이의 색으로 설정했다.

![image](https://github.com/user-attachments/assets/d2e753de-b882-417d-b396-bdeffdb8078f){: width="70%" height="70%"}{: .align-center}


code만 추가하자 코드블럭에서도 같은 설정이 적용되어 이상해졌다. 코드블럭에 대한 부분도 추가적으로 커스텀해서 해결했다.