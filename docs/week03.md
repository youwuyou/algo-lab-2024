---
layout: default
title: Week 03
parent: Fundamentals
---

Week 03: 
===
{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>





# PoW: James Bond's Sovereigns

- Goal: find the largest possible winnings that passenger $$p_{k}$$ can collect regardless of how other passengers play

**Splitting procedure of passengers**

Passengers act in order
- $$p_{0}$$ can pick
  - left most $$s_{0}$$ or right most $$s_{n-1}$$

One cycle, starts with $$p_{0}$$, ends with $$p_{m-1}$$
- For $$p_{1}$$
  - if $$s_{0}$$ picked: choice between $${s_{1}, s_{n-1}}$$
  - if $$s_{n-1}$$ picked: choice between $${s_{0}, s_{n-2}}$$

The cycle starts with $$p_{0}$$ again and repeats until no coins left.

# First steps with BGL

**Goal:** Given a connected, weighted, undirected graph, compute the total weight of its minimum spanning tree and the distance from node `0` to a node furthest from it.

<details open markdown="block">
  <summary>
    Input
  </summary>
  {: .text-delta }
- 1st line with no. vertices `n` and no. edges `m` of the graph
- m consecutive lines, each with three integers representing a weighted edge 
  - `ep1 [space] ep2 [space] edge_weight`
  - non-negative weights at most 1000
{:toc}
</details>


<details open markdown="block">
  <summary>
    Output
  </summary>
  {: .text-delta }
- single line of `w` the sum of weights of all edges of a minimum spanning tree, and `d` the distance from node `0` to a node furthest from it.
  - `w [space] d`
{:toc}
</details>

## Solution Technique
- this exercise serves as a gentle introduction to the boost graph library
- to find the MST one uses the [Kruskal's Algorithm](./reference-graph-problems.html#using-kruskals-algorithm), and to obtain the distance from node `0` to a node furthest from it one can use [Dijkstra's Algorithm](./reference-graph-problems.html#using-dijkstras-algorithm).
- to iterate through the weighting map and subtract the sum of all weights one can use [`std::accumulate`](https://en.cppreference.com/w/cpp/algorithm/accumulate) and to find the maximum distance [`std::max_element`](https://en.cppreference.com/w/cpp/algorithm/max_element).

# Buddy Selection 	

<details open markdown="block">
  <summary>
    Input
  </summary>
  {: .text-delta }

{:toc}
</details>


<details open markdown="block">
  <summary>
    Output
  </summary>
  {: .text-delta }

{:toc}
</details>

# Ant Challenge 	

<details open markdown="block">
  <summary>
    Input
  </summary>
  {: .text-delta }

{:toc}
</details>


<details open markdown="block">
  <summary>
    Output
  </summary>
  {: .text-delta }

{:toc}
</details>

# Important Bridges

<details open markdown="block">
  <summary>
    Input
  </summary>
  {: .text-delta }

{:toc}
</details>


<details open markdown="block">
  <summary>
    Output
  </summary>
  {: .text-delta }

{:toc}
</details>