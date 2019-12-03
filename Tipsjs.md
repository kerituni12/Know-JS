Use !! to coerced boolean if use Truthy and Falsy

```js

function isAdministrator(user) {
  return user && user.isAdmin;
}

let user = { isAdmin: 'bcc' };

console.log(!!isAdministrator(null))

if (isAdministrator(user)) {
  // ...
}


!!true = !false = true
!!false = !true = false

!!0 = !true = false
!!1 = !false = true
```
