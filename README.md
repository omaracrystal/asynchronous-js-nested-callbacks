# Understanding Asynchronous Code

Asynchronous programming can quickly become complicated. Nested callbacks and
anonymous inline functions can make debugging and reading the call stack
challenging.  One tool to have in your arsenal is a solid visualization of JavaScript's Event Loop.

## Setup

Fork and clone this repository. When you write answers to questions be sure to push.

## Objectives

By the end of this lesson you should be able to:

- Describe what's happening with a given snippet of async code using language like "stack", "heap", and "queue"
- Identify the order in which code will be executed
- Write multiple nested async calls in succession

## Set The Stage

_Why?_ Understanding JavaScript's asynchronous capability (and how to use it to
your advantage) forms the foundation of the vast majority of both Node and Browser programming. Imagine if your
application had to wait for one event to finish before it could start another.
What kind of problems could that lead to? What would the user experience be
like?

The "what" of this exercise is that you'll do a bunch of searching and reading
to first visualize how the event loop runs. Then, you'll practice evaluating
code and creating your own visualizations.

Let's get started!  Take as much time as you need, and come up with your own exercises to complement the ones here.

Don't forget to close other browser tabs / Atom windows / Terminal tabs.

## Activities

**Learn up!**

First, watch the video at [https://vimeo.com/134061121](https://vimeo.com/134061121) - password is `schoolhouserock`

Now read through the MDN docs at [https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop) and see if they make sense.

This is a **great** video as well [https://www.youtube.com/watch?v=8aGhZQkoFbQ](https://www.youtube.com/watch?v=8aGhZQkoFbQ)

And here is a link to the excellent tool he built for visualizing the event loop. Play around with it a little and watch how your code is executed: [latentflip.com](http://latentflip.com/loupe/?code=JC5vbignYnV0dG9uJywgJ2NsaWNrJywgZnVuY3Rpb24gb25DbGljaygpIHsKICAgIHNldFRpbWVvdXQoZnVuY3Rpb24gdGltZXIoKSB7CiAgICAgICAgY29uc29sZS5sb2coJ1lvdSBjbGlja2VkIHRoZSBidXR0b24hJyk7ICAgIAogICAgfSwgMjAwMCk7Cn0pOwoKY29uc29sZS5sb2coIkhpISIpOwoKc2V0VGltZW91dChmdW5jdGlvbiB0aW1lb3V0KCkgewogICAgY29uc29sZS5sb2coIkNsaWNrIHRoZSBidXR0b24hIik7Cn0sIDUwMDApOwoKY29uc29sZS5sb2coIldlbGNvbWUgdG8gbG91cGUuIik7!!!PGJ1dHRvbj5DbGljayBtZSE8L2J1dHRvbj4%3D)

Click on "Click Me" LOTS of times as quickly as you can :)

**#1 - Evaluate simple async code execution**

What will you see in the console when you run the following code?

```js
setTimeout(function () {
  console.log("a");
}, 5000)

console.log("b");
```

[Enter your answer here]

Go ahead and open up a `node` REPL, or a Chrome Snippet and run that code. Is the result what you expected?

The answer is that it will print `b` and then `a`.

**#2 - Evaluate complex async code execution**

Assume you have a Mongo document that looks like this in the `users` collection:

```
{_id: 'abc', email: 'user@example.com'}
```

This is some code that makes an asynchronous Mongo query using `monk`.  What will you see when you run this code?

```js
var db = require('monk')('localhost/async-examples')
var users = db.get('users')
var id
users.find({email: 'user@example.com'}, function (err, doc) {
  id = doc._id
})
console.log(id);
```

Take a minute to really think about that.  What's happening with the stack?  With the event queue?

DOH!  Yout get `undefined`.  Here's what happens:

1. `users.find` is added to the Stack
  1. The callback (with `id = doc._id`) is added to the Heap
1. `console.log` executes, and prints `undefined`
1. When Mongo is done with the query and sends you back the results, the callback is added to the Queue
1. On the next tick of the Event Loop, the callback is added to the Stack and executed
1. `id` gets set to `'abc'`, but the console.log has _already_ happened

How would you fix that code such that you are properly logging the `id`?

**#2 - Evaluate nested callbacks**

What will the following program output, assuming that `readFile` is asychronous?

```js
console.log('a')
fs.readFile('file1.txt', function (err, data) {
  console.log('b')
  fs.readFile('file1.txt', function (err, data) {
    console.log('c')
  })
  console.log('d')
});
console.log('e')
```

The correct answer is:

- a
- e
- b
- d
- c

Did you get that answer?  If not, grab a whiteboard and draw the stack / heap / queue and work through it.

**#3 - Write nested callbacks**

In Node you can read a file asynchronously like this:

```js
var fs = require('fs')
fs.readFile('path/to-file.txt', 'utf8', function (err, data) {
  // data contains the text of the doc
})
```

Write a separate JavaScript file (it'll need to be separate because you'll have to run it) and read the data from `files/start.txt`.  Notice how there are 2 lines:  the first line has some content, the second line has the name of another file.

Write some code that will:

1. create a variable named `sentence` set to an empty string
1. read `files/start.txt`
1. concatenate the first line onto `sentence`
1. read the file whose name is on line 2
1. concatenate the first line of that file onto `sentence`
1. read the file whose name is on line 2
1. concatenate the first line of that file onto `sentence`

When you are done, your sentence should look like: "My Very Eyes May Just See Under Nine Planets"

**#4 - Show some examples**

Look to any Express app or client-side app you've written that involved nested callbacks, and paste that code here:

```js
// your code here...
```

In a brief sentence or two, describe what's happening in the code above:

> YOUR ANSWER HERE

## Reflect - Self Assess

Go through the Objectives above. For each one, how do you think you did?

Do you _really_ think you could describe the Stack / Heap and Queue to a beginner?  Try it out on your classmates to see!

## Reflect - New questions

What new questions do you have now that you've seen these examples?  Write down 4:

1. _
1. _
1. _
1. _
