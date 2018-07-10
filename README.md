# VanillaJavascriptInOneHour

> What is this repository about? 

It's my note on studying javascript, and can also be used as a very quick intro to vanilla javascript(most contents won't cover ES6 and some other advanced techniques).

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
