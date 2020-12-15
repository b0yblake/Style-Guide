## 1. COMMON

### 1-1. BEM

- BEM (**Block__Element—Modifier**) Dựa trên phương pháp luận, hãy đọc kỹ doc trước khi đọc note này . [http://getbem.com/naming/](http://getbem.com/naming/)
- Block : đại diện cho 1 khối riêng biệt, nó sẽ bao trùm - tương tác với các thành phần khác của trang 1 cách gần như độc lập. Hãy tưởng tượng rằng, nếu viết theo phương pháp BEM, project có ít element, có các khối module lặp lại liên tục, còn không - hãy lựa chọn phương pháp khác.
- Element : Phần con của "Block", để chỉ các thực thể cấp thấp hơn nhưng không hoàn toàn nhỏ lẻ.
  ```html
  <div class="form-login">
    <h2 class="form-login__title">
  </div>
  ```
- Modifier : Phần con của "Element" hoặc "Block", cực kì riêng lẻ, thưởng chỉ thuộc tính, trạng thái, giá trị attributes.
  ```html
  <div class="wrap-layer">
    <div class="layer layer--full"></div>
  </div>
  ```
- 1 vài trường hợp, 1 từ chưa đủ nghĩa để diễn tả ngữ nghĩa của "Block" **Hãy dùng dấu(-)** khi muốn mở rộng ngữ nghĩa.
- Đừng lo khi bạn bị trùng lặp với "Modifer", hãy làm thật nhiều để rút được kn.
- Block có thể đứng 1 mình - hoặc kết hợp multiple block.
- Đừng phức tạp hóa cấu trúc, vì BEM đã dài dòng rồi, đừng cố nhét 4-5 thậm chí 6 cấp của selector nhé.
  ```html
  <!-- `header` block -->
  <div class="header">
    <div class="form-search header__form-search"></div>
  </div>
  ```
- Mixer với phương pháp viết tường minh và gò ép : **Sử dụng các Boolean và cấu trúc gò ép** *(Prefix)*


### 1-2. Prefix

- Các trạng thái khi dùng CSS :  `is-` / khi dùng CSS để control, hãy chú ý dùng các trạng thái
    - `is-active`
    - `is-disabled`
    - `is-selected`
    - `is-scroll`
- Javascript : `js-` / dùng với js
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

- Hãy chú ý các dạng viết tắt của tên class, nó không còn thích hợp trong thể kỷ 21. Nếu viết tắt, xin hãy làm như fb or google.
  ```html
  <!-- Bad -->
  <div class="box-search">
    <strong class="box-sh__tit"></strong> <!-- sh = shorthand/search/shit/sheet.., tit = title -->
    <span class="box-sh__txt"></span> <!-- sh = shorthand/search/shit/sheet.., txt = text -->
    <p class="box-search__dsc"></p> <!-- dsc = description/disc -->
  </div>

  <!-- Good -->
  <div class="box-search">
    <strong class="box-search__title"></strong>
    <span class="box-search__text"></span>
    <p class="box-search__desc"></p>
  </div>
  ```
- Các cấp độ bao bọc khi viết `wrap` > `area` > `box` > `inner`.
- `group` khi muốn gói 1 tổ hợp các phần tử giống nhau.
  ```html
  <!-- Bad -->
  <div class="box-search">
    <div class="area-search"> <!-- "box-search" nhỏ hơn "area-search" nên không hợp lý -->
      <div class="inner-search">
          Search
      </div>
    </div>
    <div class="area-btn"> <!-- "box-search" nhỏ hơn "area-btn" nên không hợp lý -->
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