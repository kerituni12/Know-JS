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
