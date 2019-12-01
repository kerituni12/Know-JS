# Thuật ngữ javascript

## Syntax Parser

> dịch code javascript sang mã máy

## Lexical Environment

> Trả lời cho câu hỏi code này được đặt ở đâu ?

## Execution Context

> Nơi | môi trường mà các đoạn mã JavaScript được evaluated và executed

- Global Execution Context
- Function Execution Context (chỉ được tạo khi function được invoked hoặc được called)
- Eval (not use)

## Call Stack (Execution Stack)

> cấu trúc ngăn xếp dạng LIFO để tạm thời lưu trữ và quản lý việc gọi hàm
- Xử lý đơn luồng.
- Xử lý đồng bộ.
- Có duy nhất 1 Global context.
- Vô hạn function contexts.
- Mỗi function khi được gọi sẽ sinh ra 1 execution context tương ứng, ngay cả khi nó gọi đến chính nó.

## Heap 

> vùng nhớ tạm -> cấp phát bộ nhớ cho các biến

## Queue

> cấu trúc dạng FIFO, phản hồi các sự kiện không đồng bộ bên ngoài (chẳng hạn như chuột được nhấp hoặc nhận phản hồi cho yêu cầu HTTP), callback

## Event Loop

> Event Loop rất đơn giản đó là đọc Stack và Event Queue. Nếu nhận thấy Stack rỗng nó sẽ nhặt Event đầu tiên trong Event Queue và handler (callback hoặc listener) gắn với Event đó và đẩy vào Stack

https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/

