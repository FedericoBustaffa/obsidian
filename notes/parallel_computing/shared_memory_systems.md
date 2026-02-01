---
id: shared_memory_systems
aliases: []
tags:
  - master
  - parallel_computing
---

# Shared Memory Systems

Consideriamo una CPU a 3GHz con 8 core, ciascuno in grado di eseguire 16
operazioni in virgola mobile per ciclo di clock. Il **picco** massimo di
performance raggiungibile è dato dalla formula

$$\mathrm{Peak} = \text{Frequency} \cdot \text{Cores} \cdot \text{FLOP}$$

che nel nostro caso specifico equivale a

$$3 \cdot 8 \cdot 16 = 384 \text{ GFLOPS}$$

Il **picco di trasferimento dati** è invece la massima capacità di trasferimento
dalla RAM ai registri del processore e si indica in byte al secondo. Nel nostro
caso abbiamo una massima capacità di trasferimento dati di $51.2 \text{GB/s}$.

Consideriamo ora il problema di calcolare il **prodotto scalare** tra due
vettori

```cpp
double dot_product(double* u, double* v, int n){
	double dotp = 0.0;
	for (int i = 0; i < n; i++)
			dotp += u[i] * v[i];

	return dotp;
}
```

Se $n = 2^{30}$ abbiamo

$$2 \cdot n = 2 \cdot 2^{30} = 2^{31} = 2 \cdot 10^9$$

operazioni in virgola mobile (somma e prodotto) e

$$
2 \cdot n \cdot 8 \text{ B} = 2 \cdot 10^{9} \cdot 8 \text{ B} >
16 \text{ GB}
$$

di dati trasferiti dalla RAM alla CPU. Otteniamo quindi un tempo totale di
computazione pari a

$$t_\text{comp} = \frac{2 \text{ GFLOP}}{384 \text{ GFLOPS}} = 5.2 \text{ ms}$$

Mentre un tempo totale di trasferimento dati pari a

$$t_\text{mem} = \frac{16 \text{ GB}}{51.2 \text{ GB/s}}$$

Nel caso si riuscisse a mascherare completamente computazione e trasferimento,
il tempo complessivo sarebbe equivalente al massimo di uno dei due.

$$t_\text{exec} \geq \max (t_\text{comp}, t_\text{mem}) = 312.5 \text{ ms}$$

Possiamo quindi ottenere un picco massimo di performance di

$$\frac{2 \text{ GFLOP}}{312.5 \text{ ms}} = 6.4 \text{ GFLOPS}$$

ossia meno del 2% del picco di performance raggiungibile. Possiamo quindi
concludere che il prodotto scalare, sulla piattaforma d'esempio sia limitato dal
trasferimento dati dalla memoria.

## Cache

Tipicamente le CPU moderne hanno tre livelli di **cache**:

- L1 è molto piccola ma molto veloce (0.5 - 1 ns).
- L3 è più grande ma più lenta (15 - 40 ns).
- RAM molto grande ma lenta (50 - 100 ns).

A seconda dell'architettura possiamo avere alcuni livelli di cache privati per
il singolo core (L1), mentre altri possono essere condivisi (L3).

## References

- [[parallel_architectures]]
