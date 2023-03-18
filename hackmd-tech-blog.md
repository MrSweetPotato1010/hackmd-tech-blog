# Who is ```this``` in JS?(With a lot of examples)
###### tags: `Javascript`
## Introduction 
This article aims to explain the keyword "this" in Javascript by introducing some inportant rules to identify who is ```this``` referencing. Additionally, I will go throught numerous examples to explain ```this``` complicated concept instead of discusssing the theory.

Before we begin, there is a core concept you need to keep in your mind all the time about ```this``` in Javascript. That is, **"In general, ```this``` is the object that ia executing the current function. Where is ```this``` dose not matter, how we call it does"**

## Start up
Let's start with two simple examples to explain the statement above, please see the code below
EX1
```javascript=
function play(){
    console.log(this)
}
play()//window(global)
```
Here we see if a funtion is a regular function, then ```this``` references the window(object) in browser or global(object) in node and this is called **Default Binding**.


Another example below
EX2
```javascript=
const video = {
    title = "a"
    play: function(){
        console.log(this)
    }
}

video.stop = function(){
    console.log(this)
}

video.play()// video
play()// window(global)
video.stop()// video
stop()// window(global)
```
In rows 1 to 6, if a function is in a object, then it is called method. And if a method is invoked with the object at front(row 12), then ```this``` here is the object which is ```video```.

In row 8, it seems a regular function but it's actually not. Instead, it's another method ```stop``` also in ```video```, therefore, if it has video at front then ```this``` here is also video just like previous one. Beside, this special this is Implicit Binding.

Additionally, if method is invoked with nothibg at front, it's actually like ```window.method()```. Thus,  ```this``` here is window(global) which is Default binding. 

Now we can conclude the first two main rules:

***Whoever(at front) calls the function/method is ```this```.**

***If no one call the function/method, then ```this``` references window in browser(global in node).**

These two rules can solve most of the problem about ```this``` because they are the core of this article.

## Constructor
* When a function is called with a ```new```  at front, then a  new object is created.
* The new object will be binded by ```this```. that is ```this``` will point to the new object.
* Unless the function specifies a substitute object to return, the new object created throught ```new``` will be returned.

Let's see a example:
EX3
```javascript=
function play(a){
    this.a = a
}

var obj = new play(123)
console.log(obj.a)// 123
console.log(obj)// play{a : 123}
```
Here we see a new object ```obj``` is created with ```new```, and ```obj``` has a variable ```a = 123``` which ```this``` is pointing to. Therefore, row 6 prints ```123```.

Now try a harder one:
EX4
```javascript=
function obj(a){
    this.a = a
    this.sayNum =function(){
        console.log("the number is " + this.a)
    }
}
var a = 1
var newNum = new obj(2)
newNum.sayNum()//the number is 2
```
Similarly, ```new```creates a new object newNum and force ```this```point to the value given which is 2. Hence row  9 still prints ```2``` even there is a global variable ```a = 1```.

## Arrow function
Arrow functions do not bind their own ```this```, instead, they inherit the one from the parent scope, which is called "lexical scoping". This makes arrow functions to be a great choice in some scenarios but a very bad one in others

If we look at the first example but using arrow functions
EX5
```javascript=
const play = () =>{
    console.log(this)
}
play()/// window
```
What can we expect ```this``` to be?.... exactly, same as with normal functions, window or global object. Same result but not the same reason. With normal functions the scoped is bound to the global one by default, arrows functions, as I said before, do not have their own ```this``` but they inherit it from the parent scope, in this case the global one.

What would happen if we add "use strict"? Nothing, it will be the same result, since the scope comes from the parent one.

What if the arrow function is a method?
EX6
```javascript=
const video = {
    play : () =>{
    console.log(this)
    }
}

video.play()// window
const stop = video.play
stop()// window
```
Weird right? Well, remember, arrow functions don't bind their own scope, but inherit it from the parent one, which in this case is window or the global object.

Another example
EX7
```javascript=
const myObject = {
    myArrowFunction: null,
    myMethod: function () {
      this.myArrowFunction = () => { console.log(this) };
    }
  };
myObject.myMethod() // null

myObject.myArrowFunction() // this === myObject

const myArrowFunction123 = myObject.myArrowFunction;
myArrowFunction123() // this === myObject
```
When we call ```myObject.myMethod()```, we initialize ```myObject.myArrowFunction``` with an arrow function which is inside of the method ```myMethod```, so it will inherit its scope. We can clearly see a perfect use case, closures.

