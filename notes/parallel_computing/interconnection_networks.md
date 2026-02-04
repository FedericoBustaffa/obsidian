---
id: interconnection_networks
aliases: []
tags:
  - master
  - parallel_computing
---

# Interconnection Networks

In the field of distributed parallel computing, **interconnection networks** are
a main topic to ensure high performance. We go from raw hardware to routing
algorithms, ending up in high level graph theory.

The main metrics involved are

- **Latency**: time from the start of a package transmission to its complete
  reception.
- **Throughput**: amount of data sent per unit of time.
- **Bandwidth**: theoretical maximum data transfer rate.

When the throughput reaches the bandwidth value, we talk about **saturation
throughput**, defined as the amount of data per unit of time, that can sustain
the network. Above that throughput the latency grows fast due to contention of
communication resources.

## Network Topologies

In network topologies, the main components are three:

- **Endpoints**: sources and destinations of messages, typically the computing
  nodes of a cluster.
- **Switches**: intermediate device that receives and sends packages accordingly
  to routing algorithms. It helps for example in broadcasting operations,
  letting the computing nodes free.
- **Links**: physical interconnection between nodes and switches like wires.

Then network topologies are divided in

- **Direct**: there are no switches; all nodes are both endpoints and swithes.
- **Indirect**: endpoints are connected through switches.

### Direct Networks

In a **linear array** topology the $n$ nodes are connected sequentially one to
each other.

![Linear Array Topology|700](linear_array_network.png)

It has a degree of $2$, a diameter of $n-1$ and a bisection width of $1$.

In **mesh** the $n$ nodes are organized in a **grid** where all the nodes are
connected to their neighbors in the grid. For simplicity we consider a square
grid of $n = d \times d$ nodes.

![Mesh Topology|400](mesh_network.png)

This topology has a degree of $4$ (constant), a diameter of $2 \cdot (d-1)$ ($O
\left( \sqrt{n} \right)$) and a bisection width of $d$ ($O \left( \sqrt{n}
\right)$).

A **torus** is an extended version of mesh where an extra link is added; this
link connect a node on one edge to the opposite edge, creating a circle.

![Torus Topology|400](torus_network.png)

This topology has a degree of $4$ (constant like the mesh), a diameter of $d/2 +
c/2$ ($O \left( \sqrt{n} \right)$) and a bisection width of $\min{(2c, 2d)}$ ($O
\left( \sqrt{n} \right)$).

In a **binary tree** topology we have nodes organized exactly in a binary tree
shape.

![Binary Tree Topology|400](binary_tree_network.png)

This topology has a degree of $3$ (constant), a diameter of $2d$ ($O \left(
\log{(n)} \right)$) and a bisection width of $1$ (constant).

The **hypercube** topology organizes nodes in a $d$-dimensional hypercube, where
every node has $2^d$ bit strings for identification. Bit strings also provide
positional informations; two nodes are adjacent if and only if the two bit
strings associated with the two nodes, differ in exactly one bit position.

![Hyper-Cube Topology|400](hypercube_network.png)

This topology has a degree of $d$ ($O \left( \log{n} \right)$), a diameter of
$d$ ($O \left( \log{(n)} \right)$) and a bisection width of $n/2$ ($O(n)$).

### Indirect Networks

One of the most common **indirect networks** is the **fat tree** topology, where
we have a binary tree composed by switches (intermediate nodes) and leaves
(endpoints).

![Fat-Tree Topology|500](fat_tree_network.png)

As we can see in the figure, all nodes have only one link to a switch, but the
higher is the level where the switch is, the higher is the number of
connections.

The problems with this topology are the cost that increase with the depth of the
three and, most important, the number of connections is not realistic.

A more realistic implementation uses switches with a **limited degree** and
organized in a **multistage network**.

![Fat-Tree Topology|500](fat_tree_network2.png)

For this topology we have to consider $k$ (the maximum degree of every node) and
$l$ the number of levels of the tree. For a tree with $l$ levels we have

$$k + \frac{k}{l}$$

switches in total and

$$n = k \times \frac{k}{l}$$

as maximum number of endpoints. As we can see every switch has the same number
of ports, so we have network degree of $k$ (constant), a diameter of that is
still $k$ and a bisection width of $n / l$.

One of the most utilized topology in the field of distributed systems with an
high number of nodes is the **dragonfly**. that still has a hierarchical
organization but quite different from a fat tree.

There are three levels: _router_, _group_ and _system_. Each **router** has
connections to $p$ endpoints, $a-1$ local channels and $h$ global channels. A
**group** consists of $a$ routers and each group has $a \cdot p$ connections to
endpoints and $a \cdot h$ connections to global channels.

![Dragonfly Topolgy|450](dragonfly_network.png)

In total this topology has a degree of $p + (a-1) + h$, a constant diameter and
large bisection width.

## References

- [[distributed_memory_systems]]
