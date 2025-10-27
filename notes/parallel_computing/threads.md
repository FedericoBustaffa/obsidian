---
id: threads
aliases: []
tags:
  - master
  - parallel_computing
---

# Threads

The main method to achieve **concurrency** in C++ is through **multi-threading**
(`std::thread`) side by side with like mutexes, condition variables, futures
and all the most common concurrency mechanisms and constructs.

To instanciate threads we can use `std::thread`, passing a callable and its
parameters; the constructor starts automatically the function, assigning it to a
thread

```cpp
#include <thread>

void hello(){ std::cout << "Hello from thread" << std::endl; }

std::thread t(hello);
t.join();
```

The `join` method wait the termination of the thread, giving a first
synchronization mechanism. If instead we are not interested in synchronize with
thread's termination we can use `detach` to let the thread manage on its own.

Each thread has a separate stack and in order to achieve true parallelism there
should be at maximum a number of threads equal to the number of cores of the
machine we are running the algorithm.

Once a thread is assigned with a task (a callable) is not possible to reuse the
thread and it is needed to spawn another one. The `std::thread` object is also
_move-only_ and non copyable (copy constructor and assignment operator are
deleted).

To pass arguments to a thread we must pay attention on the fact that for passing
references we must use `std::ref`.

```cpp
void func(int value, int* ptr, int& ref) { }

int value, ptr, ref;
std::thread t(value, &ptr, std::ref(ref));
```

otherwise it will not compile. To compile remember to add the `-pthread` flag

```bash
g++ -O3 -std=c++17 main.cpp -o app.out -pthread
```

that enables the threading support and could define some macros that can change
the conditional inclusion in some standard header. It also enable the compiler
to choose the thread-safe version of some functions.

## Promises and Futures

In the C++ threading library, **promises** and **futures** are one of the lowest
level mechanism to communicate results. The callable passed to the thread should
return `void` but we `std::promise` we can create a one-time synchronization
mechanism to store and get the result.

```cpp
#include <thread>
#include <future>

std::promise<int> p;
std::future<int> f = p.get_future();

std::thread worker(
    [p{std::move(p)}](int a, int b) mutable {
        promise.set_value(a + b);
    },
    10, 20 // a and b parameters
);

std::cout << f.get() << std::endl; // output: 30
worker.join();
```

The `get` method of the `std::future` class blocks until the promise is
fulfilled with the `set_value` method.

The `std::promise` class is not copyable but movable, so, in order to pass it to
a thread, it must be moved like in the example above.

## Packaged Tasks

A more high level mechanism is given by the `std::packaged_task` that let us
embed the task we want to execute and the promise/future mechanism in a single
callable object.

```cpp
int sum(int a, int b) { return a + b; }

std::packaged_task<int(int, int)> task(sum);
std::future<int> f = task.get_future();
task(5, 8); // ! sequential execution
std::cout << f.get() << std::endl;
```

To execute the task asynchronously it's necessary to explicitly spawn a thread
and move the task.

```cpp
int sum(int a, int b) { return a + b; }

std::packaged_task<int(int, int)> task(sum);
std::future<int> f = task.get_future();
std::thread worker(std::move(task), 5, 8); // async execution

std::cout << f.get() << std::endl;
worker.join();
```

Remember also that the `std::packaged_task` is a _move-only_ type, like
`std::promise`.

## Async

The most high level mechanism to execute something asynchronously, provided by
the standard C++ is `std::async`.

```cpp
int sum(int a, int b) { return a + b; }

std::future<int> f = std::async(sum, 5, 8);
std::cout << f.get() << std::endl;
```

which spawns a thread that execute the task with the provided arguments that then
will be destroyed.

## References

Links: [[parallel_computing]], [[cpp]]

- [std::thread](https://en.cppreference.com/w/cpp/thread/thread)
- [std::promise](https://en.cppreference.com/w/cpp/thread/promise)
- [std::future](https://en.cppreference.com/w/cpp/thread/future)
- [std::packaged_task](https://en.cppreference.com/w/cpp/thread/packaged_task)
- [std::async](https://en.cppreference.com/w/cpp/thread/async)
- [thread_local](https://en.cppreference.com/w/cpp/language/storage_duration)
