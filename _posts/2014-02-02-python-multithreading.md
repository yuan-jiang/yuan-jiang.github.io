---
layout: post
title: Python multithreading
date: 2014-02-02 15:50:00 +0800
author: Yuan Jiang
tags: python multithreading
---

Python multithreading. NOTE that although multiple threads can run within the python interpreter, ONLY one thread is being executed by the interpreter at any given time, which is ensured by the GIL (global interpreter lock) in python virtual machine. Python multithreading is more appropriate for I/O-bound applications than for CPU-bound applications as I/O releases GIL.

## Python multithreading programming with thread module (for test only)
{% highlight python %}
#!/usr/bin/env python
# coding=utf-8

import thread
import time

def printer_A(lock):
    print "A starts at: ", time.ctime()
    time.sleep(4)
    print "A finishes at: ", time.ctime()
    lock.release()

def printer_B(lock):
    print "B starts at: ", time.ctime()
    time.sleep(2)
    print "B finishes at: ", time.ctime()
    lock.release()

if __name__ == "__main__":
    print "Main starts at: ", time.ctime()
    lockA = thread.allocate_lock()
    lockA.acquire()
    thread.start_new_thread(printer_A, (lockA,))
    lockB = thread.allocate_lock()
    lockB.acquire()
    thread.start_new_thread(printer_B, (lockB,))
    while lockA.locked(): pass
    while lockB.locked(): pass
    print "Main finishes at: ", time.ctime()

# output
Main starts at:  Tue Mar 29 16:40:57 2016
B starts at:  Tue Mar 29 16:40:57 2016
A starts at:  Tue Mar 29 16:40:57 2016
B finishes at:  Tue Mar 29 16:40:59 2016
A finishes at:  Tue Mar 29 16:41:01 2016
Main finishes at:  Tue Mar 29 16:41:01 2016
[Finished in 4.093s]
{% endhighlight %}

## Python multithreading programming with threading module
- Similar to thread module, pass function to Thread instance:
{% highlight python %}
#!/usr/bin/env python
# coding=utf-8

import threading
import time

def printA():
  print "A starts at: ", time.ctime()
  time.sleep(4)
  print "A finishes at: ", time.ctime()

def printB():
  print "B starts at: ", time.ctime()
  time.sleep(2)
  print "B finishes at: ", time.ctime()

if __name__ == "__main__":
  print "Main starts at: ", time.ctime()
  t_A = threading.Thread(target=printA, args=())
  t_B = threading.Thread(target=printB, args=())
  t_A.start()
  t_B.start()
  t_A.join()
  t_B.join()
  print "Main finishes at: ", time.ctime()

# output
Main starts at:  Wed Mar 30 10:53:44 2016
A starts at:  Wed Mar 30 10:53:44 2016
B starts at:  Wed Mar 30 10:53:44 2016
B finishes at:  Wed Mar 30 10:53:46 2016
A finishes at:  Wed Mar 30 10:53:48 2016
Main finishes at:  Wed Mar 30 10:53:48 2016
[Finished in 4.074s]
{% endhighlight %}

- Subclass Thread and create subclass instance:
{% highlight python %}
#!/usr/bin/env python
# coding=utf-8

import threading
import time

class MyThread(threading.Thread):
  def __init__(self, func, args, name=''):
    threading.Thread.__init__(self)
    self.name = name
    self.func = func
    self.args = args

  def run(self):
    self.func(*self.args)

def printA():
  print "A starts at: ", time.ctime()
  time.sleep(4)
  print "A finishes at: ", time.ctime()

def printB():
  print "B starts at: ", time.ctime()
  time.sleep(2)
  print "B finishes at: ", time.ctime()

if __name__ == "__main__":
  print "Main starts at: ", time.ctime()
  t_A = MyThread(printA, ())
  t_B = MyThread(printB, ())
  t_A.start()
  t_B.start()
  t_A.join()
  t_B.join()
  print "Main finishes at: ", time.ctime()

# output
Main starts at:  Wed Mar 30 11:05:57 2016
A starts at:  Wed Mar 30 11:05:57 2016
B starts at:  Wed Mar 30 11:05:57 2016
B finishes at:  Wed Mar 30 11:05:59 2016
A finishes at:  Wed Mar 30 11:06:01 2016
Main finishes at:  Wed Mar 30 11:06:01 2016
[Finished in 4.11s]
{% endhighlight %}
