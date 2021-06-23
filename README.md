# SCSS

## 주석
* //: 주석 처리 되지 않고 사라짐
```scss
.test {
  #till {
      // color: red;
  }
}
```

* /* */: css에 주석 처리 되어 컴파일 됨
```scss
.test {
  #till {
      /*color: red;*/
  }
}
```

```css
.test #till {
  /*color: red;*/
}
```

## 중첩
중첩은 기본적으로 후손 선택자로 컴파일 됨
```scss
//scss
.container {
  ul {
    li {
      font-size: 40px;
      .name {
        color: royalblue;  
      }
      .age {
        color: orange;
      }
    }
  }
}
```
```css
/*css*/
.container ul li {
  font-size: 40px;
}
.container ul li .name {
  color: royalblue;
}
.container ul li .age {
  color: orange;
}

```
*자식 선택자*로 만들어야 할 경우 > 를 붙임
```scss
//scss
.container {
  > ul {
    li {
      font-size: 40px;
      .name {
        color: royalblue;  
      }
      .age {
        color: orange;
      }
    }
  }
}
```

```css
/*css*/
.container > ul li {
  font-size: 40px;
}
.container > ul li .name {
  color: royalblue;
}
.container > ul li .age {
  color: orange;
}
```

## 상위(부모) 선택자 참조

```scss
//& - 상위 선택자 참조
.btn {
    position: absolute;
    &.active {
        color: red;
    }
}

.list {
    li {
        &:last-child {
            margin-right: 0;
        }
    }
}

.fs {
    &-small { font-size: 12px; }
    &-medium { font-size: 14px; }
    &-large { font-size: 16px; }
}
```

```css
.btn {
  position: absolute;
}
.btn.active {
  color: red;
}

.list li:last-child {
  margin-right: 0;
}

.fs-small {
  font-size: 12px;
}
.fs-medium {
  font-size: 14px;
}
.fs-large {
  font-size: 16px;
}
```

## 중첩 속성
```scss
.box {
    font: {
        weight: bold;
        size: 10px;
        family: sans-serif;
    };
    margin: {
        top: 10px;
        left: 20px;
    };
    padding: {
        top: 10px;
        bottom: 40px;
        left: 20px;
        right: 30px;
    };
}
```

```css
.box {
  font-weight: bold;
  font-size: 10px;
  font-family: sans-serif;
  margin-top: 10px;
  margin-left: 20px;
  padding-top: 10px;
  padding-bottom: 40px;
  padding-left: 20px;
  padding-right: 30px;
}
```

## 변수
```scss
$value: 300px;
//유효 범위: {} 안에서 혹은 global하게 사용 가능

.container {
    $size: 100px;
    position: fixed;
    top: $size;
    .item {
        $size: 200px;
        width: $size;
        height: $size;
        transform: translateX($size);
    }
}
```

```css
.container {
  position: fixed;
  top: 100px;
}
.container .item {
  width: 200px;
  height: 200px;
  transform: translateX(200px);
}
```

## 산술연산
```scss
div {
    $size: 30px;
    width: 20px + 20px;
    height: 20px - 10px;
    font-size: 10px * 2;
    margin: (30px / 2); /*괄호를 통해 사용*/
    border: solid $size / 15 red; /*변수를 통해 사용*/
    border-radius: 10px + 20px / -5; /*다른 산술연산과 같이 사용*/
    padding: 20px % 7;
}

span {
    font-size: 10px;
    line-height: 10px;
    font-family: serif;
    font: 10px / 10px serif;
}
```

```css
@charset "UTF-8";
div {
  width: 40px;
  height: 10px;
  font-size: 20px;
  margin: 15px;
  /*괄호를 통해 사용*/
  border: solid 2px red;
  /*변수를 통해 사용*/
  border-radius: 6px;
  /*다른 산술연산과 같이 사용*/
  padding: 6px;
}

span {
  font-size: 10px;
  line-height: 10px;
  font-family: serif;
  font: 10px/10px serif;
}
```

## 재활용(Mixins)
```scss
//인수(argument)
@mixin center($size: 80px, $color: tomato) {
    display: flex;
    justify-content: center;
    align-items: center;
    width: $size;
    background: $color;
}

.container {
    @include center(200px, red);
    .item {
        // 키워드 인수
        @include center($color: green);
    }
}
.box {
    @include center;
}
```

```css
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 200px;
  background: red;
}
.container .item {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 80px;
  background: green;
}

.box {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 80px;
  background: tomato;
}
```

## 반복문

```scss
@for $i from 1 through 3 {
    .box:nth-child(#{$i}) {
        width: 100px * $i;
    }
}

@for $i from 1 through 3 {
    .test {
        width: 100px * $i;
        > :nth-child(#{$i}) {
            width: 100px;
        }
    }
}
```

```css
.box:nth-child(1) {
  width: 100px;
}

.box:nth-child(2) {
  width: 200px;
}

.box:nth-child(3) {
  width: 300px;
}

.test {
  width: 100px;
}
.test > :nth-child(1) {
  width: 100px;
}

.test {
  width: 200px;
}
.test > :nth-child(2) {
  width: 100px;
}

.test {
  width: 300px;
}
.test > :nth-child(3) {
  width: 100px;
}
```

## 함수
```scss
@mixin center {
    display: flex;
    justify-content: center;
}

@function ratio($size, $ratio) {
    @return $size * $ratio;
}

.box {
    $width: 100px;
    width: $width;
    height: ratio($width, 1/2);
    @include center;
}
```

```css
.box {
  width: 100px;
  height: 50px;
  display: flex;
  justify-content: center;
}
```

## 색상 내장 함수
```scss
.box {
    $color: royalblue;
    width: 200px;
    height: 100px;
    margin: 20px;
    border-radius: 10px;
    background-color: $color;
    &.built-in {
        //mix, lighten, darken, saturate <> desaturate: 채도, grayscale: 회색으로, invert: 반전, 
        background-color: mix($color, red);
        color: lighten($color, 10%);
        background: darken($color, 30%);
        border-color: invert($color);
    }
}
```

```css
.box {
  width: 200px;
  height: 100px;
  margin: 20px;
  border-radius: 10px;
  background-color: royalblue;
}
.box.built-in {
  background-color: #a03571;
  color: #6d8ce8;
  background: #132c76;
  border-color: #be961e;
}
```

