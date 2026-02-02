---
id: shared_memory_systems
aliases: []
tags:
  - master
  - parallel_computing
---

# Shared Memory Systems

## Cache

Tipicamente le CPU moderne hanno tre livelli di **cache**:

- L1 è molto piccola ma molto veloce (0.5 - 1 ns).
- L3 è più grande ma più lenta (15 - 40 ns).
- RAM molto grande ma lenta (50 - 100 ns).

A seconda dell'architettura possiamo avere alcuni livelli di cache privati per
il singolo core (L1), mentre altri possono essere condivisi (L3).

## References

- [[parallel_architectures]]
