# Note

không thể sử dụng newObject.prototype cho Literal Object nó chỉ được sử dụng trong constructor function vì constructor function là 1 first-class object

## Object.create()

- Khi sử dụng Object.create(obj) -> obj sẽ trở thành nguyên mẫu của đối tượng được tạo
- Đối tượng được tạo -> {}, không có thuộc tính riêng trừ khi sử dụng tham số thứ 2 của Object.create(obj, propertiesObject)
- propertiesObject phải là dạng một đối tượng có tính chất giống đối số thứ hai của Object.defineProperties()

```
{
  'property1': {
    value: true,
    writable: true,
    ...
  },
  'property2': {
    value: 'Hello',
    writable: false,
    ...
  }
}
```

```js
var userInfo = {
  name: 'Hieunv',
  age: 20,
};

var newObject = Object.create(userInfo, {
  property1: {
    value: true,
  },
  property2: {
    value: 'Hello',
  },
});
// newObject -> {},
//newObject.__proto__.name === userInfo.name

// Việc thay đổi thuộc tính ở userInfo sẽ dẫn đến sự thay đổi của newObject

userInfo.name = 'change';
newObject.name; // 'change'

// Tạo thuộc tính riêng cho đối tượng newObject.name = 'abc'

newObject.name = 'abc';
// newObject -> {name: 'abc'},
// newObject.__proto__.name === userInfo.name
// newObject.name !== userInfo.name

// Việc thay đổi thuộc tính ở userInfo sẽ không thay đổi thuộc tính riêng của newObject

userInfo.name = 'change';
newObject.name; // 'abc'
```

Ưu điểm

- Chúng ta có thể tận dụng lại các tài sản ở nguyên mẫu
- Không cần phải tiêu tốn thêm dung lượng bộ nhớ để lưu các method và properties hiện có

Nhược điểm

- Đối tượng mới được tạo không tạo ra một thuộc tính riêng biệt. Sửa đổi thuộc tính từ Đối tượng nguồn, cũng sửa đổi dữ liệu có sẵn cho Đối tượng mới.(shalow copy)

## Prototye chain

### Prototype chain work example
```js
let f = function () {
   this.a = 1;
   this.b = 2;
}
let o = new f(); // {a: 1, b: 2}

// Thêm thuộc tính cho f.prototype
f.prototype.b = 3;
f.prototype.c = 4;

console.log(o.a); // 1

console.log(o.b); // 2

//Thuộc tính của chính nó đè lên prototype

console.log(o.c); // 4
//nó không có thuộc tính c nên quay lên prototype để tìm kiếm

console.log(o.d); 
```
[READMORE](https://viblo.asia/p/prototype-trong-javascript-hoat-dong-nhu-the-nao-RQqKLYXOZ7z)


### Hiệu suất 

Thời gian tra cứu các thuộc tính tăng cao trên chuỗi nguyên mẫu có thể có tác động tiêu cực đến hiệu suất và điều này có thể có ý nghĩa trong mã trong đó hiệu suất là rất quan trọng. Ngoài ra, cố gắng truy cập các thuộc tính không tồn tại sẽ luôn đi qua chuỗi nguyên mẫu đầy đủ.

hasOwnProperty là điều duy nhất trong JavaScript liên quan đến các thuộc tính và không đi qua chuỗi nguyên mẫu.

Không nên mở rộng các native prototype Object.prototype,...

## So sánh object.create vs new

### trường hợp không tham số

```js
function Dog() {
  this.pupper = 'Pupper';
}

Dog.prototype.pupperino = 'Pups.';
var maddie = new Dog();
var buddy = Object.create(Dog.prototype);

//Using Object.create()
console.log(buddy.pupper); //Output is undefined
console.log(buddy.pupperino); //Output is Pups.

//Using New Keyword
console.log(maddie.pupper); //Output is Pupper
console.log(maddie.pupperino); //Output is Pups.
```

Trong trường hợp này Object.create sẽ không chạy hàm construtor -> có thể sẽ nhanh hơn so với new. Nhưng trong thực tế ta thường dùng với dạng truyền tham số

Benmark : https://jsperf.com/function-vs-l

### Trường hợp có tham số

```js
// new
function userInfo(name, age) {
  (this.name = name), (this.age = age);
}
userInfo.prototype = {
  pro: 'pro',
};
var proto = new userInfo('hieu', 20);

//Object.create
var userInfo = {};
// Tương đương Object.create(Object.prototype)

userInfo.pro = 'pro';
var newObject = Object.create(userInfo, {
  name: {
    value: 'hieu',
  },
  age: {
    value: 20,
  },
});
```

Chúng ta thấy hiệu suất tương đương nhau , nhưng theo tài liệu [MDN](https://developer.mozilla.org/vi/docs/Web/JavaScript/Inheritance_and_the_prototype_chain) thì trong trường hợp Object.create() nếu tham số thứ 2 propertiesObject có thêm các mô tả thuộc tính sẽ dẫn đến hiệu suất kém hơn

Benmark: https://jsperf.com/function-vs-2-param

Ảnh minh họa chi tiết : https://stackoverflow.com/a/49434962

Cách viết prototype

// Kế thừa được
function Person(name, lastName, age) {
this.name = name;
}

Person.prototype.getInfo = function getInfo() {
return this.name + " " + this.lastName + ", " + this.age + "!";
}

// Không kế thừa được
function Person(name, lastName, age) {
this.name = name;
this.getInfo = function getInfo() {
return this.name + " " + this.lastName + ", " + this.age + "!";
}
}
