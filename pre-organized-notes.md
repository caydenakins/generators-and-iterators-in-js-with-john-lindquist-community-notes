# Course Title
## General Notes

The cool thing here is that the generator features of Javascript, they clean up
the syntax and give you a different more incise syntax to work with, it's just
that it's hiding everything behind the ??????.

When you remove the iterator from an array, you can't loop through it. It
becomes 'uniterable', shutting down the ability to loop.

`const abcs = null;`

`const abcs = ["A", "B", "C"];`

Iterator is something you loop through.

You can use the iterator to loop through arrays.

```js
const abcs = ["A", "B", "C"];

const iterator = abcs[Symbol.iterator]();

for (let value of iterator) {
  console.log(value);
}
```

## Code Examples

An iterator is any object that has a symbol, iterator, on it. which is a function.

We can write our own iterators.

```js
const abcs = ["A", "B", "C"];
let i = abcs.length

function next(){
  return {
    value: abcs[--i],
    done: i < 0
  }
}

const iterator = {
  [Symbol.iterator]() {
    return {
      next
    }
  }
}

for (let value of iterator) {
  console.log(value);
}
```

This has the iterator running backwards through the array.

Computer property ex

```js
...
const iterator = {
  [Symbol.iterator]() {
    return {
      next
    };
  }
}
...
```

Symbol.iterator is built in to Javascript, it's a unique key that's global to Javascript.

```js
const abcs = ["A", "B", "C"];

const iterator = abcs[Symbol.iterator]()
```

If you create iterator, make sure to .bind(abcs)

You can call .next on an iterator

### Example
```js
const abcs = ["A", "B", "C"];

console.log(iterator.next());
```

Iterators do not go backwards, they go forwards, so after 3 calls, there is no
meaning.

### Example 2
```js
const abcs = ["A", "B", "C"];

const iterator = abcs[Symbol.iterator]();

for (let value of iterator) {
  console.log(value);
}

for (let value of iterator) {
  console.log(value);
}
```

Although we would expect ABC to output twice, it only happens once because it ties
to iterators. You would have to create a second iterator to make it work.

### Example 2 Fix
```js
const abcs = ["A", "B", "C"];

const iterator = abcs[Symbol.iterator]();
const secondIterator = abcs[Symbol.iterator]();

for (let value of iterator) {
  console.log(value);
}

for (let value of secondIterator) {
  console.log(value);
}
```

### Example 3 (Outputting array without iterator)
```js
const abcs = ["A", "B", "C"];

for(let i = 0; i < abcs.length; i++) {
  console.log(abcs[i]);
}
```

This is different than using iterators, this goes through using the index of the
array.

### Example 3 (Outputting array with )
```js
const abcs = ["A", "B", "C"];
let i = 0

function next() {
  return {
    value: abcs[i++],
    done: i > abcs.length
  }
}

const iterator = {
  [Symbol.iterator](){
    return {next}
  }
}

for(let value of iterator) {
  console.log(value)
}
```

`i < abcs.length` = until true.
`done: i > abcs.length` = while true.

### Reverse Iterator Example
```js
const abcs = ["A", "B", "C"];

const reverseIterator = function(array) {
  let i = array.length;

  function next() {
    return {
      value: array[--i],
      done: i < 0
    };
  }
}

return {
  [Symbol.iterator]() {

  }
}

for (let value of reverseIterator(abcs)) {
  console.log(value);
}
```

### Example ?
```js
const abcs = ["A", "B", "C"];

const iterator = {
  [Symbol.iterator]: function*() {
    yield 1;
  }
};

for (let value of iterator) {
  console.log(value);
}
```

The `*` in `[Symbol.iterator]: function*()` lets us avoid returning an object
with next on it.

### Example ?2
```js
const abcs = ["A", "B", "C"];

const iterator = {
  [Symbol.iterator]: function*() {
    let i = 0
    while (i < abcs.length) {
      yield abcs[i++]
    }
    return;
  }
};

for (let value of iterator) {
  console.log(value);
}
```

