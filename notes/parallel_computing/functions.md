---
id: functions
aliases: []
tags:
  - master
---

# Function Objects

In C++ we have this concept of a _callable_ object that in some specific case
assumes the name of **function object** or **functor**. A function object is an
object (like a `struct` or a `class`) that has an override for the `call`
operator like

```cpp
class Printer {
public:
    Printer(const std::string& message)
        : m_Message(message) { }

    void operator()() {
        std::cout << m_Message << std::endl;
    }
};
```

and that can be used like a function

```cpp
Printer printer("Foo");
printer(); // output: "Foo"
```

In general a function object can be used where a plain function is needed but
can also have some state like in the example above.

## Lambdas

Another very useful and powerfull mechanism is given by **lambdas**, anonymous
functions that have the power to _capture_ the environment (variables) in a
smart way.

### Capture Environment

Environment variables can be captured by

- **Value**: copied or moved and by default immutable.
- **Reference**: normal by reference argument passing.

```cpp
std::string s = "Bar";
int n = 1024;

// capture s by reference and n by value
auto f = [&s, x]() {
    std::cout << s << std::endl;
    std::cout << n << std::endl;
};

f(); // call the lambda
```

If the only argument specified in the brackets is `=` all the variables used in
the lambda will be captured by value from the environment. If instead, the only
argument is `&` all the variables will be captured by reference.

#### Mutable

As said before, all variables captured by value are immutable or constants, so
to modify them the `mutable` keyword must be used.

```cpp
int n = 1024;

// capture all by value
auto f = [=]() mutable {
    n = n * 2;          // now is allowed
    std::cout << n << std::endl;
};
```

Sometimes there will be objects like `std::promise` that are not copyable and
that cannot be passed by reference so we have to move them and to enable changes
we have to use the `mutable` keyword.

---

We can also specify the return type of lambda

```cpp
auto f = [&] (int a, int b) -> int { return a + b; };
```

but usually is not necessary because is deduced automatically by the compiler.

### Store Lambdas

Usually lambdas have _unique_ types and they are difficult to store in a
container; so a good practice is to let them be held by an `std::function`,
provided in the `functional` header.

```cpp
auto sum = [](float a, float b) { return a + b; };
auto sub = [](float a, float b) { return a - b; };

std::vector<std::function<float(float, float)>> lambdas;
lambdas.push_back(sum);
lambdas.push_back(sub);
```

Of course all the lambdas in this example must have the same number and type of
parameters and same return value type to stay in the `vector`.

In this way we can store them in a vector for various purposes like task storing
in concurrent programming.

## References

- [Function objects](https://en.cppreference.com/w/cpp/utility/functional)
- [Lambda](https://en.cppreference.com/w/cpp/language/lambda)
- [std::function](https://en.cppreference.com/w/cpp/utility/functional/function)
