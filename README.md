# VanillaJavascriptInOneHour

> What is this repository about? 

It's my note on studying javascript based on LiaoXueFeng's tutorial, and can also be used as a very quick intro to vanilla javascript(most contents won't cover ES6 and some other advanced techniques). 

## Level 0
```
var x = 0;
y = 1;
```
Both is OK. However, don't use the latter one. y will be regarded as a global variable. Using 'use strict' on the first line.

x = 123;
y = 123.1;
x y both Number type.

Never use == ! Use === instead.
```
NaN === NaN
```
Be aware of NaN(Not a number), NaN === anything will return false. 

0.1 + 0.2 === 0.3 will return false, due to the way floats are represented in computers.

Use null rather than undefined at most times, it's another silly design in JS. 

```
var x = [1,2,3,4,5,6];
```
The way to initialized an array, don't use new Array();

```
var obj = {
  a:1,
  b:'abc'
};
```
The way to init an object.

```
var multi = `so
many
lines`;
console.log(multi);
```
The way to get a multi-line string conveniently. Note: \` not '

```
var hello = 'Don\'t say hello';
var world = 'world';
var s = `${hello} ${world}`;
console.log(s);
```
Formatted string like the one in C or Python.

No need to remember string api functions, they're so similar to the ones in other languages.

The loop utility in js is very similar to other languages. Be aware of the for ... in ... loop
```
var obj = {
    nice: 1,
    shuai: 'ge'
};

for (var a in obj) {
    if (obj.hasOwnProperty(a)) {
        var outputInfo = `The object obj has property ${a} and its value should be ${obj[a]}`;
        console.log(outputInfo);
    }
}
```
```
var a = ['A', 'B', 'C'];
for (var i in a) {
    console.log(i); // '0', '1', '2'
    console.log(a[i]); // 'A', 'B', 'C'
}
```
You should note that the index is the type string rather than number. You should better use for...of and forEach instead of for...in. for...of can be applicable to type of iterable(Map, Set and Array)

``` 
var arr = ['a', 'b', 'c'];
for (var a of arr) {
    console.log(a);
}
arr.forEach(function(value, index, array) {
    console.log(value);
    console.log(index);
});
```
## Level 1

The most common way to define a function in Javascript
```
var factorial = function(x) {
    if (x <= 1) {
        return 1;
    }
    return x * factorial(x - 1);
}

console.log(factorial(3));
```

You should notice that Javascript set completely no restrictions on arguments, so if you pass the following to factorial
* nothing ------> you get NaN, x equals undefined
* 10, Nan -------> the function works normally

Javascript keeps an array-like structure called arguments internally in a function. You can get the nth element by using 
arguments[n - 1].

You can use arguments to do something like the default parameters in other language.
```
function foo(a, b, c) {
    if (arguments.length === 2) {
        c = b;
        b = some default value;
    }
}
```

In ES6, there's a better way to deal with unknown numbers of parameters by using ...rest.

About name scope, we need to point out a key point. Looking at the following code, 
```
'use strict';

function foo() {
    var x = 'Hello, ' + y;
    console.log(x);
    var y = 'Bob';
}

foo();
```
There will be no error indeed, while the value of y printed out would be undefined. 
We should always declare and assign values in the very beginning of a function.

A better way to use global variables
```
var NameScope = {};

NameScope.x = 1;
NameScope.y = 2;
NameScope.func = function(x, y) {
    return x + y;
}

console.log(NameScope.func(NameScope.x, NameScope.y));
```
In ES6, you can use let to make a blocked name scope, and use const to declare a constant.

To specify an object that your function will work on:
Use function.apply(object, array)
or function.call(object, bunches of elements)

decorator in JS:
```
'use strict';

var count = 0;
var oldParseInt = parseInt; 
window.parseInt = function () {
    count += 1;
    return oldParseInt.apply(null, arguments); 
};
```

Some examples using higher-order functions
Deleting elements appearing multiple times
```
'use strict';

var
    r,
    arr = ['apple', 'strawberry', 'banana', 'pear', 'apple', 'orange', 'orange', 'strawberry'];
r = arr.filter(function (element, index, self) {
    return self.indexOf(element) === index;
});
```

Closure is very very important in Javascript, for advanced usage, you should google yourself.
```
function outer() {
    var state = 0;
    var inner = function() {
        state = state + 1;
        console.log(state);
    }
    return inner;
}

var closure = outer();
closure();
closure();
```

You'd better not refer to variables appeared in loops when using closure, or you will have some troubles.
```
function count() {
    var arr = [];
    for (var i=1; i<=3; i++) {
        arr.push((function (n) {
            return function () {
                return n * n;
            }
        })(i));
    }
    return arr;
}

var results = count();
var f1 = results[0];
var f2 = results[1];
var f3 = results[2];

f1(); // 1
f2(); // 4
f3(); // 9
```

In ES6, you can use arrow functions rather than anonymous function, it's smarter when dealing with name scopes.

```
var arr = [1, 3, 2];
arr.sort((x, y) => {
    if (x > y) {
        return 1;
    } else {
        return -1;
    }
});
console.log(arr);
```

In ES6, you can use generator, which is nearly the same the one in Python
```
function* fib(max) {
    var
        t,
        a = 0,
        b = 1,
        n = 0;
    while (n < max) {
        yield a;
        [a, b] = [b, a + b];
        n ++;
    }
    return;
}
```