When this is done inside of a generator, return won't get hit until done is true.
However, you can break out of it by placing return before the while() loop.

### New Code
```js
function* range(start, end){
  while (start < end) yield start++
}

console.log([...range(0, 100)])
```

Because generators are iterators, we don't have to write `[Symbol.iterator]`, we can use
the function itself as an iterator.

This is asynchronous, if we wanted to delay, we would have to add something

### Example ... Using an Iterator
```js
const iterator = function(start, end) {
  return {[Symbol.iterator]() {
    return {
      next() {

      }
    }
  }}
}
```

Generator functions are unique in the fact that they don't run to the end, so you can think that
in traditional functions, you have `statement`, `expression`, `runs`, until it's `done`. Generator functions
get to the `yield`, then it will stop and wait until it's called again. It can be called
a co-routine, or part of a routine. There's a back and forth between the generator and the thing that's invoking it.

### Pinging
```js
function* ping() {
  const one = yield 1;
  console.log(one);

  const message = yield 2;

  yield message;
}

const iterator = ping();
console.log(iterator.next("one"));
console.log(iterator.next("two"));
console.log(iterator.next("three"));
```

`yield 1;` is sort of the start, run, or the thing that kicks off the program in
the generator.

### User Comment
*Alexis S*: Yield pauses execution of the generator, so when you yield for the
first time, it goes until the first yield and pause.

### Example dwawas
```js
const range = function*(start, end) {
  while (start < end) yield start++;
};

const ranges = function*(count, iteratorFactory) {
  let i = 0;
  while (i < count) {
    i++;
    yield* iteratorFactory();
  }
};

const numbers =
```

### Problem Solving
We can use generators to - next week I'm gonna call this function again, what's
it going to give me.

`yield *` allows you to send out any iterable.

```js
function* gen() {
  yield* [1, 2, 3];
}

console.log([...gen()]);
```

The legacy of generators
TJ's CO; the project is deprecated because async await exists, and we no longer need
to write generators that handle async await.

Generators are not just control flow, they can handle mapping, filtering, and
many other array things.

---

## User Questions

*Is iterator a built in property in arrays?*
- Yes it is built in, see console.log

*The iterator is the same in for, for of, and for in?*

*Will there be a problem with the variable outside the scope?*
- Essentially, once you start writing your own custom iterators, you can start encapsulating
- things and saying `const createIterator = function(array) {}` and returns an object.

*Is this how these iterators actually work under the hood or is this for illustration?*
- Essentially this is the API to create iterators, so I'd assume they use the same one

*I'm under the impression that for loops are faster than this type of iterator.*
- It's the same iterator I'd assume, unless they optimize it somehow.

*Any other data structures you would typically implement an iterator on?*
- It's a step-by-step evaluation of something.

*What's the difference between yield and return?*
-

*With a generator you can use promises?*
- You can return promises, and there are things called async iterators, which are
- a bit more advanced you can look at.

*Are the generators sort of state machines?*
- Yeah, they have

Fernando Balmaceda: *Generator makes a closure or something like that?*
- w

Robert Reed: *Is ES6 spread operator all thanks to iterators?*
- I believe so, they needed that symbol.iterator to enable that so there was a hard-defined
- contract between iterables and what can iterate on them.

David Fontz: *What does it look like if you don’t spread the range call?*
- dwadaw

David Fontz: *So the … spreads all the yields*
- Yes, the ... spreads all the yields, or rather iterates through all the values
- and ?????????

Hrafnkell Palsson: *What’s a practical example of the use for a parameter to the next function?*
-

David Fontz: *Should you have a return in the generator? is that best practice?*
- That's something I need to investigate myself, I'm not sure if once it gets to
- return, there's probably a memory cleanup there, unless it hangs around.
- Because it wouldn't know when to stop until return is called, so it's best practice to leave it ?

Hrafnkell Palsson: *So the generator saves all local state?*
- It's
