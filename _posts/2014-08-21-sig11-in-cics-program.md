---
layout: post
title: Signal 11 in CICS program
tags: [cics, abend, c]
---
I've dusted off my CICS programming skills for a small project and while trying to debug a problem using the `resp` and `resp2` codes from an `exec cics` call, the program starting terminating with a signal 11.

As it turns out I'd forgotten a crucial line of code to add at the start of the main function
```c
exec cics address eib(dfheiptr);
```
which sets up the required pointers to access the response codes.

Without this line the translated code attempts to dereference a `NULL` pointer and you end up in the same mess I did, with signal 11 termination of the program.
