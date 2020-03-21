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
promise
  .then(function(resolve) {
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
console.log(a); // 20
console.log(x); // 10
```

> Reference Type không mang giá trị mà chỉ tham chiếu đến vùng lưu trữ của object đó trong bộ nhớ.

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

## Pure Vs Impure Function , Mutate

> Một hàm chỉ thuần nếu nếu được cung cấp cùng một đầu vào, nó sẽ luôn tạo ra cùng một đầu ra, không tạo ra tác dụng phụ, không dùng các giá trị bên ngoài

```js
function changeAgePure(person) {
  var newPersonObj = JSON.parse(JSON.stringify(person));
  newPersonObj.age = 25;
  return newPersonObj;
}
var alex = {
  name: 'Alex',
  age: 30,
};
var alexChanged = changeAgePure(alex);
console.log(alex); // -> { name: 'Alex', age: 30 }
console.log(alexChanged); // -> { name: 'Alex', age: 25 }
```

> Hàm không thuần túy

```js
var age = 20; //
function changeAgeImpure(person) {
  person.age = 25; // person.age = age;
  return person;
}
var alex = {
  name: 'Alex',
  age: 30,
};
var changedAlex = changeAgeImpure(alex);
console.log(alex); // -> { name: 'Alex', age: 25 }
console.log(changedAlex); // -> { name: 'Alex', age: 25 }
```

### Mutate

[Readmore](https://medium.com/@fknussel/arrays-objects-and-mutations-6b23348b54aa)
[Case use](https://slemgrim.com/mutate-or-not-to-mutate/)

## Higher Order Functions & Callback Functions , First class function

- Higher Order Functions (nhận vào 1 hàm khác(callback) hoặc return ra 1 hàm khác)
- Callback là hàm được truyền vào hàm khác như 1 tham số nó sẽ được gọi kích hoạt bên trong hàm gọi nó.
- Lưu ý khi dùng callback (mất context this, callback hell)
- First class function hiểu ngắn gọn là ngôn ngữ cho phép xài hàm như biến, và cho phép hàm trong hàm, là kết quả trả về của một hàm, lưu trữ dữ liệu hay thậm chí là có thuộc tính riêng như đối tượng (objects)

## Generator function , Yield

> Generators are functions which can be exited and later re-entered

```js
function* name([param[, param[, ... param]]]) {
   statements
}
```

> yeild là từ khóa dùng để tạm dừng và cũng để tiếp tục việc thực thi bên trong generator function.

```js
function* generatorFunc(index) {
  while (index < 2) {
    yield index++;
  }
}

const iterator = generatorFunc(0);

console.log(iterator.next());
// log output: {value : 0, done : false}

console.log(iterator.next());
// log output: {value : 1, done : false}

console.log(iterator.next());
// log output: {value : underfined, done : true}
```

> yield\* có thể nhúng mã của một generator function ngay sau nó hoặc là ủy quyền trực tiếp cho một iterator object.

[Readmore](https://viblo.asia/p/javascript-es6-generators-and-yield-m68Z00bAZkG)

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
String([1, 2, 3]); // 1,2,3 // chuỗi các giá trị của array ngăn bởi dấu ,
String([]); // ""
String({}); // '[object Object]'
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

> Tất cả toán tử sẽ kích hoạt chuyển đổi số trừ toán tử + (nếu có tồn tại toán hạng là chuỗi) hoặc ==, != (nếu cả hai toán hạng điều là chuỗi)

```js
Number(null); // 0
Number(undefined); // NaN
Number(true); // 1
Number(false); // 0
Number(' 12 '); // 12
Number('-12.34'); // -12.34
Number('\n'); // 0
Number(' 12s '); // NaN
Number(123); // 123
Number([]); // 0
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
null == 0; // false, null is not converted to 0
null == null; // true
undefined == undefined; // true
null == undefined; // true
NaN == NaN; // false
```

## Scope, var, let , const

- Global
- Local (in function)
- Block (in block, IIFE will create a block scope)
- Lexical (Every inner level can access its outer levels.)

var: Function in which the variable is declared
let: Block in which the variable is declared
const: Block in which the variable is declared

Block scopes are what you get when you use if statements, for statements or write code inside curly brackets.

Rule 

Don’t use var, because let and const is more specific
Default to const, because it cannot be re-assigned or re-declared
Use let when you want to re-assign the variable in future
Always prefer using let over var and const over let

```js
function foo(){
  for (let i=0; i<5 ; i++){
    console.log(i);
  }
  console.log(i); // err
// if use var =>  i = 5
}

foo();
```

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
- Nó thêm một thuộc tính vào đối tượng mới được tạo của chúng ta có tên là **proto**, trỏ vào đối tượng nguyên mẫu của hàm tạo.
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
  let a = 0; // keep a = 2, for (2)
  return function(y) {
    return (a += y);
  };
}

var add10 = makeAdder();

console.log(add10(2)); // 2   (1)
console.log(add10(2)); // 4   (2)
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
function rest(a, ...b) {
  console.log(a); // 1
  console.log(b); // [2, 3, 4]
}

rest(1, 2, 3, 4);

// spread
let a = [1, 2];

console.log(1, 2, ...a); // 1, 2, 1, 2
```

## Function Arrow, Anonymous, Expression, Constructor

- Anonymous funtion no name
- Expression ( biểu thức hàm )
- Constructor ( viết hoa từ đầu tiên, được excute với toán tử new)

```js
// Nếu return một đối tượng, thì đối tượng được trả về thay vì this.
// Nếu return một gt nguyên thủy, nó bị bỏ qua.

function BigUser() {
  this.name = 'John';

  return { name: 'Godzilla' }; // <-- returns this object
}

alert(new BigUser().name); // Godzilla, got that object
```

- Arrow function () => {} , không binding this (không tạo ra ngữ cảnh this của riêng hàm), không được hoisted

[Readmore](https://developer.mozilla.org/vi/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
[When use arrow funtion](https://www.freecodecamp.org/news/when-and-why-you-should-use-es6-arrow-functions-and-when-you-shouldnt-3d851d7f0b26/)

# Operator
## Binary Operators


Arithmetic Operators (+, -, *, /+,−,∗,/)
Assignment Operators (=, +=, -=, *=)
Logical Operators ($&&, ||, ! $)

//Logical Operators
console.log("\n****Logical Operators****\n")
console.log("1 OR 1 = " + (1 || 1)) // 1 OR 1
console.log("1 OR 0 = " + (1 || 0)) // 1 OR 0
console.log("0 OR 0 = " + (0 || 0)) // 0 OR 0
console.log("1 AND 2 = " + (1 && 2)) // 1 AND 1
console.log("1 AND 0 = " + (1 && 0)) // 1 AND 0
console.log("0 AND 0 = " + (0 && 0)) // 0 AND 0
console.log(!true)  // NOT TRUE
console.log(!1)     // NOT TRUE
console.log(!false) // NOT FALSE
console.log(!0)     // NOT FALSE

ex const page = (result && result.page) || 0;

****Logical Operators****

1 OR 1 = 1
1 OR 0 = 1
0 OR 0 = 0
1 AND 2 = 2
1 AND 0 = 0
0 AND 0 = 0
false
false
true
true

Comma Operator (,),): The Comma operator evaluates each operand from left to right and returns the value of right most operand
Thường sử dụng trong vòng lặp for
A comma operator (,) is used when you want to evaluate an expression from left to right.
for (var a = 0, b =5; a <= 5; a++, b--)

Bitwise Operators (&, |, ^)
****Bitwise Operators****
Bitwise AND of 5 and 1: 1
Bitwise OR of 5 and 1: 5
Bitwise XOR of 5 and 1: 4


String Operators (+)

Conditional (ternary) operators (? :?:)

https://www.robinwieruch.de/conditional-rendering-react#multiple-conditional-renderings-in-react

## Unary Operators
typeof: Returns the type of the given operand
delete: Deletes an object, object’s attribute or an instance in an array
void: Specifies that an expression does not return anything
Increment Operators : ++, --

## Bitwise Operator

Readmore: [E](https://blog.logrocket.com/interesting-use-cases-for-javascript-bitwise-operators/), [V](http://fedu.vn/thu-vien-hoc-tap/lap-trinh-web/front-end/javascript/huong-dan-javascript/cac-phep-tinh-bitwise-trong-javascript/),  
[Case use](https://stackoverflow.com/questions/654057/where-would-i-use-a-bitwise-operator-in-javascript),  
[Case use](https://codeburst.io/using-javascript-bitwise-operators-in-real-life-f551a731ff5),  
[Case use](https://medium.com/bother7-blog/bitwise-operators-in-javascript-65c4c69be0d3)


Expressions
Anything that evaluates to a value is called an expression. Some of the basic expressions and keywords used in JavaScript are mentioned below:

this: points to the current object
super: calls methods on an object’s parent, for example, call parent’s constructor
function: used to define a function
function*: used to define a generator function
async function: used to define an async function


Function Declaration
Function Expression
Generator Function
Generator Function Expression
Arrow Function
Function Constructor



Function Declaration	This is the most typical method to declare a function in JavaScript. All functions declared using this method allow hoisting; means they can be used before declaration.	
function function_name(Arg1, Arg2..){}
Function Expression	This is the most commonly used type. It is most suitable to use when you want to assign your function as an object to a variable. It’s often used when you want to use your function as callback function.	
Named: var var_name = function function_name(Arg1,Arg2..){};
Anonymous:var var_name = function(Arg1, Arg2..){};
Generator Function Declaration	It is used to declare a Generator Function, a function that uses yeild keyword to return a Generator-Iterator object on which next method can be called later.	
function* name(Arg1, Arg2..) {}
Generator Function Expression	This is much similar to the type we just discussed above. The only difference is that it allows omitting name from the function.	
Named: function* function_name(Arg1,Arg2..){}
Anonymous:function* (Arg1,Arg2..){}
Arrow Function	The two reasons why this type of functions were introduced in ES6 are: writer shorter syntax for function expressions and get rid of this value. You can exclude function parentheses if it only takes one parameter. You can also erase the curly brackets if there’s only one statement inside function body.	
var var_name = (Arg1, Arg2..) => {};
Function Constructor	This is the least recommended way of declaring a function. Here, the Function keyword is actually a constructor which creates a new function. The arguments passed to the constructor become arguments of the newly created function and the last parameter is a string which is converted into a function body. This may cause security and engine optimization problems which is why it’s always never recommended to use.	
var var_name = new Function(Arg1, Arg2..,'FunctionBodyString');


## Factory Function

> Là một function không phải class hoặc hàm khởi tạo trả về new object mà trả về 1 object khi không dùng từ khóa new

```js
const createUser = ({ userName, avatar }) => ({
  userName,
  avatar,
  setUserName(userName) {
    this.userName = userName;
    return this;
  },
});
console.log(createUser({ userName: 'echo', avatar: 'echo.png' }));
```

- Default Parameters

```js
const createUser = ({ userName = 'Anonymous', avatar = 'anon.png' } = {}) => ({
  userName,
  avatar,
});
console.log(
  // { userName: "echo", avatar: 'anon.png' }
  createUser({ userName: 'echo' }),
  // { userName: "Anonymous", avatar: 'anon.png' }
  createUser(),
);
```

## This

- khi một hàm được gọi, thì nó sẽ có một thuộc tính là this, và thuộc tính this này chứa giá trị về một đối tượng đang gọi tới hàm này => cách để biết this đang tham chiếu đến object nào là nhìn vào nơi mà hàm chứa this được gọi

- This trong callback
- This trong closure
- This khi được gán bằng biến
- This khi sử dụng bind call apply

```js
let Man = {
  name: 'John',
  key: ['a', 'b', 'c'],
  getName: function() {
    return this.name;
  },
  mother: {
    name: 'Stacey',
    getName() {
      return this.name;
    },
    getkey() {
      this.key.forEach(function(v, i) {
        console.log(this); // this mất context không thể trỏ về đối tượng Man
      });
    },
  },
};

// var name = Man.getName();  // name sẽ được giữ trong global -> this.name = John ->  khi call callName() ở bên dưới sẽ trả về John

let name = Man.getName(); // đối tượng thực thi nó là Man,

console.log(this.name); // this.name -> undefined

console.log(name); // John

console.log(Man.mother.getName()); //Stacey

/**
 * This trong trường hợp gán vào 1 biến
 */
let callName = Man.getName;

console.log(callName()); // đối tượng thực thi nó là global object

/**
 * This trong callback
 */

$('button').click(Man.printName); // đối tượng thực thi nó là button, this mất context

// Use bind
$('button').click(Man.printName.bind(Man));

// Use arrow function
$('button').click(() => Man.printName);

/**
 * This trong closure hàm getkey
 */

// Use arrow function

this.key.forEach(() => console.log(this));

/**
 * This trong hàm mượn (borrowing methods)
 * Sử dụng bind, call, apply to call method thông qua method có sẵn của đối tượng khác
 */
```

Readmore: [This Kipalog](https://kipalog.com/posts/Con-tro-this-trong-Javascript),
[This tylermcginnis](https://tylermcginnis.com/this-keyword-call-apply-bind-javascript/)

## Call, bind, apply

- Hàm call và apply là gần giống nhau đều gọi hàm trực tiếp. Chỉ khác ở cách truyền tham số vào (với call thì đối số phân cách bởi dấu phẩy comma và với apply thì đối số cho bởi mảng array)
- Hàm bind không gọi hàm trực tiếp mà nó sẽ trả về một hàm mới. khi được gọi cùng với tham số this thì những tham số truyền vào sau này khi gọi tới function mới sẽ được thay đổi sang ngữ cảnh mà this gắn tới , truyền tham số giống call

EX bind & arrow function in class:
arrow function => private
https://viblo.asia/p/arrow-function-trong-reactcomponent-co-van-de-gi-L4x5xdBB5BM

```js
var person1 = { firstName: 'Jon', lastName: 'Kuperman' };
var person2 = { firstName: 'Kelly', lastName: 'King' };

function say(greeting1, greeting2) {
  console.log(
    greeting1 + ',' + greeting2 + ' ' + this.firstName + ' ' + this.lastName,
  );
}

say.call(person1, 'Hello', 'Good morning'); // => Hello,Good morning Jon Kuperman

say.apply(person1, ['Hello', 'Good moring']); // => Hello,Good moring Jon Kuperman

var sayHelloJon = say.bind(person1, 'Hello', 'Good morning');

sayHelloJon();
```

[Call , bind, apply](https://kipalog.com/posts/PHAN-BIET-CALL--APPLY-VA-BIND-TRONG-JAVASCRIPT)

## Object.create vs Object.assgin

> Object.create() tạo object mới, dùng object cũ như [ the prototype ] của object mới.

```js
const person = {
  isHuman: false,
  printIntroduction: function () {
    console.log(`My name is ${this.name}. Am I human? ${this.isHuman}`);
  }
};

const me = Object.create(person);

printIntroduction();             -> to _proto_
isHuman                          -> property
```

[Benifits](https://stackoverflow.com/questions/17392857/benefits-of-using-object-create-for-inheritance)

> Object.assign() sao chép tất cả thuộc tính enumerable từ 1 hoặc nhiều nguồn , (nguồn trước có thể được ghi đè bởi nguồn sau)

```js
var obj = Object.create(
  { foo: 1 },
  {
    // foo is on obj's prototype chain.
    bar: {
      value: 2, // bar is a non-enumerable property.
    },
    baz: {
      value: 3,
      enumerable: true, // baz is an own enumerable property.
    },
  },
);

var copy = Object.assign({}, obj);
console.log(copy); // { baz: 3 }
```

## Recursion (Đệ quy)

Deep copy array with recursion

```js
const clone = items =>
  items.map(item => (Array.isArray(item) ? clone(item) : item));
```

## Collection

ES6 collection gồm Map, Set, WeakMap, WeakSet

[Readmore](https://viblo.asia/p/es6-collection-map-set-weakmap-weakset-oOVlYqnQl8W)

[Case use](https://codeburst.io/array-vs-set-vs-map-vs-object-real-time-use-cases-in-javascript-es6-47ee3295329b)

## Promise, Async await

> Promise là một cơ chế trong JavaScript giúp bạn thực thi các tác vụ bất đồng bộ mà không rơi vào callback hell

- Luôn đưa vào then 1 function

```js
const promise = new Promise((resolve, reject) => {
  // Note: resolve chỉ cho phép truyền đúng 1 param
  return resolve(27);
});

// Tham số  từ resolve sẽ được chuyển đến then.
promise.then(number => console.log(number)); // 27
```

```js
const add2 = x => x + 2;

//Promise.resolve(4).then(result => add2(result))

Promise.resolve(4).then(add2);
```

> async/await là một cơ chế giúp bạn thực hiện các thao tác bất đồng bộ một cách tuần tự hơn. Async/await vẫn sử dụng Promise ở bên dưới nhưng mã nguồn của bạn (theo một cách nào đó) sẽ trong sáng và dễ theo dõi.

- Để sử dụng, bạn phải khai báo hàm với từ khóa async. Khi đó bên trong hàm bạn có thể dùng await.
- Dùng await sẽ chặn câu lệnh sau nó thực thi

[Readmore](https://ehkoo.com/bai-viet/tat-tan-tat-ve-promise-va-async-await)

## Invoke vs call

- Invoke (Direct/Automatic way) được gọi trực tiếp
- Call (Indirect way) được gọi gián tiếp

## Function vs Method

- Function là một chức năng

Mọi thứ truyền vào Function là tường minh (explicitly), function chỉ có thể sử dụng dữ liệu là chính những parameter truyền cho nó.

- Method là một hành động của đối tượng

Tất cả dữ liệu (các tham số) được truyền (pass) cho method là không tường minh(implicitly). Ngoài những tham số bạn truyền cho method bạn còn truyền theo cả chính object đó

Hoisting
chuyển các khai báo trên đầu (lên đầu tập lệnh hiện tại hoặc hàm hiện tại). trừ let, const, khởi tạo (var x = 5), express function, class
Nên khai báo tất cả biến trên đầu để tránh lỗi
static
Chỉ truy cập trực tiếp bằng this trong method tĩnh
Hoặc truy cập bằng this.constructor.staticMethod () in all
arrow function
không có {} thì 1 lệnh không return
có {} phải return nếu 1 lệnh  
object
object literal | initializer
chỉ tạo được 1 đối tương đơn
không thể khởi tạo đối tượng của mình trừ khi bạn thêm một init()hàm tùy chỉnh
thêm thuộc tính obj.props = props;
công khai
nhanh tiết kiệm bộ nhớ
object constructor ( function )
tạo được nhiều đối tượng với new keyword
thêm thuộc tính sử dụng prototype
bảo mật nhờ closrue

Class
Không nên truyền parameter vào public function in class, có thể trong static
In JavaScript, class inheritance is implemented on top of prototypal inheritance, but that does not mean that it does the same thing:

Classes inherit from classes and create subclass relationships: hierarchical class taxonomies.”
Kế thừa lớp của JavaScript sử dụng chuỗi nguyên mẫu để kết nối `Constructor.prototype` đến cha mẹ` Constructor.prototype` để ủy quyền

problem vs class

Class inheritance creates parent/child object taxonomies as a side-effect.

(class inheritance is the tightest coupling available in oo design), which leads to the next one…

https://stackoverflow.com/questions/2832017/what-is-the-difference-between-loose-coupling-and-tight-coupling-in-the-object-o
The fragile base class problem
Inflexible hierarchy problem (eventually, all evolving hierarchies are wrong for new uses)
The duplication by necessity problem (due to inflexible hierarchies, new use cases are often shoe-horned in by duplicating, rather than adapting existing code)
The Gorilla/banana problem (What you wanted was a banana, but what you got was a gorilla holding the banana, and the entire jungle)

Prototype 

Prototypal Inheritance: A prototype is a working object instance. Objects inherit directly from other objects.
Các thực thể có thể được tạo từ nhiều đối tượng nguồn khác nhau

The fragile base class problem (Tóm lại, lớp cơ sở là lớp bạn đang kế thừa và nó thường được gọi là mong manh vì những thay đổi đối với lớp này có thể có kết quả bất ngờ trong các lớp kế thừa từ nó.)

Nếu có contructor bên trong class con thì cần dùng super() để sử dụng this
// overite , array object to string
