Classes vs Objects vs Instances

class = blueprint (bản thiết kế, khuôn mẫu)
Objects là sản phẩm (thực thể) thực tế được xây dựng từ bản thiết kế
Instances là bản sao ảo của Objects

Vd: Class là loài người . Object là thực thể con người nói chung . 1 Instance là 1 con người nhất định nào đó.

Vd2: Class Car . Objects là tất cả các dòng xe được được tạo ra bởi khuôn mẫu là Car (có 4 bánh ,...). Và 1 Instance là 1 chiếc xe cụ thể từ các dòng xem trên

https://www.codecademy.com/forum_questions/558cd3fc76b8fe06280002ce
https://alfredjava.wordpress.com/2008/07/08/class-vs-object-vs-instance/

Function vs method

Sự khác biệt chính giữa một hàm và một phương thức lớp: Một hàm là trôi nổi tự do, không bị cản trở. Một phương thức lớp (ví dụ) phải nhận thức được nó là thuộc tính cha mẹ (và thuộc tính cha mẹ), do đó bạn cần truyền phương thức tham chiếu đến lớp cha (dưới dạng tự). Đó chỉ là một quy tắc ít ngầm định mà bạn phải tiếp thu trước khi hiểu OOP. Các ngôn ngữ khác chọn đường cú pháp thay vì đơn giản ngữ nghĩa, python không phải là ngôn ngữ khác.

Class vs factory

https://www.freecodecamp.org/news/class-vs-factory-function-exploring-the-way-forward-73258b6a8d15/

Factory vs contructor

https://medium.com/@chamikakasun/javascript-factory-functions-vs-constructor-functions-585919818afe

As you can see above, JavaScript has object versions of the primitive data types String, Number, and Boolean. But there is no reason to create complex objects. Primitive values are much faster.

ALSO:

Use object literals {} instead of new Object().

Use string literals "" instead of new String().

Use number literals 12345 instead of new Number().

Use boolean literals true / false instead of new Boolean().

Use array literals [] instead of new Array().

Use pattern literals /()/ instead of new RegExp().

Use function expressions () {} instead of new Function().

https://www.w3schools.com/js/js_object_constructors.asp

class vs prototype
https://developer.mozilla.org/vi/docs/Web/JavaScript/Guide/Details_of_the_Object_Model

**proto** vs prototye

prototype là một thuộc tính của một đối tượng Hàm. Nó là nguyên mẫu của các đối tượng được xây dựng bởi chức năng đó.

**proto** là một accessor property sẽ exposes [[prototype]] là internal property (tài sản nội bộ) của một đối tượng khi tao doi tuong voi new, trỏ đến nguyên mẫu của nó. Các tiêu chuẩn hiện tại cung cấp một Object.getPrototypeOf(O)phương pháp tương đương , mặc dù pp **proto** (không chuẩn) nhanh hơn.

Accessor properties are represented by “getter” and “setter” methods. In an object literal they are denoted by get and set
We don’t call user.fullName as a function, we read it normally: the getter runs behind the scenes.
https://javascript.info/property-accessors

function Point(x, y) {
this.x = x;
this.y = y;
}

var myPoint = new Point();

// the following are all true
myPoint.**proto** == Point.prototype
myPoint.**proto**.**proto** == Object.prototype
myPoint instanceof Point;
myPoint instanceof Object;

---

Class vs factory

class kém bảo mật hơn vì không có tính đóng gói

class TodoModel {
constructor(){
this.todos = [];
this.lastChange = null;
}

addToPrivateList(){
console.log("addToPrivateList");
}
add() { console.log("add"); }
reload(){}
}

var todoModel = new TodoModel();
console.log(todoModel.todos); //[]
console.log(todoModel.lastChange) //null
todoModel.addToPrivateList(); //addToPrivateList

mất this context
Tính bất biến

Class tốt hơn về bộ nhớ nếu số lượng instance lớn

Sâu hơn https://viblo.asia/p/class-va-factory-function-maGK7kWDKj2

constructor vs class vs factory
constructor phá vỡ Nguyên tắc Mở / Đóng
https://medium.com/javascript-scene/javascript-factory-functions-vs-constructor-functions-vs-classes-2f22ceddf33e

Nên sử dụng class thay vì constructor
https://github.com/airbnb/javascript/blob/master/README.md#classes--constructors

prototype based vs. class based inheritance

http://aaditmshah.github.io/why-prototypal-inheritance-matters/
![](https://res.cloudinary.com/practicaldev/image/fetch/s--P-uVQjti--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/ii4vwgaxg6jyt8e19zpd.png)
