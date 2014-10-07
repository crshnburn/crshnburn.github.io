---
layout: post
title: Simple Java Producer/Consumer
---

For a recent project I needed to have a some data stored in a remote database but having this happen on the main processing thread introduced too much of a delay.

To counter this the slow part needed to be moved off to another thread but retain a way to communicate with the main thread. This was done using the `ArrayBlockingQueue` class to store the data objects until they're ready to be processed.

The main thread that is producing the data can then put an object on the queue using the `put` method. This will block if the queue is full, so if this is likely to be a problem due to slow processing by the consumer the size of the queue will need to be made big enough to accomodate this.

The Consumer thread that is then started can loop forever and wait on the `take` method of the queue until some data is `put` there and then do the processing to store it in the remote database.
