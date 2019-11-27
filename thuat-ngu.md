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
