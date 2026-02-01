---
id: pram_model
aliases: []
tags:
  - master
  - parallel_computing
---

# PRAM Model

The **PRAM** (Parallel Access Random Machine) model is shared memory platform
which make the following **assumptions**:

- No caching mechanism
- No NUMA organization
- No synchronization overhead

The PRAM machine is composed by $n$ _identical_ processors connected to a global
shared memory $M$ and any memory location is accessible from any processor in
**constant time**.

![[pram_model.jpg]]

The communication between processors can be implemented by reading and writing
to the global memory $M$.

The PRAM model is a _synchronous_ parallel machine, that means that the $n$
processors operate in _locksteps_ composed by three main phases:

1. **Read**: each processor can read from $M$ at the same time and store the
   data in their local memory.
2. **Compute**: each processor can perform an operation and store the result in
   a register.
3. **Write**: each processor can write data item to a shared memory cell.

Each of these steps takes constant time and the last can be done in some sort of
exclusive way to avoid race conditions, depending on the PRAM _variant_ adopted.

## Complexity

Complexity analysis for PRAM involves two measures:

- **Time complexity** $T(n)$: number of synchronous steps needed to complete the
  algorithm.
- **Processors complexity** $P(n)$: number of processors used to execute the
  algorithm.

These two metrics combined give the formula for the parallel algorithm cost

$$C(n) = T(n) \times P(n)$$

In general, a **cost optimal** algorithm aim for a cost proportional to the best
known sequential algorithm.

### Prefix Computation

A **prefix** is a general binary associative operation

$$\circ: X \times X \to X$$

for which is valid the following property

$$(x_i \circ x_j) \circ x_k = x_i \circ (x_j \circ x_k)$$

and it could be addition, multiplication, maximum, minimum and so on. Given a
set

$$X = \{ x_0, \dots, x_{n-1} \}$$

the **prefix computation** aim to obtain the set

$$S = \{ s_0, \dots, s_{n-1}\}$$

where

$$
\begin{align*}
    s_0 &= x_0 \\
    s_i &= s_{i-1} \circ x_i
\end{align*}
$$

A possible algorithm is called **recursive doubling algorithm** which consider
$n = \#X$ and $p = n$:

```cpp
for (int j = 0; j < n; j++) { // parallel for
   reg = A[j];
   for (int i = 0; i < ceil(log(n)); i++) {
      for (int j = pow(2, i); j < n; j++) { // parallel for
          reg += A[j - pow(2, i)];
          A[j] = reg;
      }
   }
}
```

But for the PRAM model it is not cost optimal because we have to consider that
every processor executes $\log (n)$ operations that gives us a total cost of

$$C(n) = T(N) \times P(N) = n \cdot \log (n)$$

that is worse than the serial version of the algorithm that have $O(n)$
complexity.

> [!example]-
>
> Suppose we have an array of $n = 8$ elements and $p = n = 8$ and we have
> $\circ = +$.
> $$A = \{ 4, 3, 5, 8, 2, 6, 3, 1 \}$$

Actually, in the PRAM model, lowering the number of processors can improve
performance and lead to a cost optimal algorithm for the prefix computation.
Instead of using $p = n$ processors lets use $p = \frac{n}{\log (n)}$ processors
so that we can have a cost of

$$C(n) = T(n) \times P(n) = \log(n) \cdot \frac{n}{\log (n)} = n$$

We also need to slightly change the previous algorithm in a way that it works
with a partitioned input instead of _unit level_ input per processor.

> [!example]-
>
> Better version of recursive doubling algorithm.

#### Sparse Array Compaction

A possible interesting use case for the prefix computation is the \*\*sparse array
compaction where we want to index every non-zero cell of the array.

To do that we can use a temporary auxiliary array containing $0$ where the main
array has zeros and $1$ where the main array has non-zero values. We can now
perform a prefix summation to generate the addresses for the non-zero elements
of the main array.

## Limitations

The PRAM model is ideal but unrealistic in many aspects as

- Uniform memory access: in real systems we have caches and memory hierarchies
  that can vary the overall speed for a read or write operation.
- The synchronization between processors to execute the algorithm is not
  accounted.
- Communication cost not accounted
- Cost optimal algorithms in PRAM might not scale well in real systems with
  limited resources.

In conclusion, PRAM model gives a very clean and simple framework to design
parallel algorithms; however it does not account a lot of real world parameters,
that actually have a huge impact on performance.

## References

- [[parallel_computing]]
