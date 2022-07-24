---
layout: post
title: The One with Var, Let and Const
date: 2020-04-29 21:15:20 +0300
description: Quick overview about vars, let and const
img: software.jpg # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Javascript]
---

_[May I have your attention please]_

Before we start to talk (or write) about the difference between Var, Let and Const, we need to know two main concepts called Scope and Block, and to explain this I need your full atention please.

## So, what is Scope ?

Scope means where a declared variable is available for use.

There it is! I told ya, very complicated isn't !? ;)

## What about Block ?

A block is chunk of code bounded by curly braces

Easy like that!

So, let's talk (or write) about declaration of variables

## VAR

_[This one is not about the Video Assistant Referee]_

So, what is the scope of var ?

`var` can be **Globally Scoped** or **Function Scoped**.

**Globally Scoped** when declared outside of a function, i.e., we can access this variable in the window. (don't worry, we gonna see it later)

**Funcion Scoped** when declared inside a function, i.e., we can access this variable only inside the function

Lets take a look at the code bellow

```
var name = "lucas"

function foo() {
	var hey = "hey"
}

console.log(window.name) // "Lucas"
console.log(window.hey) // undefined
```

as I said before, the variable declared as name, is **Globally Scoped** and can be seen through the `window` as you can see in the console.log

## LET

let is block scoped, i.e. lives inside curly braces. So a variable declared using let, inside a block with the let is only available for use inside that block.

Let's take a look at the code bellow

```
let name = "lucas";

if (true) {
	let hey = "hey"
	console.log(hey) // "hey"
}
console.log(hey) // hey is not defined
```

We see that using hey outside its block (the curly braces where it was defined) returns an error. This is because let variables are block scoped.

## CONST

const are block scoped and cant be updated

When you set a value to a variable declared with const, it can't be updated, and will remain the same inside its scope.

```
const hey = "Hey you!"
hey = "ops I cant do that" // error : Assignment to constant variable.
```

But, how it works when the const variable has an object ?

Well, the const itself can't be updated, but the properties of this object can!

Let's see it

```
const user = {
	name : "lucas",
	city: "Porto"
}

user.name = "lucas germano";
```

So, any questions, opinions, insults !? ;)