![Event Loop](https://miro.medium.com/max/800/1*iHhUyO4DliDwa6x_cO5E3A.gif)

## Job Queue

> chỉ dành riêng cho new Promise() , thực hiện hết job queue rồi mới tới call queue

```js
console.log('Message no. 1: Sync');
setTimeout(function() {
   console.log('Message no. 2: setTimeout');
}, 0);
let promise = new Promise(function(resolve, reject) {
   resolve();
});
promise.then(function(resolve) {
   console.log('Message no. 3: 1st Promise');
})
.then(function(resolve) {
   console.log('Message no. 4: 2nd Promise');
});
console.log('Message no. 5: Sync');

// Message no. 1: Sync
// Message no. 5: Sync
// Message no. 3: 1st Promise
// Message no. 4: 2nd Promise
// Message no. 2: setTimeout
```
## Primitive Type vs Reference Type

> Primitive Type khi copy giá trị của biến này cho biến khác, 2 giá trị này hoàn toàn độc lập không có liên hệ gì với nhau

- Boolean
- null
- undefined
- String
- Number

```js
let x = 10;
let a = x;
a = a + 10;
console.log(a) // 20
console.log(x) // 10
```

> Reference Type  không mang giá trị mà chỉ tham chiếu đến vùng lưu trữ của object đó trong bộ nhớ.

> Khi thực hiện so sánh = trên biến kiểu tham chiếu, trả về true khi cả 2 biến số cùng trỏ về một dùng nhớ chứ không phải so sánh giá trị của 2 biến.

- Array
- Function
- Object

```js
let arr = [];
arr.push(1);
let refArr = arr;
refArr.push(2);

console.log(arr, refArr); // [1, 2], [1, 2]
console.log(arr === refArr); //true

refArr = [1, 2]; // trỏ đến vùng nhớ khác  

console.log(arr === refArr); //false
```

## Pure Vs Impure Function 

> Một hàm chỉ thuần nếu nếu được cung cấp cùng một đầu vào, nó sẽ luôn tạo ra cùng một đầu ra, không tạo ra tác dụng phụ, không dùng các giá trị bên ngoài

```js
function changeAgePure(person) {
    var newPersonObj = JSON.parse(JSON.stringify(person));
    newPersonObj.age = 25;
    return newPersonObj;
}
var alex = {
    name: 'Alex',
    age: 30
};
var alexChanged = changeAgePure(alex);
console.log(alex); // -> { name: 'Alex', age: 30 }
console.log(alexChanged); // -> { name: 'Alex', age: 25 }
```

> Hàm không thuần túy 

```js
var age = 20; //
function changeAgeImpure(person) {
    person.age = 25;  // person.age = age; 
    return person;
}
var alex = {
    name: 'Alex',
    age: 30
};
var changedAlex = changeAgeImpure(alex);
console.log(alex); // -> { name: 'Alex', age: 25 }
console.log(changedAlex); // -> { name: 'Alex', age: 25 }
```
## Type Coercion 

- chỉ có 3 loại chuyển đổi
-- to string
-- to number
-- to boolean
- unary + , ! sẽ được thực thi trước
- Toán tử Logic || và && sẽ trả về giá trị gốc thay vì boolean 
- object -> valueOf -> toString 

Post: 
[1](https://www.freecodecamp.org/news/js-type-coercion-explained-27ba3d9a2839/), 
[2](https://thedevs.network/blog/type-coercion-in-javascript-and-why-everyone-gets-it-wrong)

### To String

```js
String([1,2,3])    // 1,2,3 // chuỗi các giá trị của array ngăn bởi dấu ,
String([])         // ""
String({})         // '[object Object]'
```

### To Boolean

```js
Boolean('')           // false
Boolean(0)            // false     
Boolean(-0)           // false
Boolean(NaN)          // false
Boolean(null)         // false
Boolean(undefined)    // false
Boolean(false)        // false
...                   // true
```

### To Number

> Tất cả toán tử sẽ kích hoạt chuyển đổi số trừ toán tử  + (nếu có tồn tại toán hạng là chuỗi) hoặc ==, != (nếu cả hai toán hạng điều là chuỗi)


```js
Number(null)                   // 0
Number(undefined)              // NaN
Number(true)                   // 1
Number(false)                  // 0
Number(" 12 ")                 // 12
Number("-12.34")               // -12.34
Number("\n")                   // 0
Number(" 12s ")                // NaN
Number(123)                    // 123
Number([])                     // 0
```

### Example

```js
true + false             // 1
12 / "6"                 // 2
"number" + 15 + 3        // 'number153'
15 + 3 + "number"        // '18number'
[1] > null               // true                ( 1 > 0)
"foo" + + "bar"          // 'fooNaN' 
'true' == true           // false               (NaN == 1)
null == ''               // false               (null not convert to 0 when use ==)
!!"false" == !!"true"    // true                (true == true)
['x'] == 'x'             // true 
[] + null + 1            // 'null1'             ('' + null => 'null')
[1,2,3] == [1,2,3]       // false               (không phải cùng 1 đối tượng)
{}+[]+{}+[1]             // '0[object Object]1'
!+[]+[]+![]              // 'truefalse'         (!0 + [] + !true => true + '' + false)
new Date(0) - 0          // 0                   (toán tử - kích hoạt chuyển đổi valueOf)
new Date(0) + 0          // 'Thu Jan 0'         (toán tử - kích hoạt chuyển đổi toSTring)
```

## Equal

[JS Comparison Table](https://dorey.github.io/JavaScript-Equality-Table/)

```js
null == 0               // false, null is not converted to 0
null == null            // true
undefined == undefined  // true
null == undefined       // true
NaN == NaN              // false
```

## Scope 

- Global 
- Local     (in function)
- Block     (in block, IIFE will create a block scope) 
- Lexical   (Every inner level can access its outer levels.)

## V8

[Cách thức hoạt động của JavaScript: V8 engine và 5 mẹo tối ưu hóa](https://techtalk.vn/cach-hoat-dong-cua-javascript-v8-engine-va-5-meo-toi-uu-hoa.html)

## Strict Mode

#### Not

- undefined
- Tên hàm trùng các keyword
- Trùng thuộc tính
- Tham số cùng tên
- Gán giá trị cho read-only
- Thay đổi arguments object
- Định nghĩa theo hệ cơ số 8
- Dùng eval để tạo biến

## New keyword , Prototype

### New
> New Dùng để Khởi tạo đối tượng

- Nó tạo ra một đối tượng mới, rỗng.
- Nó liên kết this với đối tượng mới được tạo.
- Nó thêm một thuộc tính vào đối tượng mới được tạo của chúng ta có tên là __proto__, trỏ vào đối tượng nguyên mẫu của hàm tạo.
- Nó thêm a return this vào cuối hàm, để đối tượng được tạo được trả về từ hàm.

[New Keywords](https://www.youtube.com/watch?v=d4zeaaJ1C7I&list=PLRhlTlpDUWsxVCluXaURF6NA_Q6uTSBFm&index=28)

### Prototype

> là một đối tượng liên kết các function và object (mặc định) cho phép function và object kế thừa các thuộc tính và method từ prototype của mình

- Có thể truy cập, chỉnh sửa prototype của function 
- Protype của object sẽ được chuyển sang proto khi tạo mới object

[Prototype](https://www.youtube.com/watch?v=pw45j6C5RyM&list=PLRhlTlpDUWsxVCluXaURF6NA_Q6uTSBFm&index=29)


## Closure

> Closures là khả năng của một chức năng ghi nhớ và tiếp tục truy cập các biến được xác định bên ngoài phạm vi của nó, ngay cả khi chức năng đó được thực thi trong một phạm vi khác.

- Outer keep states between multi call
- Inner reference to variable outer
- Return funtion to multi call

```js
function makeAdder() {
   let a = 0;           // keep a = 2, for (2)
  return function(y) {
    return a += y;
  };
}

var add10 = makeAdder();

console.log(add10(2));  // 2   (1)
console.log(add10(2))   // 4   (2)
```

## IIFE 

> Là một function expression được thực thi ngay sau khi biên dịch

- Tạo block scope riêng tránh sung đột global variable

```js
(var x = 1)                // fail
(function(){})             // true
(function(user){})(user)   // exec
```
## Rest Vs Spread

```js
//rest
function rest (a, ...b) {
  console.log(a);         // 1
  console.log(b);         // [2, 3, 4]
}

rest(1, 2, 3, 4);

// spread
let a = [1, 2]

console.log(1, 2, ...a);  // 1, 2, 1, 2

```

## Function Arrow, Anonymous, Expression, Constructor

- Anonymous funtion no name
- Expression ( biểu thức hàm )
- Constructor ( viết hoa từ đầu tiên, được excute với toán tử new)

```js

// Nếu return một đối tượng, thì đối tượng được trả về thay vì this.
// Nếu return một gt nguyên thủy, nó bị bỏ qua.

function BigUser() {

  this.name = "John";

  return { name: "Godzilla" };  // <-- returns this object
}

alert( new BigUser().name );  // Godzilla, got that object
```

- Arrow function () => {} , không binding this (không tạo ra ngữ cảnh this của riêng hàm), không được hoisted

[Readmore](https://developer.mozilla.org/vi/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
[When use arrow funtion] (https://www.freecodecamp.org/news/when-and-why-you-should-use-es6-arrow-functions-and-when-you-shouldnt-3d851d7f0b26/)



## Bitwise Operator

Readmore:  [E](https://blog.logrocket.com/interesting-use-cases-for-javascript-bitwise-operators/), [V](http://fedu.vn/thu-vien-hoc-tap/lap-trinh-web/front-end/javascript/huong-dan-javascript/cac-phep-tinh-bitwise-trong-javascript/),   
[Case use](https://stackoverflow.com/questions/654057/where-would-i-use-a-bitwise-operator-in-javascript),    
[Case use](https://codeburst.io/using-javascript-bitwise-operators-in-real-life-f551a731ff5),   
[Case use](https://medium.com/bother7-blog/bitwise-operators-in-javascript-65c4c69be0d3)


## DOM 

> mô hình các đối tượng trong tài liệu HTML, có nhiệm vụ xử lý các vấn đề như đổi thuộc tính của thẻ, đổi cấu trúc HTML của thẻ,...

## Factory Function

> Là một function không phải class hoặc hàm khởi tạo trả về new object mà trả về 1 object khi không dùng từ khóa new

```js
const createUser = ({ userName, avatar }) => ({
  userName,
  avatar,
  setUserName (userName) {
    this.userName = userName;
    return this;
  }
});
console.log(createUser({ userName: 'echo', avatar: 'echo.png' }));
```

- Default Parameters

```js
const createUser = ({
  userName = 'Anonymous',
  avatar = 'anon.png'
} = {}) => ({
  userName,
  avatar
});
console.log(
  // { userName: "echo", avatar: 'anon.png' }
  createUser({ userName: 'echo' }),
  // { userName: "Anonymous", avatar: 'anon.png' }
  createUser()
);
```

## This, call, bind, apply

[This](https://kipalog.com/posts/Con-tro-this-trong-Javascript)
[Call , bind, apply](https://kipalog.com/posts/PHAN-BIET-CALL--APPLY-VA-BIND-TRONG-JAVASCRIPT)