## Explicit Binding
Usually ```this```has a default value like window or undefined, but we can still change the value with some keyword. They are ```apply``` ```call``` ```bind```. 
EX8
```javascript=
function hello(a, b){
  console.log(this, a, b)
}

hello(1, 2) // window 1 2
hello.call(window, 1, 2) // window 1 2
hello.apply(window, [1, 2]) // window 1 2
hello.call(a, 1, 2) // Number{10} 1 2
hello.apply(a, [1, 2]) // Number{10} 1 2
hello.call("a", 1, 2) // String{'a'} 1 2
hello.apply("a", [1, 2]) // String{'a'} 1 2

```
There is a function ```hello```with three parameters. We can see the results of rows 5 to 7 are same. But, why row 5 is still showing ```window```even the first parameter is gone? Because we didn't assign the value of the first parameter, ```this```is window by default. And when we use the keywords ```apply``` and ```call```, ```this```will be reassigned with the value inputted, that is ```window``` in rows 6 and 7. 

```apply``` and ```call``` are almost the same, the only difference is the way to input argument. ```call``` requires string and array for ```apply```.

Another keyword is ```bind```, unlike the previous two, ```bind``` return a new function.
EX9
```javascript=
var obj = {
  x: 123
};

var func = function () {
  console.log(this.x);
};

func();            // undefined
func.bind(obj)();  // 123
```
Since ```x``` is undefinded is function ```func``` but in  object ```obj```, ```this.x``` resulting ```undefinded```. And hence we bind ```this``` with ```obj``` so that he can sccess the variable ```x``` and print ```123```.

## Strict Mode
Strict mode is a very simple concept. Javascript is not a strict language, sometime ut doesn't return a error even it's suppose to. In EX1, since no one call the function ```play```, the broswer assigned ```window``` to do the job by default and ```this``` references ```window``` as a result. But in strict mode, it will return ```undefined```. 
EX10
```javascript=
"use strict"
function play(){
  console.log(this)
}
play()// undefined
```
So the rule is that all the ```window``` that ```this```is referencing will convert to ```undefined``` if strict mode is on.

## Practice Examples
EX11
```javascript=
function a(){
  function b(){
    console.log(this);
    function c(){
      "use strict"
      console.log(this);
    }
    c()// undefined
  }
  b()// window
}
a() 
```
Since strict mode only applies on function c, ```this``` in function c is undefined. And ```this```in function b is default binding which is ```window```.

---
EX12
```javascript=
var a = 1

function num(){
  console.log(this.a);
}

var set = {
  a : 2,
  detail : function(){
    console.log(this.a);
  },
  set2 : {
    a : 3,
    detail: function(){
      console.log(this.a)
    },
  },
  num: num
}
set.detail() // 2
set.set2.detail() //3
set.num() //2
```
```set.detail()``` : ```this``` references ```set```, so ```a``` is 2.

```set.set2.detail()``` : ```this``` references ```set2```, so ```a``` is 3.

```set.num()``` : Because ```num``` is a attribute of a function in ```set```, function is actually inside ```set``` even it seem located outsie. Therefore, ```this``` references object ```set```, so ```a``` is 2 as well.

---


EX13
```javascript=
var num = 1
function a(){
  var num = 2
  console.log(this.num);
}
function d(i){
  return(i)
}
var b = {
  num: 3,
  detail: function(){
    console.log(this.num);
  },
  obj: function () {
    return function() {
      console.log(this.num);
    }   
  }
}
var c = b.detail
b.a = a
var e = b.obj()
a() //1
c() //1
b.a() //3
d(b.detail)//1
e() //1
```
```a()```, ```c()``` and ```e()```: These function are called with no one at front, so ```this```  all referecnce ```window``` and ```num```are 1. 

```b.a()```In row 21, we declare ```a``` as a method of ```b```, so ```this``` references object ```b``` and result is 3.

```d(b.detail)```: ```b.detail``` is a function called by ```d()```, so ```this``` also references ```window``` just like a regular function. 

---

## Conclusion
* ```this``` references whoever call the function.
* If no one call the function, then ```this```references window by default.
* ```new```will creat a new object and make ```this```point to thaat object.
* ```apply, call and bind``` can change the object that ```this``` is referencing.
* In strict mode, ```window```will convert to ```undefined```.
* Arrow functions do not bind their own ```this```, instead, they inherit the one from the parent scope, which is called “lexical scoping”.
 
## Reference
* https://kuro.tw/posts/2017/10/17/What-s-THIS-in-JavaScript-%E4%B8%AD/
* https://www.youtube.com/watch?v=RmxUK
* https://www.youtube.com/watch?v=gvicrj31JOM
* https://www.casper.tw/javascript/2019/03/21/this-why-window/
* https://blog.techbridge.cc/2019/02/23/javascript-this/