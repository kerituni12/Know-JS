Truthy and Falsy
 
A && B returns the value A if A can be coerced into false; otherwise, it returns B.
A || B returns the value A if A can be coerced into true; otherwise, it returns B.


```js
function isAdministrator(user) {
  return user && user.isAdmin;
}

let user = { isAdmin: 'bcc' };

console.log(isAdministrator(user))      // 'bcc'
console.log(isAdministrator(null))      // null
console.log(!!isAdministrator(null))    // false
```

Use !! to coerced boolean

```js
!!true = !false = true
!!false = !true = false

!!0 = !true = false
!!1 = !false = true
```

Mảng to object 

```js

const keys = ['height', 'width'];
const values = ['12px', '24px'];

// output {  height: "12px",  width: "24px"}

function toObject(names, values) {
    var result = {};
    for (var i = 0; i < names.length; i++)
         result[names[i]] = values[i];
    return result;
}

function toObject(keys, vals, ref) {
  return keys.length === vals.length ? keys.reduce(function(obj, key, index) {
    obj[key] : vals[index];
    return obj;
  }, ref || {}) : null;
}

keys.reduce((obj, key, index) => ({ ...obj, [key]: values[index] }), {});
```
