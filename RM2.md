## 1. HTML

- Sử dụng các thẻ đa dạng, các mục đích khác nhau và chú ý dùng Html5 để đa dạng hóa, SEO.
- Tránh sử dụng các tag không cần thiết ("</input>", "</img>",..)
- Với hệ thống html lớn, lưu ý sử dụng lùi dòng 2 space thay vì 4 space.
- Không cần thiết thêm giá trị boolean khi làm việc với các attribute state của thẻ  *(e.g. `selected`, `disabled`, `checked`)*
    ```html
    <!-- Bad -->
    <input type="text" disabled="true">
    <input type="checkbox" value="1" checked="true">
    <select>
      <option value="1" selected="true">1</option>
    </select>

    <!-- Good -->
    <input type="text" disabled>
    <input type="checkbox" value="1" checked>
    <select>
      <option value="1" selected>1</option>
    </select>
    ```

---

## 2. Sass
### 2-1. Common
- Với hệ thống html/css thuần túy, sử dụng selector tới class là hợp lý nhất thay vì sử dụng các attribute selector (Vue + react nên sử dụng attribte selector)
- Với hệ thống html lớn, lưu ý sử dụng lùi dòng 2 space thay vì 4 space.
- Sử dụng dấu nháy đơn thay vì nháy kép.
- Với các lớp selector trùng nhau, đảm bảo nó được viết trên từng dòng 1.
  ```scss
  /* Bad */
  .selector, .selector-secondary, .selector[type=text] {
    padding: 15px;margin: 15px;
  }

  /* Good */
  .selector,
  .selector-secondary,
  .selector[type='text'] {
    padding: 15px;
    margin: 15px;
  }
  ```
- Chỉ nên dùng selector 3depth cho độ sâu, hay cố gắng module các thành phần.
- Nếu giá trị là đơn vị thập phân, cố gắng giữ lại số 0 phía trước.
  ```scss
  /* Bad */
  .selector {
    width: 0px;
    height: 2px;
    background-color: rgba(0, 0, 0, .15);
      }

  /* Good */
  .selector {
    width: 0;
    height: 2px;
    background-color: rgba(0, 0, 0, 0.15);
  }
  ```
- 인라인 CSS, `@extend` 사용을 지양한다.
- 주석은 최종 산출물에 출력되지 않도록 인라인 주석(`//`)으로 사용한다.

### 2-2. Fonts
- 폰트관련 속성을 mobile/pc 환경에서 유동적으로 사용하기 위해, 유동 단위로 변환해주는 과정이다.
- 기본 입력 단위는 **px**이며, **숫자**만 입력한다.

#### [Usage]
```scss
@include fonts({{font-size}});
@include fonts({{font-size}}, {{font-weight}});
@include fonts({{font-size}}, false, {{line-height}});
@include fonts({{font-size}}, {{font-weight}}, {{line-height}});

@include fonts(18);
@include fonts(14, 100);
@include fonts(15, false, 15);
@include fonts(18, 700, 22);
```

#### [Process]
1. font-size는 단위를 제외하고 숫자만 입력한다. font-weight, line-height는 생략 가능하다.
    ```scss
      // font-size: 18px;
      @include fonts(18);
    ```
2. font-weight의 재선언 없이 line-height를 사용하고 싶은 경우는 `false`값으로 대체한다.
    ```scss
    // font-size: 18px; line-height: 22px;
    @include fonts(18, false, 22);
    ```
3. 개발자도구 등으로 유동 단위로 변환된 값을 확인한다. line-height 값은 소수점 2째자리까지 반올림 처리된다.
    ```scss
    /* Input */
    @include fonts(18, 700, 22);

    /* Output */
    // AS-IS
    font-size: 18px; font-weight: 700; line-height: 22px;
    // TO-BE
    font-size: 1.8rem; font-weight: 700; line-height: 1.22;
    ```


### 2-3. SVG Sprite
- svg 파일들을 하나의 스프라이트 이미지로 합쳐서 사용한다.
- svg 파일 경로 : `/src/img/sprites-svg/{{폴더명}}/{{파일명}}.svg` 
- 스프라이트 이미지는 각 폴더별로 1개씩 생성된다.

#### [Usage]
```scss
@include useSvg-{{폴더명}}('이미지명');
@include useSvg-{{폴더명}}('이미지명', {{width}}px);

@include useSvg-pattern('pattern2');
@include useSvg-common('main_icon-search', 20px);
```

#### [Process]
1. 사용할 svg 파일을 위에 기입된 **svg 파일 경로**에 추가한다.
2. 신규 폴더명을 토대로 `/src/img/`, `/dist/img/` 경로에 SVG Sprite 이미지가 생성된다.  
3. 이미지명 선언 시, single quotation 으로 사용한다.
    ```scss
    /* Bad */
    @include useSvg-common("main_icon-search");

    /* Good */
    @include useSvg-common('main_icon-search');
    ```
4. 요소가 실제 파일 크기와 다른 크기여야 하는 경우, px 단위로 추가한다.<br> 이미지 비율을 유지하기 위해 width 값만 입력한다.
    ```scss
    @include useSvg-common('main_icon-search', 30px);
    ```

---

## 3. Javascript

### 3-1. Common
- 스크립트 분기용 선택자는 `js-` 접두사를 사용하며, 이 선택자에는 스타일을 포함하지 않는다.
- 인라인 CSS 사용을 지양하며, 필요 시 클래스를 추가, 삭제하여 스타일을 제어한다.
- 변수는 함수 상단에 정의하며, for문 외부에서 선언한다.
- 변수 선언 시 전역 선언을 지양하며, 하나의 var 선언에 결합한 형태로 사용한다.
- 이벤트 바인딩은 `on()` / `off()` 메소드를 사용한다.
- HTML에 이벤트 핸들러를 직접 설정하지 않는다.
  ```html
  <!-- Bad -->
  <a id="myLink" href="#" onclick="myEventHandler();">my link</a>
  ```
  ```javascript
  // Good

  $('#myLink').on('click', myEventHandler);
  ```
- 메서드 체이닝 시 가독성을 위해 줄바꿈을 사용한다.
  ```javascript
  // Bad
  $('#myLink').addClass('bold').on('click', myClickHandler).on('mouseover', myMouseOverHandler).show();

  // Good
  $('#myLink')
      .addClass('bold')
      .on('click', myClickHandler)
      .on('mouseover', myMouseOverHandler)
      .show();
  ```
- 속성을 연속해서 변경할 때 메서드 체이닝 대신 객체 리터럴을 사용한다.
  ```javascript
  // Bad
  $myLink.attr('href', '#').attr('title', 'my link').attr('rel', 'external');

  // Good
  $myLink.attr({
      href: '#',
      title: 'my link',
      rel: 'external'
  });
  ```
- 자식요소가 동적으로 추가 또는 수정되는 경우, 이벤트 위임(Event delegation)을 사용한다.
  ```javascript
  $("#parent-container").on("click", "a", delegatedClickHandler);
  ```