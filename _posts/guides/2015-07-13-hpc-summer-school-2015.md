---
layout: article
title: "SciNet high performance computing summer school 2015"
categories: guides
excerpt: "Notes from the courses."
ads: false
share: false
toc: true
image:
  feature: hpc2015-1600x800.jpg
  teaser: cover.hpc2015.jpg
---

R
---

R: Iterators
============

These are handy if you want to access a massive list/array/container without loading it all into memory first. They are and ugly bolt-on library though, and cannot be used from within a for loop! So they are sort of limited in utility.

{% highlight r %}

library(iterators)
names <- c("Bob", "Mary", "Jack", "Jane")
inames <- iter(names)
nextElem(inames)
[1] "Bob"
nextElem(inames)
[1] "Mary"

{% endhighlight %}

R: Apply
========

I regularly confuse these. The apply functions repeatedly apply a function to a lot of individual elements. Many parallel routines are parallel versions of these higher-level functions.

+ lapply: apply a function to each element of a list/vector.
+ sapply: simplify the lapply return list to a vector or array if possible.
+ apply: apply a function to rows, columns, or elements of an array.
+ tapply: apply a function to subsets of a list/vector.
+ mapply: apply a function to the ”transpose” of a list. Pass two lists of length three; apply function to first items of lists, then second, then third.

Examples below:

{% highlight r %}
# lapply
mean.n.rnorm <- function(n) return(mean(rnorm(n)))
ns <- c(1, 10, 100, 1000)
str(lapply(ns, mean.n.rnorm))
# List of 4
#  $ : num 0.685
#  $ : num 0.0685
#  $ : num -0.038
#  $ : num 0.049

# sapply
ns <- c(1, 10, 100, 1000)
random.nums <- lapply(ns, rnorm)
means <- sapply(random.nums, mean)
# [1]  1.12830392  0.06851177  0.20311111 -0.04550857

# apply
A <- matrix(1:9, nrow = 3, ncol = 3)
apply(A, MARGIN = 1, max)                  # max of each row
apply(A, MARGIN = 2, max)                  # max of each col
apply(A, MARGIN = 1:2, function(x) {x**2}) # square of each item

# tapply
data <- morley
str(data)
'data.frame': 100 obs. of  3 variables:
  $ Expt : int  1 1 1 1 1 1 1 1 1 1 ...
  $ Run  : int  1 2 3 4 5 6 7 8 9 10 ...
  $ Speed: int  850 740 900 1070 930 850 950 980 980 880 ...

# tapply
tapply(data$Speed, data$Expt, mean)
   1     2     3     4     5
909.0 856.0 845.0 820.5 831.5

# split: x into unique values of y
str(split(data$Speed, data$Expt))
List of 5
  $ 1: int [1:20] 850 740 900 1070 930 850 950 980 980 880 ...
  $ 2: int [1:20] 960 940 960 940 880 800 850 880 900 840 ...
  $ 3: int [1:20] 880 880 880 860 720 720 620 860 970 950 ...
  $ 4: int [1:20] 890 810 810 820 800 770 760 740 750 760 ...
  $ 5: int [1:20] 890 840 780 810 760 810 790 810 820 850 ...

{% endhighlight %}

Performance tip: if you know the size and type that sapply will return, create such a vector/matrix and use vapply, passing it the example object as the third parameter (everything else stays the same). This can be substantially faster, and more memory-efficient for large outputs.

R: Plotting
===========

+ par(new = TRUE): This will keep R from overwriting your previous plot, which is the default behaviour.
+ par(family = ’HersheySans’): change the font family to a vector-drawn font. Always use vector-drawn fonts for publication-quality plots.
+ par(font = 2): change the font format. 1 - default, 2 - bold, 3 - italic, 4 - bold italic.
+ par(ann = FALSE): do not annotate the plot. In this case you must label your axes after the plotting function is called.
+ par(mfrow = c(2,2)): make a 2 x 2 plots. Allows you to put several plots together.

{% highlight r %}

x <- 1:100 / 100
plot(x, sin(x * 4 * pi),  type = "o", col = "red", axes = F, xlab = "time [s]")
lines(x, sin(x * 2 * pi), type = "o", col = "black") # add a new line

{% endhighlight %}

To save a plot after it has been made, use one of the `dev.copy(...)`, `dev.copy2pdf(...)`, etc. commands.

R: Decision Tree Example
========================

{% highlight r %}

str(iris)
 'data.frame': 150 obs. of  5 variables:
  $ Sepal.Length: num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
  $ Sepal.Width : num  3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
  $ Petal.Length: num  1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
  $ Petal.Width : num  0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...
  $ Species     : Factor w/ 3 levels "setosa","versicolor",..: 1 1 1 1 1 1 1 1 1 1 ...

# 70/30 split
ind <- sample(1:2, nrow(iris), replace = T, prob = c(0.7, 0.3))
trainData <- iris[ind == 1,]
testData <- iris[ind == 2,]

# formula depends on all of the data
library(party) # load up party
myFormula <- Species ~ Sepal.Length + Sepal.Width + Petal.Length + Petal.Width
iris.tree <- ctree(myFormula, data = trainData) # create decision tree
table(predict(iris tree), trainData$Species) # get a sense of our fitting accuracy
plot(iris_tree)
testPred <- predict(iris_tree, newdata = testData) # test data

{% endhighlight %}

Parallel Programming in Python
------------------------------

