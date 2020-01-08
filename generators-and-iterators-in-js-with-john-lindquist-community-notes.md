# Generators and Iterators in JavaScript with John Lindquist
## General Notes

An iterator is something you loop through, you can use it to loop through
arrays.

The generator features of Javascript are cool because they clean up syntax and
give you a different, more precise syntax to work with - it hides
everything behind the inner workings.

You can use iterators to loop through arrays. But, when you remove the iterator
from an array, you can't loop through it. The array becomes 'uniterable',
shutting down its ability to loop.

Generators are not just control flow, they can handle mapping, filtering, and
many other array things. A huge benefit of generators is they give us a chance
to pass something in and do logic based on it.

Problem solving/real life examples generators could be like if you want an event
to run every week. You can set up a generator which you can call `next()` every
single day and make the generator schedule something every Thursday.

The legacy of generators - [tj/co](https://github.com/tj/co).
The project is deprecated because async/await exists, and we no longer need
to write generators that handle async/await.

## Code Examples

### Example 1 (Iterating through Arrays using for-each)
```js
const abcs = ["A", "B", "C"];

const iterator = abcs[Symbol.iterator]();

for (let value of iterator) {
  console.log(value);
}
```

An iterator is any object that has a symbol, iterator, on it. which is a
function. We can write our own iterators.

### Example 2 (Iterating through Arrays)
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

This example above has the iterator running backwards through the array.

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

`Symbol.iterator` is built in to Javascript, it's a unique key that's global to
Javascript. It specifies the default iterator for an object.

```js
const abcs = ["A", "B", "C"];

const iterator = abcs[Symbol.iterator]()
```

Alternative way to write the iterator. It's worth noting you can use `.next()`
on an iterator.

### Example 3 (Iterator Direction)
```js
const abcs = ["A", "B", "C"];

console.log(iterator.next());
```

Iterators do not go backwards, they go forwards. So, in the code above after 3
calls, there is no meaning.

### Example 4
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

Although we would expect 'ABC' to output twice, it only happens once because it
ties to iterators. You would have to create a second iterator to make it work.

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

Because we created two iterators in the updated code, we would have 'ABC'
output twice.

```js
const abcs = ["A", "B", "C"];

for(let i = 0; i < abcs.length; i++) {
  console.log(abcs[i]);
}
```

We don't have to use iterators to output arrays. This is approach is different,
it goes through using the index of the array.

### Example 5
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

### Example 6 (Reverse Iterator)
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

### Example 7 (Functions/Generators)
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

When this is done inside of a generator, return won't get hit until done is
true. However, you can break out of it by placing return before the `while()`
loop.

### Example 8 (Generators)
```js
function* range(start, end){
  while (start < end) yield start++
}

console.log([...range(0, 100)])
```

Because generators are iterators, we don't have to write `[Symbol.iterator]`,
we can use the function itself as an iterator. This is asynchronous, if we
wanted to delay, we would have to add more.

### Example 9
```js
const iterator = function(start, end) {
  return {[Symbol.iterator]() {
    return {
      next() {
        return {
          value: start++,
          done: start > end
        }
      }
    }
  }}
}

function* range(start, end){
  while (start < end) yield start++
}

console.log([...range(0, 100)])
```

This is what we can do with a generator function. It's hiding things for us, but
it's also extending it and giving us the ability to do things asynchronously.

Generator functions are unique in the fact that they don't run to the end. You
can think of traditional functions where you have `statement`, `expression`,
`runs`, until it's `done`. Generator functions get to the `yield`, then stop
and wait until it's called again. It can be called a `co-routine`, or part of a
routine. There's a back and forth between the generator and the thing that's
invoking it.

### Example 10 (Pinging)
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

`yield 1;` is sort of the start, run, or the thing that kicks off the program
in the generator.

**User Comment** - *Alexis S*: Yield pauses execution of the generator, so when
you yield for the first time, it goes until the first yield and pause.

### Example 11 (Yield)
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

const numbers = [...ranges(3, () => range(0, 10))];

console.log(numbers);
```

`yield *` allows you to send out any iterable.

### Example 12
```js
function* gen() {
  yield* [1, 2, 3];
}

console.log([...gen()]);
```

---

## User Questions

Anonymous: *Is iterator a built in property in arrays?*
- Yes, iterator is built in to all arrays, it is on the array prototype. See
console.log examples: `console.log(Array.prototype[Symbol.iterator])`.

Fernando Balmaceda: *The iterator is the same in for, for of, and for in?*
- Yes, the iterators are all the same in those.

Anonymous: *Will there be a problem with the variable outside the scope?*
- Essentially, once you start writing your own custom iterators, you can start
encapsulating things and saying `const createIterator = function(array) {}` and
returns an object.

Jordan Kline: *Is this how these iterators actually work under the hood or is
this for illustration?*
- Essentially, I don't know how the library that drives v8 is implemented. But,
these are the contracts that they expose for you to work with. This is the API
to create iterators, so I'd assume they use the same one internally, but they
could be doing something different.

Hrafnkell Palsson: *I'm under the impression that for loops are faster than this
type of iterator.*
- It's the same iterator I'd assume, unless they optimize it somehow.

*Any other data structures you would typically implement an iterator on?*
- Well, it's a step-by-step evaluation of something. The generator features of
Javascript clean up the syntax and give you a more incise syntax to work with,
it just hides everything that's going behind `yield` and the `start` function.

Anonymous: *What's the difference between yield and return?*
- The difference is in iterators, you'll often see `while()` loops and checking
if things have reached a certain length and then going about their business.

Fernando Balmaceda: *With a generator you can use promises?*
- You can return promises, and there are things called async iterators, which
are a bit more advanced you can look at.

Hrafnkell Palsson: *Are the iterators sort of state machines?*
- Yeah, they have internal state. And they have a context and as requested,
give you back what you requested. It allows you to write all sorts of funky
state machines.

Robert Reed: *Is ES6 spread operator all thanks to iterators?*
- I believe so, they needed that symbol.iterator to enable that so there was a
hard-defined contract between iterables and what can iterate on them.

David Fontz: *So the … spreads all the yields*
- Yes, the `...` spreads all the yields, that's a way of saying it. Or rather,
it iterates through it and grabs every value your generator is pushing out.
  - _Learner's Advocate Note_ - Example with `...`:
  `console.log([...range(0, 100)])`

David Fontz: *Should you have a return in the generator? is that best
practice?*
- That's something I need to investigate myself, I'm not sure if once it gets
to return, there's probably a memory cleanup there, otherwise it hangs around.
Because it wouldn't know when to stop until return is called, so it's best
practice to leave it in.

Hrafnkell Palsson: *So the generator saves all local state?*
- _Learner's Advocate Note_ - Left unanswered, but found that `There's no longer
a need to create an object to hold the iteration state—generators automatically
save their local state every time they yield.` via
[this](https://eloquentjavascript.net/11_async.html) website.
