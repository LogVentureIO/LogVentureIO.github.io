---
title: "부트스트랩 반응형을 위해 필요한 개념 총정리 (feat. Grid, container, breakpoint)"
excerpt: "부트스트랩의 사이즈표와 container, grid 레이아웃에 대해 정리합니다"
categories:
  - frontend
tags:
  - html-css
---

```jsx
환경
- html
- css
- bootstrap
```

부트스트랩 사용을 위해 필요한 css와 js를 가져온다.

## 가. 부트스트랩의 기본 사이즈표 정리 🩵
[부트스트랩 정리 by.개발자의낙원](https://batcave.tistory.com/29)

부트스트랩의 화면 사이즈 표

![image](https://github.com/user-attachments/assets/2c1a1ed8-2d4d-4b8e-90b6-c17171e019c6){: width="80%" height="80%"}{: .align-center}
뷰포트의 크기를 나누는 6가지 기준이다. xs를 제외하고는 전부 이상의 개념이며 sm을 사용한다면 스몰 사이즈보다 클 때는 해당 설정을 적용한다. breakpoint가 6개이다. 해당 포인트는 커스텀할 수 있다.

### 화면 사이즈 축약어

```jsx
| xs   | eXtra Small           | `< 576px`                |
| sm   | SMall                 | `≥ 576px and < 768px`    |
| md   | MeDium                | `≥ 768px and < 992px`    |
| lg   | LarGe                 | `≥ 992px and < 1200px`   |
| xl   | eXtra Large           | `≥ 1200px and < 1400px`  |
| xxl  | eXtra eXtra Large     | `≥ 1400px`               |
```
> 뷰포트의 크기와 관계없이 기본적으로 하나의 row=행에는 12개의 col=열이 존재한다. 화면의 크기에 따라 몇 개의 col이 하나의 행에 배치될지 결정한다.
> 

```jsx
col-md-12 // md 사이즈 화면에서 하나의 행이 12개의 열 크기를 차지
col-sm-6 // sm 사이즈 화면에서 co
```

![image](https://github.com/user-attachments/assets/a1799a12-bca4-499a-a530-438460398608){: width="80%" height="80%"}{: .align-center}
## 나. container 🩵

[부트스트랩 반응형 그리드 알아보기](https://soulno.tistory.com/21)

container는 부트스트랩에서 가장 기본적인 레이아웃 요소이다. 그리드 시스템 사용에 필요하다. container 클래스는 기본적으로 여백을 적용해 콘텐츠를 가운데 정렬한다. .container-fluid를 사용하면 여백이 없어지고 콘텐츠가 화면 전체에 퍼진다. 

 container는 화면 사이즈에 따라 너비가 지정되어있다. fluid를 사용하면 항상 너비가 100%가 된다. breakpoint를 설정하면 해당 사이즈보다 커져야 너비가 조정되고 그 전까지는 너비가 100%가 된다. 즉 breakpoint는 max-width의 중단점을 변경한다. `container-md` 은 md 사이즈 전까지는 너비가 100프로가 된다. 

```css

.container
.container-fluid
.container-{breakpoint} // container-sm,md,lg,xl,xxl
```

![image](https://github.com/user-attachments/assets/f5fda6f4-9779-461e-8e72-f69ddffbdd19){: width="80%" height="80%"}{: .align-center}
## 다. Grid 레이아웃 🩵

행렬 형태의 구조이다. 반응형 레이아웃을 위해 사용된다. 12개의 열로 구성된 그리드 기반으로 구성된다. 열과 행을 이용한다. 하나의 행은 12개의 열을 포함한다. row 클래스에서는 자식 요소를 가로로 정렬하고 그 사이의 간격을 관리한다.

```jsx
<div class="container">
      <div>
        <div class="row">
          <div class="col-md-6 col-1-bg text-center p-4">Column 1</div>
          <div class="col-md-6 col-2-bg text-center p-4">Column 2</div>
        </div>
        <div class="row">
          <div class="col-md-4 col-3-bg text-center p-4">Column 3</div>
          <div class="col-md-4 col-4-bg text-center p-4">Column 4</div>
          <div class="col-md-4 col-5-bg text-center p-4">Column 5</div>
        </div>
      </div>
    </div>
...
```

![image](https://github.com/user-attachments/assets/6f0c8502-6d97-48a7-b8d5-24d538a4ad67){: width="80%" height="80%"}{: .align-center}
미디엄 사이즈 이상의 화면에서는 설정한 값이 열 12칸 중 그 만큼을 차지한다. 미디엄 미만의 화면에서는 디폴트로 열이 12칸 전부를 차지하게 된다. 즉 각 열이 한 줄씩 쌓이게 된다.