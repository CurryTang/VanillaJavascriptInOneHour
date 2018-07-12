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

## Level 3
Before ES6, no class keywords, and we use prototypes to do OOP.

There are several methods to implement inheritance.
```
var Student = {
    name: 'name',
    age: 15,
    ability: function(x) {
        console.log(`${this.name} always studies hard`);
    }
}

function newStudent(name) {
    var s = Object.create(Student);
    s.name = name;
    return s;
}

var shuaige = newStudent('shuaige');
shuaige.ability();
```

![Help to understand prototype](https://cdn.liaoxuefeng.com/cdn/files/attachments/00143529922671163eebb527bc14547ac11363bf186557d000/l)
```
// constructor function

function Boy(name) {
    this.name = name;
    this.hello = function() {
        console.log('Hello, ' + this.name);
    }
}

var cxk = new Boy('cxk');
console.log(cxk.name);
cxk.hello();

// the relationship:
// cxk ----> Student.prototype ----> Object.prototype ----> null

```


Better logic:
![](https://cdn.liaoxuefeng.com/cdn/files/attachments/001435299854512faf32868f60348be878982909b5a5d04000/l)
```
function Student(name) {
    this.name = name;
}

Student.prototype.hello = function () {
    alert('Hello, ' + this.name + '!');
};
```

Implementing inheritance:

![](https://cdn.liaoxuefeng.com/cdn/files/attachments/001439872160923ca15925ec79f4692a98404ddb2ed5503000/l)
```
function Boy(props) {
    this.name = props.name || 'sxc';
}

Boy.prototype.hello = function() {
    console.log(`Hello, ${this.name}`);
}

var cxk = new Boy('cxk');
console.log(cxk.name);
cxk.hello();

// the relationship:
// cxk ----> Student.prototype ----> Object.prototype ----> null

function sillyBoy(props) {
    Boy.call(this, props);
    this.grade = props.grade || 1;
}

function F() {}

F.prototype = Boy.prototype;

sillyBoy.prototype = new F();

sillyBoy.prototype.constructor = sillyBoy;

sillyBoy.prototype.fuckSelf = function() {
    console.log(`${this.name} fucks himself`);
}

sillyBoy.prototype.getGrade = function() {
    return this.grade;
}

var sxc = new sillyBoy({
    name: 'xiaochuan'
})

console.log(sxc.name);
console.log(sxc.getGrade());
```

Inherit function
```
function inherits(Child, Parent) {
    var F = function () {};
    F.prototype = Parent.prototype;
    Child.prototype = new F();
    Child.prototype.constructor = Child;
}
```
Finally, in ES6, you can use class keyword.

## Level 4
The part about DOM is trivial and boring, you can use jQuery to simplify the work. I'll only take down some interesting parts in the tutorial.

Deal with password form
```
<form id="login-form" method="post" onsubmit="return checkForm()">
    <input type="text" id="username" name="username">
    <input type="password" id="input-password">
    <input type="hidden" id="md5-password" name="password">
    <button type="submit">Submit</button>
</form>

<script>
function checkForm() {
    var input_pwd = document.getElementById('input-password');
    var md5_pwd = document.getElementById('md5-password');
    // 把用户输入的明文变为MD5:
    md5_pwd.value = toMD5(input_pwd.value);
    // 继续下一步:
    return true;
}
</script>
```
Note: input form without name property won't be submitted.

## Level 5
We will then talk about async Javascript.

### Ajax

```
function success(text) {
    var textarea = document.getElementById('test-response-text');
    textarea.value = text;
}

function fail(code) {
    var textarea = document.getElementById('test-response-text');
    textarea.value = 'Error code: ' + code;
}

var request = new XMLHttpRequest(); // 新建XMLHttpRequest对象

request.onreadystatechange = function () { // 状态发生变化时，函数被回调
    if (request.readyState === 4) { // 成功完成
        // 判断响应结果:
        if (request.status === 200) {
            // 成功，通过responseText拿到响应的文本:
            return success(request.responseText);
        } else {
            // 失败，根据响应码判断失败原因:
            return fail(request.status);
        }
    } else {
        // HTTP请求还在继续...
    }
}

// 发送请求:
request.open('GET', '/api/categories');
request.send();
```

### Promise
```
function test(resolve, reject) {
    var timeOut = Math.random() * 2;
    console.log(`set timeout ${timeOut} seconds`);
    setTimeout(
        function() {
            if (timeOut < 1) {
                console.log('resolve');
                resolve('200 OK');
            } else {
                console.log('reject');
                reject(`timeout in ${timeOut} seconds`);
            }
        },
        timeOut * 1000
    )
}

var promise = new Promise(test);
promise.then(function(result) {
    console.log(`success ${result}`);
}).catch(function(reason) {
    console.log(`fail ${result}`);
})
```
## Level 6
Final part is about error handling. 
The structure of error handling is try...catch..finally, very similar to other language.
Moreover, you can throw an error using throw new Error(some descriptions).
When dealing with async error, never use something like this

```
try{
  settimeout(something, some time)
}catch{

}
```

you should put try catch block inside async function's logical block
