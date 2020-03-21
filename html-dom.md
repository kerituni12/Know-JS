## HTML (HyperText Markup Language) , DOM

[READMORE](https://javascript.info/dom-attributes-and-properties)
> HTML Là ngôn ngữ đánh dấu siêu văn bản. Nó là một XML namespace (tập các thẻ XML) mà trình duyệt có thể sử lý. Chúng ta nhìn vào một file HTML thì nhìn thấy text, còn trình duyệt nhìn vào sẽ thấy cây DOM.

> HTML không phải là ngôn ngữ lập trình -> không thể tạo ra các chức năng “động” 

### Block-Level Tags
[1.]() Tag `<html></html>` là element cao nhất dùng để đóng gói mỗi trang HTML.

[2.]() Tag `<head></head>` chứa các thông tin meta như là tiêu đề trang và charset.

[3.]() Tag `<body></body>` đóng gói tất cả nội dung sẽ hiện trên trang.

```html 
<html>
<head>
<!-- META INFORMATION -->
</head>
<body>
<!-- PAGE CONTENT -->
</body>
</html>
```


----
> DOM Là mô hình các đối tượng trong tài liệu HTML, có nhiệm vụ xử lý các vấn đề như đổi thuộc tính của thẻ, đổi cấu trúc HTML của thẻ,...

### Cấu trúc DOM

<pre><code class="ui segment hljs java">document
|<span class="hljs-function">__ <span class="hljs-title">documentElement</span> <span class="hljs-params">(HTML)</span>
   |__ <span class="hljs-title">bodyElement</span> <span class="hljs-params">(BODY)</span>
      |__ <span class="hljs-title">widthAttribute</span> <span class="hljs-params">(value = <span class="hljs-number">200</span>px)</span>
      |__ <span class="hljs-title">textNode</span> <span class="hljs-params">(CDATA = <span class="hljs-string">'This dummy'</span>)</span>
</span></code></pre> 

> Chúng ta có thể thêm property, method , prototype cho node DOM 

```js
Element.prototype.sayHi = function() {
  alert(`Hello, I'm ${this.tagName}`);
};

document.documentElement.sayHi(); // Hello, I'm HTML
document.body.sayHi(); // Hello, I'm BODY
```
#### Note:
- Type của DOM không phải lúc nào cũng là string vd (attribute checked -> property checked is true) or style is a object

-----
### Attributes vs Properties

- Properties : thuộc tính của đối tượng DOM
- Attributes  : được thêm vào HTML element 
- Khi trình duyệt load pages nó sẽ parse HTML and generates DOM from it .
- Một số attribute thay đổi -> properties thay đổi và ngược lại (ví dụ : id ), và một số chỉ có thể thay đổi 1 chiều (vd: value) 

#### Rule:

- A few HTML attributes have 1:1 mapping to properties. `id` is one example.
`<body id="page"> -> body.id="page"`

- Some HTML attributes don't have corresponding properties. `colspan` is one example.

- Some DOM properties don't have corresponding attributes. `textContent` is one example.

- Many HTML attributes appear to map to properties ... but not in the way you might think!


#### Note:

- Standard attribute của một element có thể không được hiểu bởi một element khác

``` html
<body id="body" type="...">
  <input id="input" type="text">
  <script>
    alert(input.type); // text
    alert(body.type); // undefined: DOM property not created, because it's non-standard
  </script>
</body>
```

Method kiểm tra attributes:

```html
elem.hasAttribute(name) - kiểm tra sự tồn tại.
elem.getAttribute(name) - được giá trị.
elem.setAttribute(name, value) - đặt giá trị.
elem.removeAttribute(name) - loại bỏ thuộc tính
elem.attributes - lấy tất cả đối tượng thuộc về lớp Attr (name, value, properties)
```


> Đối với hầu hết các tình huống sử dụng thuộc tính DOM là tốt hơn. Chúng ta chỉ nên tham chiếu các thuộc tính khi các thuộc tính DOM không phù hợp với chúng ta, ví dụ khi chúng ta cần các thuộc tính chính xác:

- Chúng ta cần một thuộc tính không chuẩn. Nhưng nếu nó bắt đầu với data-, thì chúng ta nên sử dụng dataset.

- Đọc giá trị 'as written' trong HTML. Giá trị của thuộc tính DOM có thể khác nhau, ví dụ, thuộc href tính luôn là một URL đầy đủ và chúng ta có thể muốn nhận giá trị gốc của bản gốc.

```html
<!DOCTYPE html>
<html>
<body>

  <div data-widget-name="menu">Choose the genre</div>

  <script>
    // getting it
    let elem = document.querySelector('[data-widget-name]');

    // Should use dataset then attribute to reading the value 
    alert(elem.dataset.widgetName);
    // or
    alert(elem.getAttribute('data-widget-name'));
  </script>
</body>
</html>
```


```js
html2dom = {
  acceptcharset: 'acceptCharset',
  accesskey: 'accessKey',
  bgcolor: 'bgColor',
  cellindex: 'cellIndex',
  cellpadding: 'cellPadding',
  cellspacing: 'cellSpacing',
  choff: 'chOff',
  class: 'className',
  codebase: 'codeBase',
  codetype: 'codeType',
  colspan: 'colSpan',
  datetime: 'dateTime',
  checked: 'defaultChecked',
  selected: 'defaultSelected',
  value: 'defaultValue',
  frameborder: 'frameBorder',
  httpequiv: 'httpEquiv',
  longdesc: 'longDesc',
  marginheight: 'marginHeight',
  marginwidth: 'marginWidth',
  maxlength: 'maxLength',
  nohref: 'noHref',
  noresize: 'noResize',
  noshade: 'noShade',
  nowrap: 'noWrap',
  readonly: 'readOnly',
  rowindex: 'rowIndex',
  rowspan: 'rowSpan',
  sectionrowindex: 'sectionRowIndex',
  selectedindex: 'selectedIndex',
  tabindex: 'tabIndex',
  tbodies: 'tBodies',
  tfoot: 'tFoot',
  thead: 'tHead',
  url: 'URL',
  usemap: 'useMap',
  valign: 'vAlign',
  valuetype: 'valueType' };
```

Course
https://app.pluralsight.com/library/courses/front-end-building-stronger-practices/table-of-contents
Html sematic , workflow
CSS style guide, calc css 
ID or Class - only use ID for major(chính) page component
HTML5 data attributes
Naming conventions: 
- create consistency
- help reduce errors
- camelCase
- name-width-dashes 
- name_with_underscore
- be descriptive
- use functional names
- use common abbreviations

naming conventions for files 
Avoid spaces , use hyphens to separate words(can use for img)

Use BEM .info

- BEM -  BLOCK 
A logically and functionally ’
the equivalent of a component in Web Components.
Blocks being independent allows for their re-use, as well as
facilitating the project development and support process.
Can be nested within other blocks.

Block naming convention
- only use classes, lowercase letters and one hyphen to separate words
Element naming convention
- double underscores

Editor configs
.editorconfig

Rule for commnets

Reset vs nomalize

Dont use import

Sass	(sasmeister.com)