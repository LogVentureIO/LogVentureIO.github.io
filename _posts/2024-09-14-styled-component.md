---
title: "Styled-components 구조 잡기"
excerpt: "Styled-components의 네이밍 선택하기. s-dot 방식과 className 방식"
categories:
  - frontend
tags:
  - html-css
---
# 0. styled components란?
CSS, 테일윈드, SASS까지 학습하고 나니 모두 장단점이 존재합니다. 스타일드 컴포넌트도 많이들 쓰시기에 이것도 써보려고 합니다. 그러나 막상 작성하다보니 파일 구조를 어떻게 잡아야 할지 모르겠습니다. 이에 대해 정리해두려고 합니다.

# 1. 설치하기

```css
npm i styled-components
```

# 2. 사용방법

```css
import styled from "styled-components";

export const ProductWrap = styled.div`
	.product {
		display : flex;
	}
`;
```

```html
<ProductWrap>
	<div> Product </div>
</ProductWrap>
```

위처럼 스타일을 컴포넌트처럼 작성해서 사용합니다. 컴포넌트처럼 props를 내려줘서 사용할 수도 있습니다. `SASS` 처럼 중첩해서도 사용 가능합니다.

# 3. S-dot 네이밍

[복잡한-styled-components-구조-개선해보기](https://velog.io/@hayoung474/Front-End-%EB%B3%B5%EC%9E%A1%ED%95%9C-styled-components-%EA%B5%AC%EC%A1%B0-%EA%B0%9C%EC%84%A0%ED%95%B4%EB%B3%B4%EA%B8%B0)

위의 방법까지는 쉽게 적용했는데 작성하다보니 css의 고질적인 문제… 코드가 너무 길어지기 시작했습니다. 그렇다면 파일을 분리해야 하는데 어떤 방식으로 해야할지 찾아보게 되었습니다. 그러던 중 네이밍 방시기에도 여러가지가 있다는 것을 알게 되었고 그 중 `s-dot` 방식도 알게 되었습니다.

이 네이밍 방식은 **BEM(Block, Element, Modifier)** 네이밍 규칙에서 파생되었다고 합니다.

> BEM 방식은 CSS 클래스 이름을 구조적으로 작성하는 방법론으로 재사용성과 유지보수성을 높이기 위해 개발된 네이밍 컨벤션이다.
> 

## 적용 방법

- pages/home/login.jsx 파일의 스타일 코드를 분리하기
    - styles/home/login.style.js 파일을 생성합니다.
    - 해당 파일에 스타일 코드를 모두 옮깁니다. 옮긴 스타일을 사용하기 위해 export를 붙입니다.
    - 기존의 login.jsx에서 코드를 추가합니다.
        
        ```html
        import * as S from 'styles/home/login.style';
        ```
        
        S라는 이름으로 해당 파일 안의 모든 코드를 가져옵니다. 사용할 때는 컴포넌트 앞에 S. 을 붙여서 사용해 가독성을 높입니다.
        
        ```html
        <S.Wrapper>
          <S.MainContainer>
            <S.Title>
        ...
        ```
        

# 4. 어떤 네이밍 방식을 사용할지?

[Styled-components Naming](https://heycoding.tistory.com/71)

테일윈드의 단점은 className을 사용하고 여러 유틸리티 클래스를 무작정 붙이다 보니 css 구조를 파악하기도 어렵고 수정도 어렵고…길어지고 그런 것이다. 그래서 className을 사용하지 않는 방식인 ***s-dot*** 방식이 좋은 것 같기도 하고? 

근데 사실 일단 클래스 네임으로 작성해놨고 `SASS` 방식으로 클래스 중첩해서 작성하는 방식도 나쁘지 않은 것 같다. `s-dot` 방식을 쓰면 스타일 태그마다 컴포넌트로 분리해야 해서 sass 방식을 사용할 수 없는게 조금 아쉽. 정답은 없으니 편한 대로 쓰면 될 듯.

개인적으로 최상위 컴포넌트인 스타일 컴포넌트에 S.을 붙여서…아래는 클래스 네임으로 네이밍하는 방식은….너무 독자적인 방식인가? 쩝..