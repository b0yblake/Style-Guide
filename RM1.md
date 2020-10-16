## 1. 공통

### 1-1. BEM

- BEM (**Block__Element—Modifier**) 방법론을 기본으로 한다. [http://getbem.com/naming/](http://getbem.com/naming/)
- Block : 독립적인 최소 단위로 컴포넌트의 기능을 할 수 있는 UI의 기준. 컴포넌트의 고유 이름을 지정한다.
- Element : 컴포넌트에 종속되어있는 요소
  ```html
  <div class="form-login">
    <h2 class="form-login__title">
  </div>
  ```
- Modifier : Block이나 Element의 디자인이나 형태를 변화시키는 속성
  ```html
  <div class="wrap-layer">
    <div class="layer layer--full"></div>
  </div>
  ```
- 각 구간에서 클래스명을 확장할 때는 **하이픈(-)** 으로 통일한다.
- Block은 단독 또는 다른 Block과 혼합해서 사용할 수 있다.
  ```html
  <!-- `header` block -->
  <div class="header">
    <div class="form-search header__form-search"></div>
  </div>
  ```
- BEM을 사용하지 않을 때 : **Boolean의 의미를 가진 상태 클래스** *(하단 Prefix 참고)*


### 1-2. Prefix

- 상태 클래스 :  `is-` / CSS 제어용으로 show/hide와 같이 true/false의 의미로 제어할 때 사용한다.
    - `is-active`
    - `is-disabled`
    - `is-selected`
    - `is-scroll`
- Javascript : `js-` / 스크립트로 구조 제어가 필요할 때만 사용하며, CSS에서 사용하지 않는다.
- 요소별 고정 클래스 (Block, Element 구간에서 사용)
    - `<ul>` `<ol>`: `list-*`
    - `<li>`: `item-*` 
    - `<form>`: `form-*` 
    - `<input type="text|search|password|email">`: `input-*` 
    - `<input type="checkbox|radio">`: `checkbox-*`  `radio-*` 
    - `<label>`: `label-*` 
    - `<a>`: `link-*`  
    - `<button>`: `btn-*`
    - title : `title-*`
    - text : `text-*`
    - description : `desc-*`
  
---
## 2. HTML

- 정해진 클래스명 외에는 축약형, 줄임말을 사용하지 않는다.
  ```html
  <!-- Bad -->
  <div class="box-search">
    <strong class="box-sh__tit"></strong> <!-- sh 말줄임, tit 말줄임 -->
    <span class="box-sh__txt"></span> <!-- sh 말줄임, txt 말줄임 -->
    <p class="box-search__dsc"></p> <!-- dsc 말줄임 -->
  </div>

  <!-- Good -->
  <div class="box-search">
    <strong class="box-search__title"></strong>
    <span class="box-search__text"></span>
    <p class="box-search__desc"></p>
  </div>
  ```
- 감싸는 영역은 `wrap` > `area` > `box` > `inner` 순으로 한다.  name이 같은 form 요소를 감쌀때는 `group`으로 통일한다.
  ```html
  <!-- Bad -->
  <div class="box-search">
    <div class="area-search"> <!-- box-search 안에 area-search가 있음 -->
      <div class="inner-search">
          검색
      </div>
    </div>
    <div class="area-btn"> <!-- box-search 안에 area-btn이 있음 -->
      버튼
    </div>
  </div>

  <!-- Good -->
  <div class="area-search">
    <div class="box-search">
      <div class="inner-search">
        <strong id="a11y-group-birth">Date of Birth</strong>
        <div role="group" aria-labelledby="a11y-group-birth">
            <input type="text" title="4 digit year (Required)">
            <input type="text" title="2 digit month (Required)">
            <input type="text" title="2 digit date (Required)">
        </div>
      </div>
    </div>
    <div class="area-btn">
      버튼
    </div>
  </div>
  ```
---
## 3. CSS/Sass

- 상태를 분기할 때는 HTML 기본 속성이나 클래스로 제어하며, 상태 클래스는 `is-` 접두사를 사용한다.
- 변수명, 믹스인명은 케밥 케이스(kebab-case)를 사용한다.

---
## 4. Javascript

- 전역 변수를 사용하지 않는다.
- 변수명, 함수명은 카멜 케이스(camelCase)를 사용한다.
  ```javascript
  // 숫자, 문자, 불린
  var variableName,
      isBoolean;
  
  // 배열 - 배열은 복수형 이름을 사용
  var arrays = [];
  
  // 정규표현식 - 정규표현식은 'r'로 시작
  var rDesc = /.*/;
  
  // 생성자 함수 - 대문자 카멜케이스 (최상단 SIBF 제외)
  win.SIBF.Layer = function (container, args) {
  // win.SIBF['Layer'] = function (container, args) {
      ...
  };
  
  // 함수/메소드 - 동사 + 목적어 형태
  function getPropertyName() {
    ...
  }
  
  win.SIBF['Layer'].prototype = {
      init : function () {
          ...
      },
      bindEvents: function () {
          this.layerWrap.on('click', '.layer-opener', $.proxy(this.onLayerOpen, this));
      },
      // 이벤트 핸들러 - 이벤트 핸들러는 'on'으로 시작
      onLayerOpen : function () {
        ...
      },
      onLayerClose : function () {
        ...
      }
  }
  ```
- 스크립트 분기용 선택자는 `js-` 접두사를 사용하며, 이 선택자에는 스타일을 포함하지 않는다.