---
id: move_semantics
aliases: []
tags:
  - master
---

# Move Semantics

Move semantics are an efficient way of _moving_ data from an object to another
avoiding useless copies and passing the _ownership_ of the moved object to
another object.

In general, moving objects makes sense only with something that is some kind of
container or data structure, like `std::string` or `std::vector`. Moving should
also be something intentional, for example if we have a `std::vector` of the
following `Buffer` class

```cpp
class Buffer
{
public:
    Buffer(size_t n) : m_Size(size) {
        m_Data = new double[n];
    }

    ~Buffer() { delete[] m_Data; // RAI pattern }

private:
    size_t m_Size;
    double* m_Data;
}
```

and supposing that every `Buffer` contains a huge array. We could get in
situation like this

```cpp
std::vector<Buffer> buffers;

buffers.emplace_back(1024);
buffers.emplace_back(2048);
buffers.emplace_back(4096);
```

where some buffers are created only one time an directly inside the `vector` so
without copying. But what if we have another `vector` of `Buffer` and we want to
merge them together? The first solution is to copy all the elements of one
`vector` to the other, but can be very costly; instead we could _move_ all the
elements like follows

```cpp
std::vector<Buffer> merged; // with some elements already inside
for (const auto& b : buffers)
    buffers.push_back(std::move(b));
```

so now the `merged` vector is the owner of all the elements and we should
account that in the old `vector` there are only elements left in an inconsistent
state that should not be used.

## Enabling Move Semantics

To enable move semantics we have to add some code that typical for many data
structures, including STL containers. The two main things that must be
implemented are a _move constructor_

```cpp
Buffer(Buffer&& other) noexcept
    : m_Size(other.m_Size)
{
    m_Data = other.m_Data;

    other.m_Size = 0;
    other.m_Data = nullptr; // lose resource ownership
}
```

and a _move assign operator_.

```cpp
void operator=(Buffer&& other) noexcept
{
    m_size = other.m_size;
    delete[] m_buffer; // previous buffer freed
    m_buffer = other.m_buffer; // new content

    other.m_Size = 0;
    other.m_Data = nullptr;
}
```

The main difference is that an assign operator assign a new value to an already
exixsting variable with its own value, so we need to delete the previous content
in order to store the new one.

The `std::move` function does not move anything, it simply casts a persistent
object to a temporary (more precisely to an _rvalue_ reference) that can be
correctly moved by a move constructor or assign operator.

### Rule of Five

A de facto standard in C++ is the so called _"rule of five"_ that states that
every data structure or container should have

1. A default normal constructor
2. A copy constructor
3. A copy assign operator
4. A move constructor
5. A move assign operator

in fact every STL container of C++ respects this rule.

## References

- [[cpp]]
- [Move Constructor](https://en.cppreference.com/w/cpp/language/move_constructor)
- [std::move](https://en.cppreference.com/w/cpp/utility/move)
