---
layout: default
title: Week 05
parent: Advanced Algorithms
---

Week 05: Greedy and Split & List
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


# PoW: Motorcycles

# Moving Books

**Goal:** Find the minimal time to move all the boxes

<details open markdown="block">
  <summary>
    Input
  </summary>
  {: .text-delta }
  - 1st line of `n [space] m`
    - `n` no. friends
    - `m` no. boxes
  - 2nd line of `s₀ [...] sₙ₋₁` strength of every friend
  - 3rd line of weights of boxes `w₀ [...] wₘ₋₁`
{:toc}
</details>

<details open markdown="block">
  <summary>
    Output
  </summary>
  {: .text-delta }
- single line containing minimum time needed to move all boxes, or `impossible` if any box cannot be moved.
{:toc}
</details>


## Problem Modeling

### Cost Calculation

From the cost description we know that the cost to move a box equals `2` min (constant) for all friends, regardless of the weights of the boxes nor the strengths of the friends. And the cost of returning back to the second floor before continuing to move the next box is `1`. 

Since "an unlimited number of friends" can use the stairs, it indicates we can use our resources in parallel. Thus, we maximize the resource distribution in which we increment the `min_time` by at most `3` each round, taking into account of the cost of returning. We need to subtract it by `1` when reporting the final cost, as "all friends are upstairs" at the beginning, but it is "not necessary for all friends to be back upstairs at the end".

## Solution Technique

To find the minimum time to move all the boxes boils down to efficiently manage resources at each round, where each round after the first round costs `3` time unit as we described above.

### Greedy Approach

Since we are allowed to use all resources in parallel, at each round we need to just iterate over all friends and assign tasks to them. We used a greedy approach in box assignment. With friends ordered in an descending order according to their strengths, we always assign the heaviest box available to each friend.

### Choice of Data Structure

One key point of this exercise lies in the efficiency of the solution, otherwise it is impossible to pass test set 2 & 3. We need an efficient choice of the containers:

- `std::vector<int> friends(n)` to store strengths of `n` friends, and we sort it with `std::greater<int>` at the beginning
- `std::multiset<int, std::greater<int>> weights` to store weights, we need efficient removal of elements. The sorted nature of it also gives us a guarantee that we can use `weights.lower_bound(f)` to find the heaviest box that is moveable by a friend.

# Asterix the Gaul

<details open markdown="block">
  <summary>
    Input
  </summary>
  {: .text-delta }
{:toc}
</details>

# Severus Snape

<details open markdown="block">
  <summary>
    Input
  </summary>
  {: .text-delta }
{:toc}
</details>


# Boats

<details open markdown="block">
  <summary>
    Input
  </summary>
  {: .text-delta }
{:toc}
</details>