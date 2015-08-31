
## STACK
A stack is a container of objects that are inserted and removed according to the last-in first-out (LIFO) principle. In the pushdown stacks only two operations are allowed: push the item into the stack, and pop the item out of the stack. A stack is a limited access data structure - elements can be added and removed from the stack only at the top. push adds an item to the top of the stack, pop removes the item from the top. A helpful analogy is to think of a stack of books; you can remove only the top book, also you can add a new book on the top.
A stack is a recursive data structure. Here is a structural definition of a Stack:

a stack is either empty or
it consistes of a top and the rest which is a stack;

## HEAP
computer science, a heap is a specialized tree-based abstract data type that satisfies the heap property: If A is a parent node of B then the key of node A is ordered with respect to the key of node B with the same ordering applying across the heap. Heaps can be classified further as either a "max heap" or a "min heap". In a max heap, the keys of parent nodes are always greater than or equal to those of the children and the highest key is in the root node. In a min heap, the keys of parent nodes are less than or equal to those of the children and the lowest key is in the root node. Heaps are crucial in several efficient graph algorithms such as Dijkstra's algorithm, and in the sorting algorithm heapsort. A common implementation of a heap is the binary heap, in which the tree is a complete binary tree (see figure).

## QUE

A queue is a container of objects (a linear collection) that are inserted and removed according to the first-in first-out (FIFO) principle. An excellent example of a queue is a line of students in the food court of the UC. New additions to a line made to the back of the queue, while removal (or serving) happens in the front. In the queue only two operations are allowed enqueue and dequeue. Enqueue means to insert an item into the back of the queue, dequeue means removing the front item. The picture demonstrates the FIFO access.

The difference between stacks and queues is in removing. In a stack we remove the item the most recently added; in a queue, we remove the item the least recently added.


# UNDERSTANDING EVENT LOOPS

Stack:
When a function is called in javascript, runtime creates a frame in the stack which holds that particular function’s arguments and local variables. When the function returns that frame is popped out of the stack. Example: (from MDN site)

function f(b) {
var a = 12;
return a+b+35;
}

function g(x) {
var m = 4;
return f(m*x);
}

g(21);

When we invoke g(21), the first frame is created holding g arguments and local vars. Now when f is called from within g a second frame is created and pushed above the first one. This new frame also holds arguments to g function and its local vars.

When f function returns the top element in the stack is popped out and when g returns the stack is now empty.

### Heap:
This is just a regular memory region.

### Queue:
JS runtime’s message queue holding a list of messages to be processed. These messages have a function associated with them in form of a callback. These message are processed one by one by js runtime when the stack is empty. Meaning, an available message’s callback function in the queue is called, causing it to be placed in the stack and out of the queue to be processed.

### Event Loop
This concept describes the implementation of how JS queuing mechanism works. it monitors the queue and waits – synchronously – for a message to arrive if there is none in the queue. Once the stack is empty and a message is available it processes it. Note, each message is processed completely before any other message is processed. Meaning your function will run in its entirety before any other code runs. This is different from languages like Java where if a function/method runs in a thread it can be stopped by another piece of code running in another thread.

### NodeJS
So far as the developer’s code is concerned , in node you only have a single main thread running. Meaning you can not do any parallel code execution. But all I/O calls are event’d(?) and asynchronous (this includes http requests) meaning as long as you use callbacks in your code you are guaranteed your code is never interrupted based on the event loop mechanism described above.

### How does nodejs implement its event loop?
Nodejs is based on C++, and internally as part node internal architecture it does use thread pools!
So does nodejs runs on a single thread or not? Yes and no :)

What nodejs provides as far as my findings go is a clever way to get us out messy, expensive, and at times implementation-wise inefficient world of threads left wide open for a programmer to play with. So, yes, nodejs runs a single thread of execution, and this is what matters to us and to our code. But, once this thread of execution invokes node’s runtime, then underneath via libraries it utilizes – for example libuv – it does use threads. But, the way it uses threads is much more efficient – see this and this- but basically it uses kernel level thread scheduling and facilities based on the platform being run under, for us it’s the mighty Linux RH.

So, as far we the developers are concerned: (highly oversimplified)
– a blocking I/O call comes in
– we wrap it with a callback function
– thread of execution gets a hold of it
– passes the request plus the callback function to node runtime
– now the thread is free and can pick up another request
– runtime will invoke the callback when the operation is complete
– thread of execution is notified and result is sent back