From what I saw today, it looks like the real package to learn is `multiprocessing` and numexpr` for performance increases. We also talked about parallelizing across multiple ipython sessions, but I think it might be worth going over the R parallel implementations, as the python ones are pretty complicated.

Python: Garbage Collection
==========================

Python, like R, relies on a ”garbage collector” to clean up un-needed variables and limit memory usage. ”Every so often” a garbage collection task runs and deletes variables that won't be used anymore. You can force the garbage collector to run at any time by running the command:

{% highlight python %}

import gc
collect = gc.col

{% endhighlight %}


Python: Out of Code Computation
===============================

Out of core or ”external memory” computation leaves the data on disk, bringing into memory only what is needed, or what fits, at any given time. For some computations, this works out well (but note: disk access is always much slower than memory access). The `np.memmap` class does this -- other techniques: pytables, hdf5, numpy.

{% highlight python %}

import numpy as np

# this is a big old file
fp = np.memmap('bigfile', dtype='float64', mode='w+', shape=(40000,40000))
for i in xrange(40000):
    total += sum(fp[i,:]) # this happens in place, saves you RAM.
average = total / (40000 * 40000)
print(average)

{% endhighlight %}

Python: Just in Time Compilation
================================

Just in time compilation for python! can really speed up computations at the small cost of bizarre syntax:

{% highlight python %}

import numexpr as ne

a = np.random.rand(1e6)
b = np.random.rand(1e6)
c = np.zeros(1e6)

%timeit c = a**2 + b**2 + 2 * a * b
10 loops, best of 3: 22.2 ms per loop

old = ne.set_num_threads(1)
%timeit ne.evaluate("a**2 + b**2 + 2 * a * b", out = c)
100 loops, best of 3: 13.9 ms per loop

{% endhighlight %}

Python: Forking Processes
=========================

The below (fork.py) uses a fork to run a separate piece of code () in a child process (child.py):

`child.py`

{% highlight python %}

import os
print "Hello from", os.getpid()
os._exit(0)

{% endhighlight %}

`fork.py`

{% highlight python %}

import os

def main():
    while(True):
        newpid = os.fork()
        if newpid == 0:
            os.execlp("python", "python", "2-child.py")
            assert False, "Error starting program :("
        else:
            print("hello from parent ", os.getpid(), newpid)

        if raw_input() == "q":
            break

if __name__ == '__main__':
    main()

{% endhighlight %}

Use `os.waitpid` (child pid) if you need to wait for the child process to finish. Otherwise the parent will exit and the child will live on. `fork()` is a Unix command. It doesn't work on Windows, except under Cygwin.

This must be used very carefully. ALL the data is copied to the child process, including file handles, open sockets, database connections, etc. Be sure to exit using `os._exit(0)` rather than `os.exit(0)`, or else the child process will try to clean up resources that the parent process is still using. Because of this, `fork()` can lead to code that is difficult to maintain long-term.

R: Threads (Multi?)
===================

+ process: resources needed to execute a program.
+ thread: path of execution within a process. faster to create and destroy.

Threads within the same process share the same address space. This means they can share the same memory and can easily communicate with each other. However, the python Interpreter uses the Global Interpreter Lock (GIL). The GIL prevents race conditions by preventing threads from the same Python program from running simultaneously. As such, only one core is used at any given time. This prevents race conditions but also prevents proper multi-threading.

R: Multiprocessing
==================

Unlike fork, multiprocessing works on Windows (better portability). Slightly longer start-up time than threads. Multiprocessing spawns separate processes, like fork, and as such they each have their own memory. Multiprocessing requires pickleability for its processes. Passing non-pickleable objects, such as sockets, to spawned processes is not possible.

{% highlight python %}

import time
import multiprocessing as mp

def my summer(start, stop):
    tot = 0
    for i in xrange(start,stop):
        tot += i

begin = time.time()
processes = []

for i in range(10):
    p = mp.Process(target=my_summer, args=(0, 50000000))
    processes.append(p)
    p.start()

# wait for all processes
for p in processes:
    p.join()

print("time: {}".format(time.time() - begin))

{% endhighlight %}

Multiprocessing pools:

{% highlight python %}

def my_summer2(data):

    # only 1 argument can be passes using Pool
    start, stop = data
    tot = 0
    for i in xrange(start, stop):
        tot += i

begin = time.time()
numjobs = 10
numproc = mp.cpu_count()

# the arguments are the same for all
inputs = [(0, 50000000)] * numjobs

p = mp.Pool(processes=numproc)
p.map(my_summer2, inputs)

print("time: {}".format(time.time() - begin))


{% endhighlight %}

Shared memory:

{% highlight python %}

from multiprocessing import Process, Value

def myfunc(v):
    time.sleep(0.001)
    v.value += 1

v = Value('i', 0)
procs = []
for i in range(10):
    p = Process(target=myFunc, args=(v,))
    procs.append(p)
    p.start()

    for proc in procs:
        proc.join()
    print v.value

{% endhighlight %}

Race condition above! We can prevent this by using `Lock` (we had multiple processes talking to the same piece of memory in the above condition, so now we will explicitly prevent that using a lock).

{% highlight python %}

from multiprocessing import Process, Value, Lock

def mufunc(v, lock):
    for i in range(50):
        time.sleep(0.001)
        with lock:
            v.value += 1


v = Value('i', 0)
lock = Lock() # :D
procs = []

for i in range(10):
    p = Process(target=myfunc, args=(v, lock))
    procs.append(p)
    p.start()

for proc in procs:
    proc.join()
print(v.value)

{% endhighlight %}

Multiprocessing also allows you to share a block of memory through the Array ctypes wrapper. This allows us to work with 1D arrays, not just single floats.

{% highlight python %}

from numpy import arange
from multiprocessing import Process, Array
def myfuncf(a, i):
    a[i] = -a[i]

arr = Array(’d’, arange(10.))
procs = []
for i in range(10):
    p = Process(target=myfunc, args=(arr, i))
    procs.append(p)
    p.start()

for proc in procs:
    proc.join()
print arr[:]

{% endhighlight %}

