---
id: auto_type_deduction
aliases: []
tags:
  - master
---

# Automatic Type Deduction

In C++ to help the programmer writing more concise and portable code, there are
some mechanisms of **automatic type deduction**. The most simple is through the
`auto` keyword, that can infer the type of a variable depending on the
right-hand side of an assignment operation.

```cpp
auto i = 10; // int
auto d = 3.14; // double
```

Auto type deduction is also useful for complex types, templates and type traits,
that most of the time have very long names that worsen the code readability.

```cpp
#include <chrono>

// full type
std::chrono::time_point<std::chrono::steady_clock> t1 =
    std::chrono::steady_clock::now();

// auto
auto t2 = std::chrono::steady_clock::now();
```

On the other hand, always use auto could add mental overhead for the programmer
who reads the code, sometimes is nice to know the type of a variable as fast as
you see it.

Another aspect to consider when using the `auto` keyword is that your code could
_adapt_ automaticaly to changes. For example we can have a variable that stores
the result of a function like this

```cpp
int sum(int a, int b) {
    return a + b;
}

auto res = sum(10, 20);
```

If now we change the function in this way

```cpp
float sum(float a, float b) {
   return a + b;
}
```

we have no need to also change the `res` variable type, which is quite useful
but in larger and more complex code base could lead to runtime errors not easy
to detect.

---

When we use automatic type deduction we also have to pay attention to some other
details, for example

```cpp
auto d = 3.14; // double
auto f = 3.14f; // float
auto s = "Hello" // const char*
```

Let's also pay attention to other qualifiers like `const` that are not directly
deduced.

```cpp
const int n = 1024;
auto x = n; // deduced as int (not const int)
const auto y = n; // deduced as const int
auto& z = n; // deduced as const int&
```

the last one is because we are _referencing_ a `const` variable so we implicitly
inherit the its constant qualifier.

Automatic type deduction can also be useful to store _lambdas_ into variables
that can be called in a second moment.

```cpp
auto f = []() { std::cout << "this is a lambda" << std::endl; }
f();
```

We can for example store lambdas in a STL container like an `std::vector`.

## Forwarding

The type **forwarding** is very useful to keep the original value category of a
variable (_lvalue_ or _rvalue_).

For example we can use the so called **forwarding reference** (`auto&&`) that
enables code that can accept both _lvalue_ (`T&`) and _rvalue_ (`T&&`)
references. For simplicity we refer to _rvalues_ as temporary objects that do
not persist beyond the expression.

Pay attention to the fact that only `auto&&` is a forwarding reference, while
something like `std::string&&` does not be a type deduce process because is
explicitly specified.

### Perfect Forwarding

Another thing to preserve the value category is called **perfect forwarding**
that can be enabled with the function `std::forward` and it is typically used
with templates.

```cpp
void foo(const std::string& s) {
    std::cout << "persistent" << std::endl;
}

void foo(std::string&& s) {
    std::cout << "temporary" << std::endl;
}

template <typename T>
void bar(T&& param) {
    foo(param);
}
```

Let's now see what happens when `bar` is called with different type of strings.

```cpp
std::string s = "string";
bar(s);             // output: persistent
bar("string");      // output: temporary
bar(std::move(s));  // output: persistent
```

So in the last call we changed `s` to be a temporary but once it is dispatched
by from `bar` to `foo` it returns being a persistent value. Let's now introduce
the `bar_fw` function and see how perfect forwarding change that.

```cpp
template <typename T>
void bar_fw(T&& param) {
    foo(std::forward<T>(param));
}

std::string s = "string";
bar_fw(s);              // output: persistent
bar_fw("string");       // output: temporary
bar_fw(std::move(s));   // output: temporary
```

As expected the last output changed and the value category was mantained.

## Templates

Another form of automatic type deduction can be done via _templates_ that are
basically _generics_. Let's for example define a function like this

```cpp
template <typename T>
T sum(T a, T b) {
    return a + b;
}
```

A template can be used with an explicit type declaration

```cpp
int res = sum<int>(10, 20);
```

but the type can also be inferred automatically

```cpp
auto res1 = sum(10, 20); // will return an int
auto res2 = sum(3.14, 9.81); // will return a double
```

and in this case the compiler will instanciate multiple versions of the `sum`
function but specialized with the needed type.

## References

Links: [[cpp]], [[move_semantics]]

- [auto](https://en.cppreference.com/w/cpp/keyword/auto)
- [std::forward](https://en.cppreference.com/w/cpp/utility/forward)
- [templates](https://en.cppreference.com/w/cpp/language/templates)
