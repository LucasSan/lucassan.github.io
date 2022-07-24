---
layout: post
title: The One with Shallow Copy and Deep Copy
date: 2020-04-19 13:32:20 +0300
description: An introduction about Shallow Copy and Deep Copy # Add post description (optional)
img: i-rest.jpg # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Javascript]
---

Have you ever wonder how objects are cloned or copied ?

_[I'll assume that you said no]_

So, let me introduce you how a Shallow Copy and a Deep Copy works and the difference between them

_[Drumrolls begin]_

## Shallow Copy

It basically get the reference from the source object and put into the target object.
Think about it as a copy of the source object address. So, the addresses of the source object and target object will be the same i.e. they will be pointing to the same memory location.

## Deep Copy

It basically makes a copy of all the members of the source object, allocates different memory locations for the target object and then assigns the copied members to this target object to achieve deep copy. In this way, if the source object vanishes, the target object still valid in the memory.

### Example Shallow Copy:

```
const sourceObj = {
    name: ‘Lucas’,
    age: 31,
};
const target = sourceObj;
```

If we change a value:

`sourceObj.name = ‘Amanda’;`

This statement will also change name from target, since we have a shallow copy, or a reference to sourceObj. This means, you’re losing the original data as well.
But, creating a brand new variable by using the properties from the original sourceObj variable, you can create a deep copy.

```
const target = {
    name: sourceObj.name,
    age: sourceObj.age,
};
```

Now if you change target.name, it will only affect target and not sourceObj.

![Shallow]({{site.baseurl}}/assets/img/shallow_deep_copy.png)

But I think we can go further in this explanation if we think about primitives types in Javascript.

What are the primitive types in Javascript !?

Well we can say that primitive types in Javascript are

- `number`
- `string`
- `boolean`
- `null`
- `undefined`
- `Symbol`

All the others data types in Javascript are objects, like

- `Objects`
- `Functions`
- `Arrays`
- `Regex`

Two rules over here:

1. Primitives types are passed by value, and are immutable.
2. Object are passed by reference, and are mutables.

So, if you make something like

```
const object1 = {}
const object2 = {}
console.log(object1 === object2) // false
```

this is happening because when you create a constant with `{}`, this will make Javascript allocate a piece of memory to it. So object1 has a piece of memory, and object2 has another piece of memory, so they are two different objects, and then when you ask if object1 is equal to object2, the answer is false.

And now, if you make something like

```
const object1 = {}
const object2 = object1
console.log(object1 === object2) // true
```

this one will be true because object2 has a reference to object1 memory address (like we saw in Shallow Copy).

_Any questions, opinions or insults !? ;)_
