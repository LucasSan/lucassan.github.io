---
layout: post
title: Shallow Copy and Deep Copy
date: 2020-04-19 13:32:20 +0300
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: i-rest.jpg # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Javascript]
---
## Shallow Copy
It makes a copy of the reference to source object into the target object. Think about it as a copy of the source object address. So, the addresses of the source object and target object will be the same i.e. they will be pointing to the same memory location.

## Deep Copy
It makes a copy of all the members of the source object, allocates different memory locations for the target object and then assigns the copied members to this target object to achieve deep copy. In this way, if the source object vanishes, the target object still valid in the memory.

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

Now if you change target.name, it will only affect target and not sourceObj

![Shallow]({{site.baseurl}}/assets/img/shallow_deep_copy.png)
