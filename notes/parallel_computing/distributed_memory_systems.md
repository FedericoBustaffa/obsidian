---
id: distributed_memory_systems
aliases: []
tags:
  - master
  - parallel_computing
---

# Distributed Memory Systems

In the field of parallel computing there are situations where remaining on one
single node is not possible. Can happen that the computation takes to long even
with an ideal parallel algorithm or simply because the amount of data to process
can be huge and does not fit in memory.

So **distribute** computation on multiple nodes can improve performances by a
lot under certain circumstances.

## Interconnection Networks

With these kind of systems one of the most important aspects is the efficiency
in the communication between nodes. Especially in the field of high performance
computing the **interconnection network** between nodes is something that tries
to minimize latency with specific topology and high bandwidth.

Other important aspects that we only cite are the **routing** methods, to
optimize message passing and **flow control**, in order to manage the rate at
which the data _flows_ in order to prevent saturation of the network.

So the three most important metrics are

- **Latency**: expressed in time units, is the time from when a packet starts to
  be transmitted from the source node and when it is wholly received at the
  destination node.
- **Throughput**: expressed in bits per seconds, is the actual amount of data
  sent into the network per unit of time.
- **Bandwidth**: expressed in bits per seconds, is the theoretical maximum data
  transfer rate under ideal conditions across a given network path; represents the
  maximum capacity of the communication channel.

In general we can say that increasing the throughput brings higher latency as
the bandwidth is not infinite and the network will saturate.

### Topologies

Before talking about network **topologies** we must introduce some terminology:

- **Endpoints**: sources and destinations of messages, typically they are the
  computing nodes equipped with a **NIC** (Network Interface Card).
- **Switches**: device connected to a set of links that transmists received
  packets to one or multiple nodes using routing logic to manage multiple and
  simoultaneous flows.
- **Links**: it is a physical connection used to transfer data between endpoints
  and switches.

We can divide networks in two main types:

- **Direct**: the nodes are both endpoints and switches.
- **Indirect**: the endpoints are connected indirectly through switches.

To study and evaluate a topology we must take into account three different
parameters, typical to networks and graphs theory:

- **Degree**: maximum number of neighbors of any node.
- **Diameter**: length of the longest of all shortest paths between any nodes.
- **Bisection width**: the smallest number of links to be removed to disconnect
  the network into two halves of (nearly) equal size. The higher the bisection
  width is the higher is the capacity of the network to move data.

These three parameters are the one that can be used to model a network topology
and understand how does it scale in the number of endpoints, switches and links.

#### Topology Evaluation

In general we can say that a _good_ topology has to have

- **Low diameter**: efficient communication between any pair of nodes.
- **High bisection width**: consequence of having an high level of
  interconnection and more or less well distributed.
- **Constant degree**: allows a network to scale well with large number of nodes
  without the need to add an axcessive number of connections.

## Message Passing

In distributed memory systems the most common way to communicate for two
processes is through **message passing**, a paradigm the involves communication
via messages.

This simplify a lot the synchronization among processes as no synchronization
primitives (like mutex and conditions variables) are involved, enforcing
**isolation** and making **failure handling and scaling** much simpler.

We can define two classes of message passing models and formalisms:

- **Actor Model**: every process is an **actor** and encapsulate state and
  behavior, communicating by sending and receiving messages.
- **Process Calculi**: **CSP** (Communicating Sequential Processes) and
  **$\pi$-calculus** are models and formalism used to describe and analyze
  interactions among concurrent entities.

The three main concepts of message passing models are:

- **Asynchrony** or **synchrony** between sender and receiver: in synchronous
  model send and receive are coupled and with a blocking behavior, in asynchrnous
  model the send is non-blocking and the receive can block or not depending on
  many factors.
- **Non-determinism**: the order in which the messages arrive can vary and the
  systems exhibits input non-deterministic behavior.
- **Ordering/Causality**: there is no clock or global ordering of events and the
  processes observe in fact partial order.

Message passing naturally supports distributed memory systems but can be used
also in shared memory systems and has to deal with deadlocks and livelocks.

### Synchronous vs Asynchronous communications

For asynchronous communication we have something called **asynchrony degree**
that defines the number capacity of the communication channel used to
communicate.

We can think about it as bounded queue: until the queue is not full, we can
continue put messages in it, obtaining a non non-blocking behavior. When the
queue is full a sender must block until at least one message is popped from the
queue. From the standpoint of the sender, the send operation completes almost
immediatly but for the receiver could be that, in case there is no message to
process it has to block.

In a synchronous model a send or receive operation is completed only after the
message is actually sent or received. This provokes a blocking behavior called
**send-receiver rendezvous**.

### Input Non-Determinism

We can have situations where a logical input channel _multiplexes_ a set of
independent channels. In this case we talk about **input non-determinism** if
the receive operation can return a message from any channel in the set.

There is also no fixed order whene receiving messages from a multi-input channel
and if more than one message is ready at once, one is chosen arbitrarily in a
non-predictable order. Also because trying to fix the order is not optimal
because reduce flexibility and add overhead.

### Communication Cost Model

The **linear model** is a very simple model, yet enough to reason about
communication in parallel applications. It describes the **communication time**
in this way

$$
T_\text{comm} = t_0 + n \cdot s \approx \begin{cases}
t_0 & \text{ for small } n \\
n \cdot s & \text{ for large } n
\end{cases}
$$

