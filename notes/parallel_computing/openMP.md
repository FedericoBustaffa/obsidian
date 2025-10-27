---
id: openMP
aliases: []
tags:
  - master
  - parallel_computing
---

# OpenMP

**OpenMP**, that stands for _Open Multi-Processing_ is, a standard framework for
single node parallel programming that offers a _pragmatic approach_ (uses
`#pragma` directives). One of the key idea is to leave the sequential code the
same and, simply adding some directives, parallelism is exploited.

```cpp
#pragma omp parallel for
    for (int i = 0; i < 10; i++)
        std::cout << i << std::endl;
```

The most simple directive is `omp parallel` that parallelizes a _block_ or, if
there is no block, it parallelize the next instruction encountered. At the end
of any block there is a _barrier_ that let all the threads synchronize. The
threads are spawn one time and will be destroyed only at the end of the program;
until then they get fed with new tasks following some policy.

To use OpenMP the `-fopenmp` flag must be used at compilation time, otherwise
all the pragmas will be ignored. An header `omp.h` is also provided but it is
not necessary to use OpenMP directives; although is necessary to use library
calls.

```cpp
#include <omp.h>

int main()
{
#pragma omp parallel
    { // all the block will be parallelized
        int i = omp_get_thread_num();
        int j = omp_get_num_threads();
        std::printf("hello from thread %d of %d\n", i, j);
    }
}
```

## References

Links: [[parallel_computing]], [[cpp]]
