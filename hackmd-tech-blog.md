# Var, Let and Const in Javascript
## Introduction
For those who just get into the world of Javascript, this problem has been a big boss  as a famous interview queston, that is "what is the difference between var、let and const".

Var has been dominating the variable declaration until the publish of ECMA2015(ES6), which provides two new ways to declare a variable, they are "const" and "let". This revision of ECMAScript made Javascript a brand new environment with more flexiability and less error.

Now let's figure out the problem in four perspectives, they are:

1. Initailizer
2. Reassigne/Redeclare
3. Hoisting
4. Function scope/Block scope  

---

## 1.Initializer

"var" and "let" do not require a initializer when they are declared, the default value is "undefined" if no real value is given in the future.

In contrast, a initializer must be given when "const" is declared, otherwise there will be a error like "const is undefined".

## 2.Reassigne and Redeclare
### reassigne
"var" and "let" allow to be reassigned after the variable is declared. The value given for the declaration is a variable, which may change during the code is running. For "const", however, reassign is not allow. A error will merge when "const" is reassigned.

### redeclare
The condition is similar to reassign. The only difference is that "var" can only be redeclared with no error. However, a error will be shown and the code will be excutable if "let" or "const" are redeclared.

## 3.Hoisting
In javascript, variable can be used before it is declared. In other word, we can declare a variable after use it. This only happens with "var", which resulting a "undefined" instead of a error. Hoisting is a special situation that only happens in Javascript. This is because Javascript runs the code im two steps, "creation" and "excution", respectively. "Creation" refers to creating variables and initializing them. In "excution" in which value will be assigned or code will be excuted. These two phases allow "var" can be used befored it is declared without any error. 

However. hoisting does not work for "let" and "const" after ES6. Specifically, hoisting will also applied on "const" and "let" but it is just dysfunctional and resulting a error. This is due to TDZ(temperal dead zone) which can be considered a box that covering and preventing "const" and "let" from being accessed during the phase of Creation. Since Javascript can not access the variable in creation phase, value can not be assigned in next phase which is Excution and cause a error eventually.

## 4.Scope
The last but not the least, the scope in hich the variable is declared is vital and can significantly affect how the code will be excuted. For example, if a variable is declared locally and invoked outside the scope, nothing will show up but "variable is not defined". We don't want this happen, so it is important to declare a variable in correct scope.

"Var" is functional scoped, that means it will only be a local variable when it is declared inside a "function" scope. Otherwise, "var" is global when declaration is outside a function and might cause global pollution in further. Once the "var" is inside a function scope, it can not be accessed outside the scope. However, "var" can be invoked anywhere as long as it is global. 

On the other hand, "let" and "const" are block scoped, a block refers to the statements like "if/else" or "for", including "function", with a {} following at behind. And the situation is similar to "var" in function. "let" and "const" are local variable only the declaration is inside a block. Addtionally, block can not constraint "var", "var" is still a global variable even it is declared inside a block.

## Conclusion
I know you may be still confused after all of these, so let me make a table to make everything more understandable.

|                | var | let | const |
| -------------- | ---:| --- |:-----:|
| initializer    |  no | no  |  yes  |
| reassign       | yes | yes |  no   |
| redeclare      | yes | no  |  no   |
| hoisting       | yes | no  |  no   |
| function scope | yes | yes |  yes  |
| block scope    |  no | yes |  yes  |


---


### Reference
1. https://www.freecodecamp.org/news/var-let-and-const-whats-the-difference/
2. https://www.w3schools.com/js/js_hoisting.asp
3. https://www.freecodecamp.org/news/var-let-and-const-whats-the-difference/
4. https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let
5. https://ithelp.ithome.com.tw/articles/10295517?sc=iThelpR
6. https://jameshsu0407.github.io/blog/20211013_var_let_const/
