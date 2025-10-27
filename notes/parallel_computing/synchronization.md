---
id: synchronization
aliases: []
tags:
  - master
  - parallel_computing
---

# Thread Synchronization

To **synchronize** thread the most common mechanisms are a mixture of **mutex**
and **condition** variables that ensure safety and correct ordering in the
operations.

## Mutex Variables

The mutex variables are the most common way to ensure that critical sections in
the code are accessed by one thread at a time.

```cpp
std::vector<int> numbers;
std::mutex mtx;

std::thread producer([&]() {
        for (int i = 0; i < 1000; i++) {
            mtx.lock();
            numbers.push_back(i);
            mtx.unlock();
        }
    }
);

std::thread consumer([&]() {
        for (int i = 0; i < 1000; i++) {
            mtx.lock();
            numbers.pop();
            mtx.unlock();
        }
    }
);

producer.join();
consumer.join();
```

## References

Links: [[parallel_computing]], [[cpp]], [[threads]]

- [std::mutex](https://en.cppreference.com/w/cpp/thread/mutex)
- [std::condition_variable](https://en.cppreference.com/w/cpp/thread/condition_variable)
- [std::shared_mutex](https://en.cppreference.com/w/cpp/thread/shared_mutex)
