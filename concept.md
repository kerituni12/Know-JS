# Know-JS
summary concept in javascript 

## Table of Contents

1. [Closure](#closure)
1. [Scope](#scope)

**[⬆ back to top](#table-of-contents)**

## Closure

 > Closure is the ability of a function to remember and continue to access variables defined outside its scope, even when that function is executed in a different scope and outer function has returned
 
```js
function greeting(msg) {   
    return {
      name: function(name) {
        console.log(`${msg}, ${name}!`);
      },
      age: function(age) {
        console.log(`${msg}, ${age}!`);
      }
    }
}

var hello = greeting("Hello");
var howdy = greeting("Howdy");

hello.name('a'); //the same env with hello.age()
hello.age('20'); //the same env with hello.name()

howdy.name('b'); //other env
```
> outer funtion has returned
```js
function counter(step = 1) {
    var count = 0;
    return function increaseCount(){
        count = count + step;
        return count;
    };
}

var incBy3 = counter(3);

console.log(incBy3());  //3   
console.log(incBy3());  //6
console.log(incBy3());  //9

```
