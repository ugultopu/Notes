When you add a dependency using Yarn (`yarn add packageName`) without specifying a version (that is, no `packageName@version`), Yarn installs the latest stable version of that package and it uses the caret SemVer range in the `package.json` file. That is, instead of declaring an exact version of the dependency in `package.json`, it puts that latest stable version with a caret at the beginning of it. Example:

```shell
yarn add react
```
Currently, the latest stable version of React is 16.13.1. Hence, Yarn will install that version and it will add `"react": "^16.13.1"` in the `dependencies` property in `package.json`.

----

Async/Await are built on top of Promises
========================================

One important difference between them and the good old callbacks like the event handlers (onClick, etc.) and functions registered to run via `setTimeout` etc. are; Async/Await and Promises are put into "ES6 Job Queue" whereas the good old callbacks are put into the "Message Queue".

The difference between these queues is that the next element in the Message Queue is executed only when the Call Stack is completely cleared. That is, imagine a function that calls another function which calls another recursive function, etc. This is a lot of function calls that will grow the stack. An element (the next element) in the Message Queue will be executed only after all of these recursive function calls and nested function call have been returned.

On the other hand, an element (the next element) in the Job Queue will be executed as soon the current frame in the call stack is returned. That is, the call stack might still have lots of other frames (that is, not-yet-completed function calls). However, an element (the next element) in the Job Queue will be executed right after the current call frame is returned.

In short, asynchronous things that become ready are placed into either the Message Queue, or into the (ES6) Job Queue, depending on their type (Async/Await - Promises vs. the good old callbacks). Note that they are placed into the relevant queue _only after they are ready_. That is, for example when we call `setTimeout(someFunction, 3500)`, `someFunction` is not placed into the Message Queue immediately. When we call `setTimeout(someFunction, 3500)` (that is, as soon as we call it), a timer is started and when that timer expires (in this case, after 3500 milliseconds), `someFunction` is placed into the Message Queue.

As a side note, `setImmediate(someCallbackFunction)` is [equivalent to][setImmediate is equivalent to setTimeout] `setTimeout(someCallbackFunction, 0)`, which is equivalent to `setTimeout(someCallbackFunction)`.

Note that Promises (ES6/ES2015) and Async/Await (which are built on top of Promises starting with ES2017/ES8) are an alternative (a replacement) to the good old callbacks ([Source](https://nodejs.dev/learn/javascript-asynchronous-programming-and-callbacks#the-problem-with-callbacks)).

## Sources
[NodeJS Docs](https://nodejs.dev/learn/the-nodejs-event-loop#es6-job-queue)

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop)

[setImmediate is equivalent to setTimeout]: https://nodejs.dev/learn/discover-javascript-timers

----

`highWaterMark` property in Node.JS streams is nothing but an advisory. That is, it is a declaration that makes the `yourStream.write()` to return `false` (instead of returning `true`) after the (data) buffer in `yourStream` has `highWaterMark` elements accumulated. In other words, `yourStream.write()` will always write to the buffer, even when it returns `false`, since the buffer does not have a fixed size but it is infinite. However, this might cause excessive memory usage and eventually might cause Node.JS to run out of memory.

In other words, `highWaterMark` is nothing but a warning, an advisory that you set yourself to prevent shooting yourself in the foot. You can very well continue writing in a stream even after the buffer has more elements than `highWaterMark` number of elements and the written elements will not be _lost_. They will, indeed, be properly written to the buffer. However then we are running the risk of using excessive memory and causing Node.JS to run out of memory, etc. From the [Stream API docs in Node.JS](https://nodejs.org/api/stream.html#stream_buffering):

> The `highWaterMark` option is a threshold, not a limit: it dictates the amount of data that a stream buffers before it stops asking for more data. It does not enforce a strict memory limitation in general. Specific stream implementations may choose to enforce stricter limits but doing so is optional.

[Source](https://stackoverflow.com/questions/35801568/stream-highwatermark-misunderstanding)
