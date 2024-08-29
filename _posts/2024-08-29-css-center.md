---
title: "[CSS] 가운데 정렬 총정리"
excerpt: "[block] margin : 0 auto; ➕ [flex] justify-content:center; align-items:center; ➕ [position] transform:translate(-50%, -50%); ➕ [position] margin-top; margin-left;"
categories:
  - frontend
tags:
  - html-css
---

[CSS-가운데중앙-정렬-방법](https://velog.io/@won11/CSS-%EA%B0%80%EC%9A%B4%EB%8D%B0%EC%A4%91%EC%95%99-%EC%A0%95%EB%A0%AC-%EB%B0%A9%EB%B2%95)

css로 가운데로 정렬하는 방법은 여러가지가 있다. 관련해서 정리해보려고 한다.

```jsx
1. [block] margin : 0 auto;
2. [flex] justify-content:center; align-items:center;
3. [position] transform:translate(-50%, -50%);
4. [position] margin-top; margin-left;
```

## 1. [block] margin : 0 auto;

**block** 요소일 때 `수평 가운데 정렬` 한다. 사용하게 되면 브라우저에서 알아서 왼쪽과 오른쪽에 margin값을 넣어줘서 가운데 정렬이 된다. 수평 정렬, 즉 가로 줄 상에서 이루어지는 정렬이다.

![image](https://github.com/user-attachments/assets/e998f274-6785-4aa6-8b03-98e74a0b1aa5){: width="80%" height="80%"}{: .align-center}

## 2. [flex] justify-content:center; align-items:center;

display: flex;로 레이아웃을 잡을 때 가운데 정렬하는 방법이다. 순서대로 주축, 교차축에서 가운데로 위치를 잡는다. 보통 두 개를 함께 쓰는 경우가 많으므로 하나의 공식처럼 봐도 좋다.

```html
display : flex;
justify-content:center; 
align-items:center;
```

## 3. [position]

[position으로-가운데-정렬하기](http://hong.adfeel.info/frontend/position%EC%9C%BC%EB%A1%9C-%EA%B0%80%EC%9A%B4%EB%8D%B0-%EC%A0%95%EB%A0%AC%ED%95%98%EA%B8%B0/)

먼저 부모에는 *position:relative*를 자식에는 *position:absolute*를 주고

```html
// 자식에게 적용
top : 50%;
left : 50%;
```

위를 적용하면 

![image](https://github.com/user-attachments/assets/e87398ca-e545-4cac-a886-56449315af61){: width="80%" height="80%"}{: .align-center}

가운데 부분이 자식 요소의 `시작점`이 된다. 이 때 추가적인 조정이 필요하다. 여러 가지 방법이 존재한다.

- 자식 요소의 크기를 모를 때 : transform:translate(-50%, -50%);

```html
...
top : 50%;
left : 50%;
transform:translate(-50%, -50%); // 순서대로 x축, y축
```

- 자식 요소의 크기를 알 때 :  margin

```html
...
top : 50%;
left : 50%;
margin-left: -{width의 반}; // -50px;
margin-top : -{height의 반}; // -50px;
```

만약 자식의 w,h가 100px면 -50px씩 하면 된다.
![image](https://github.com/user-attachments/assets/a3dd0a8a-cf3d-417c-8550-149f2a21fbd7){: width="80%" height="80%"}{: .align-center}