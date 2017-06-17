---
layout: post
title: Pathos solves pickle issue for multiprocssing
date: 2017-06-16 23:15:00 +0800
tags: python
---

Python multiprocssing is useful in executing concurrent tasks with multiple processes. But it also requires the objects being executed support pickling, which is not always true for types like class instance methods, staticmethods and etc. [Pathos](https://github.com/uqfoundation/pathos) has a multiprocessing implementation that uses [dill](https://github.com/uqfoundation/dill) on the backend which supports serializing and deserializing for almost all types.

## Example using builtin multiprocessing that would raise PicklingError
{% highlight python %}
import os
from multiprocessing import Pool


class Tasks:

    @staticmethod
    def process_some_task(item):
        print("Processing...", item, "by pid:", os.getpid())

if __name__ == "__main__":
    with Pool(4) as pool:
        pool.map(Tasks.process_some_task, range(10))

{% endhighlight %}

## Error raised running above script
{% highlight python %}
(venv) vagrant@vagrant-ubuntu-trusty-64:~/test$ python test.py
Traceback (most recent call last):
  File "test.py", line 13, in <module>
    pool.map(Tasks.process_some_task, range(10))
  File "/usr/lib/python3.4/multiprocessing/pool.py", line 260, in map
    return self._map_async(func, iterable, mapstar, chunksize).get()
  File "/usr/lib/python3.4/multiprocessing/pool.py", line 599, in get
    raise self._value
  File "/usr/lib/python3.4/multiprocessing/pool.py", line 383, in _handle_tasks
    put(task)
  File "/usr/lib/python3.4/multiprocessing/connection.py", line 206, in send
    self._send_bytes(ForkingPickler.dumps(obj))
  File "/usr/lib/python3.4/multiprocessing/reduction.py", line 50, in dumps
    cls(buf, protocol).dump(obj)
_pickle.PicklingError: Can't pickle <function Tasks.process_some_task at 0x7f45b3e626a8>: attribute lookup process_some_task on __main__ failed
{% endhighlight %}

## Solution by pathos
- install pathos
{% highlight bash %}
$ pip install pathos
{% endhighlight %}

- replace multiprocessing
{% highlight python %}
import os
from pathos.multiprocessing import ProcessingPool as Pool


class Tasks:

    @staticmethod
    def process_some_task(item):
        print("Processing...", item, "by pid:", os.getpid())

if __name__ == "__main__":
    with Pool(4) as pool:
        pool.map(Tasks.process_some_task, range(10))
{% endhighlight %}

## Successful output with pathos
{% highlight python %}
(venv) vagrant@vagrant-ubuntu-trusty-64:~/test$ python test.py
Processing... 0 by pid: 3827
Processing... 1 by pid: 3828
Processing... 2 by pid: 3826
Processing... 3 by pid: 3829
Processing... 4 by pid: 3827
Processing... 5 by pid: 3828
Processing... 6 by pid: 3826
Processing... 7 by pid: 3829
Processing... 8 by pid: 3827
Processing... 9 by pid: 3828
{% endhighlight %}

## References
- [What can multiprocessing and dill do together?](https://stackoverflow.com/questions/19984152/what-can-multiprocessing-and-dill-do-together)
- [pathos: a framework for parallel graph management and execution in heterogeneous computing](http://trac.mystic.cacr.caltech.edu/project/pathos/wiki.html)
- [dill: serialize all of python](http://trac.mystic.cacr.caltech.edu/project/pathos/wiki/dill.html)
