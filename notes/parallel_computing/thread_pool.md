---
id: thread_pool
aliases: []
tags:
  - master
  - parallel_computing
---

# Thread Pool

In order to provide an high level abstraction for handling parallel tasks with
C++ threads, we need to build a simple **thread pool**. Through it, will be
possible to implement some of the most used paradigms and skeletons. The idea is
to have at least three main methods:

- `submit`: submit a single task to the pool.
- `batch`: submit a set of tasks to the pool.
- `map`: apply a given function to all the elements of an sequence.

Every of these three methods provide both synchronous and asynchronous versions.

## Usage

The idea is to spawn a precise amount of threads, specified by the user; a
number of threads equal to the number of cores of the machine will be spawned if
nothing no number is specified.

```cpp
thread_pool pool(8);
pool.shutdown();
```

Once the pool is instantiated, all the threads start on a generic task that
expects specific tasks to be computed.

### Submit

The first method to execute a task in parallel is `submit` that submits a single
task to the pool. The asynchronous version (`submit_async`) is non blocking and
returns a `std::future` to handle the result.

```cpp
// async call
auto sum = [](int a, int b) -> int { return a + b; };
auto future = pool.submit_async(sum, 3, 7);

// synchronize and wait for the result
std::cout << future.get() << std::endl;
```

All the jobs are enqueued, waiting to be executed by a free worker; it is
possible to submit an arbitrary number of jobs but only a number equal to the
number of workers of the pool will be executed at the same time.

```cpp
std::vector<std::futures> futures;
auto sum = [](int a, int b) -> int { return a + b; };

// submit multiple jobs and store their future results in a vector
for (int i = 0; i < 32; i++)
	futures.emplace_back(pool.submit(sum, i, i+1));

// print all the results
for (auto& f : futures)
	std::cout << f.get() << std::endl;
```

### Batch

If you want to submit multiple, independent and different from each other tasks
at a time, you can use the `batch` method that accepts the task, its arguments
and number of time it should be executed.

This method comes in _synchronous_ and _asynchronous_ versions. The first one is
a blocking version and returns a `std::vector` of already computed results. The
second one is non blocking and returns a `std::vector` of `std::futures`.

```cpp
// independent tasks
std::vector<task> tasks;

// synchronous version
std::vector<int> results = pool.batch(tasks);

// asynchronous version
std::vector<std::future> futures = pool.batch_async(tasks);
```

### Map

The last implemented method is `map`, that apply a function to a given iterable object.

```cpp
std::vector<double> numbers;
for (int i = 0; i < 128; i++)
	numbers.push_back(i);

// function to apply
auto sum_one = [](int a) -> int { return a + 1; };

// synchronous version
numbers = pool.map(numbers, sum_one);
```

The given function should be defined in order to work on a single element of the iterable.
The pool will try to schedule different parts of the data structure to free threads.

### Shutdown

All threads remain alive and accept new tasks until the `shutdown` method is
invoked; once invoked, the method does not terminate pending tasks but wait
until all the already submitted tasks are done.

## Policies

All the jobs submitted are stored in a shared queue among the workers, that will
concurrently access it for get jobs if their free.

The asynchronous methods execution policy is not _lazy_, in fact they will be
executed as soon as possible so that when `get` method of the future is called,
the result may be ready.

## Future Works

In order to guarantee more efficient and fast scheduling, an _scheduler_ thread
should be introduced; this thread should try to assign tasks to threads without
having them to concurrently access a shared queue, causing lots of performance
issues.

Another feature to be implemented is the possibility to choose if the
asynchronous version of the methods should be _lazy_ or not, granting more
flexibility for different use cases.

## References

- [[parallel_computing]]
- [[cpp]]
- [[workload_balancing]]
- [[threads]]
- [[synchronization]]
