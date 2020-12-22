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
- Chỉ nên dùng selector 3depth cho độ sâu, hãy cố gắng module các thành phần.
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
- Inline CSS, `@extend` nên tránh sử dụng, hoặc nếu sử dụng, hãy đảm bảo phần extend là common.
- Sử dụng comment để CSS trong SASS được dễ hiểu nhất (`//`)

### 2-2. Fonts
- Việc sử dụng font giá trị "px" cho mobile/pc là không còn hợp lý vì môi trường 2 bên khác nhau, hãy đảm bảo giá trị font được dùng bằng `em` hoặc `rem`
- Đơn vị truyền vào là **px**, giá trị font-weight, giá trị line-height.

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
1. Chỉ font-size:
    ```scss
      // font-size: 18px;
      @include fonts(18);
    ```
2. không có font-weight (dùng mặc định 400)
    ```scss
    // font-size: 18px; line-height: 22px;
    @include fonts(18, false, 22);
    ```
3. có đầy đủ các yếu tố
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
- svg việc sử dụng file SVG sprite nhằm mục đích tối ưu tốc độ cũng như ratio của ảnh.
- svg file path : `/src/img/sprites-svg/{{folder-name}}/{{file-name}}.svg` 
- 1 file sprite tưng ứng với 1 folder ảnh icon SVG.

#### [Usage]
```scss
@include useSvg-{{folder-name}}('file-name');
@include useSvg-{{folder-name}}('file-name', {{width}}px);

@include useSvg-pattern('pattern2');
@include useSvg-common('main_icon-search', 20px);
```

#### [Process]
1. Add the svg file to be used to the **svg file path** listed above.
2. Based on the new folder name, the SVG Sprite image is created in the paths `/src/img/` and `/dist/img/`.
3. When declaring an image name, it is used as a single quotation.
    ```scss
    /* Bad */
    @include useSvg-common("main_icon-search");

    /* Good */
    @include useSvg-common('main_icon-search');
    ```
4. If the element must be of a different size than the actual file size, add it in px units.<br> Enter only the width value to maintain the image ratio.
    ```scss
    @include useSvg-common('main_icon-search', 30px);
    ```

---

## 3. Javascript

### 3-1. Common
- The selection for the quarter and the script using the `js-` prefix, this selector does not include the style.
- And avoiding the use inline CSS, add, and delete classes when necessary to control the style.
- Variable and function definitions at the top, for statements, declarations from the outside.
- And avoiding global declarations variable declarations, and used in the form bound to a declaration var.
- Event binding uses `on ()` / `off ()` method.
- It does not directly set the event handler to the HTML.
  ```html
  <!-- Bad -->
  <a id="myLink" href="#" onclick="myEventHandler();">my link</a>
  ```
  ```javascript
  // Good

  $('#myLink').on('click', myEventHandler);
  ```
- When chaining method to use line breaks for readability.
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
- When you change the properties continuously uses the object literal instead of chaining method.
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
- If the child element is dynamically added or modified, using the event-propagation (Event delegation).
  ```javascript
  $("#parent-container").on("click", "a", delegatedClickHandler);
  ```