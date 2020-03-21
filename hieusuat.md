Tránh thay đổi __proto__ thay vào đó sử dụng Object.create()

var fn = function () {};
fn.prototype.myname = function () {
    console.log('myname');
};

// Bad
var obj = {
    __proto__: fn.prototype
};

// Good 
var newObject = Object.create(fn.prototype);