where $t_0$ is the communication **startup** overhead (also called **latency**),
$n$ is the amount of data to be transferred and $s$ is transmission cost per
byte.

The value of $s$ is often estimated with $1 / B$ where $B$ is available
bandwidth along the transmission path and is limited by the slowest part of the
path.

Modern HPC systems has a $t_0$ of $1$-$10$ $\mu$sec whereas $B$ is $100$-$200$
Gb/s.

This model is simple and so cannot express some aspect of the communication, for
example does not include effect of network distance and does not model
contention in any way, not taking into account the network usage.of network
distance and does not model contention in any way, not taking into account the
network usage. It also does not know if the communication happens synchrnously
or asynchronously.

## Computation to Communication Overlap

Even in a HPC system, communication via message passing can slow down the whole
computation, so is common to try hiding the communication cost and overlap it
with the computation.

To do this we need dedicated hardware that implements mechanisms like DMA (or
RDMA) in order to offload this work from the CPU and let it do only computation.

A **NIC** (Network Interface Card) is used to handle sends and receives data
while the CPU does other and more important computations. If a NIC has more than
simple DMA capabilities, is called **SmartNIC**.

When a sender performs a non-blocking send, the NIC executes the data transfer
and then notifies the sender about the completion of the transmission.

Let's now suppose a module $C$ receives a list of tasks (called **data stream**)
and takes $T_\text{calc}$ time for the computation and $T_\text{comm}$ for
sending the compute task to the next module. The **service time** of a module is
$T_S^C$ is the time interval between the start of processing two consecutives
inputs. We may have

- $T_S^C = T_\text{calc} + T_\text{comm}$ if there is **no overlap**.
- $T_S^C = \max{(T_\text{calc}, T_\text{comm})}$ with **overlap**.

Smart NICs may include RDMA technology which provides **_memory-to-memory_**
data transfer between two nodes without the continous involvement of the
operating system.

### PCAM Approach

The **PCAM** approach is a framework for reasoning about parallelization of a
given problem, proposed by Ian Foster and is composed by 4 steps:

- **Partitioning**: decompose the problem in a large amount of small tasks that
  can be executed in parallel.
- **Communication**: determine the required communication between tasks
  (**dependencies**).
- **Agglomeration**: combine identified tasks into larger tasks to reduce
  communication and improve data locality.
- **Mapping**: assign the aggregated tasks to processes according to the network
  topology to minimize communication, enable concurrency and balance the
  workload to the network topology to minimize communication, enable concurrency
  and balance the workload.

#### Jacobi Iteration

Let's take as example the Jacobi algorithm, where we have a $n \times n$ matrix
and we want to perform a _stencil_ computation, updating every cell with the
average of its four neighbors.

$$
m_{t+1} (i, j) = \frac{m_t (i-1, j) + m_t (i+1, j) +
m_t (i, j-1) + m_t (i, j+1)}{4}
$$

The update rule is applied iteratively until convergence, the boundary values
remain constant at each iteration and at the end of each iteration, swap the
updated array with the original one to avoid overwriting during the next
iteration.

The PCAM approach suggests two different approaches to parallelize the problem,
but the difference is in only step:

1. **Partitioning**: every fine grained task is the computation of one single
   element (the average of 4 neighbors).
2. **Communication**: within an iteration, every fine grained task can be
   computed indepently and each task needs the data of the four neighbors. At
   the end of each iteration there is a synchronization barrier among all $p$
   processors and data is exchanged.
3. **Agglomeration**: this is where the two methods diverge:
   - **Row**: fine grained tasks are combined in coarse grained task
     agglomerating rows.
   - **Square grid**: fine grained tasks are combined in coarse grained task in
     square grid.
4. **Mapping**: the mapping follows the policy chosen at the previous step:
   - **Row**: every processor computes $n / p$ rows where $p$ is the number of
     processors.
   - **Square grid**: every processor computes grids organized in a
     $\sqrt{p} \times \sqrt{p}$ grid

Let's now discuss about the communication cost of the two algorithms
considering the problem size $n \times n$ and the number $p$ of processors (or
nodes) using the linear model.

For the first method we have that every processor own roughly $n / p$ rows (or
columns), so we have a communication time of

$$T_\text{comm} (n) = 2 \cdot (t_0 + n \cdot s)$$

because every process needs to exchange two rows with their "neighbors". For the
other method we have a communication time of

$$T_\text{comm} (n) = 4 \cdot \left( t_0 + \frac{n}{\sqrt{p}} \cdot s \right)$$

because every process has basically four neighbors for exchange data. From the
standpoint of the process, all its communications happen sequentially, but all
the processes run in parallel so we take into account the communication cost of
only one process.

Comparing the two formulas we can see how, in the firs method we obtain a
constant value (considering obviously $t_0$, $n$ and $s$ fixed). In the other
method we have that if we move on a system, increasing the number of $p$, the
communication cost will decrease.

Let's consider for example $n = 1024$, $t_0 = 2$ and $s = 1$ and let's see what
happens varying $p$:

![[jacobi_comparison_plot.png]]

As we can see, the first method can be better for a low number of processors,
but as $p$ increases, the communication cost decreases, so, under certain
circumnstances, the square grid method is better.

## References

- [[parallel_architectures]]
- [[interconnection_networks]]
- [[message_passing]]
