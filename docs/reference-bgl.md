---
layout: default
title: BGL
parent: References
---

The Boost Graph Library (BGL)
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


# Overview


| Algorithm | Data Structure | Runtime |
| --- | --- | --- |
| `boost::breadth_first_search` |  |  $$O(n + m)$$ | 
| `boost::depth_first_search` |  | $$O(n + m)$$ | 
| `boost::dijkstra_shortest_path` |  | $$O(n\log n + m)$$ | 
| `boost::kruskal_minimum_spanning_tree` | Union Find | $$O(m \log m)$$ | 
| `boost::edmonds_maximum_cardinality_matching` |  | $$O(mn \cdot \alpha(m,n)), \alpha(m,n) \leq 4 $$ | 
| `boost::connected_components` |  | $$O(n + m)$$ | 
| `boost::biconnected_components` |  | $$O(n + m)$$ | 
| `boost::articulation_points` |  | $$O(n + m)$$ | 
| `boost::is_bipartite` |  | $$O(n + m)$$ | 



# Selector Types

Specific containers from the STL can be used to configure the underlying structure of the `OutEdgeList` (out-edges) and `VertexList` (in-edges) in the graph represented by an [`adjacency_list`](https://www.boost.org/doc/libs/1_84_0/libs/graph/doc/using_adjacency_list.html).


| Alias | STL Container |
| --- | --- |
| `vecS` | `std::vector` |
| `listS` | `std::list` |
| `slistS` | `std::slist` |
| `setS` | `std::set` |
| `multisetS` | `std::multiset` |
| `hash_setS` | `boost::unordered_set` |


# Graph

## Representation
We represernt a graph $$G = (V, E)$$ as an `adjacency_list`. $$G$$ has `n` vertices and `m` edges.
- Space $$O(n + m)$$


```cpp
#include <boost/graph/adjacency_list.hpp>
typedef boost::adjacency_list<boost::vecS,
                              boost::vecS,
                              boost::undirectedS
                              > graph;

// Initialize graph with 5 vertices {0,.., 4}
graph G(5);
// Add an edge in the undirected & unweighted graph `G`
boost::add_edge(0, 1, G);
// Warning! This extends the list of vertices to {0,..., 7}
boost::add_edge(0, 7, G);
```


### Getting Graph Information

```cpp
int n = boost::num_vertices(G);
```



### Iterating Over Edges
- with the definition of `graph` using the `boost::adjacency_list` as above
- to unpack the iterators, one could use `boost::tie(e_beg, e_end)  = boost::edges(G)` or `std::tie` (C++11) instead.

```cpp
typedef boost::graph_traits<graph>::edge_iterator edge_it;

// iterate over edges in provided undirected graph
edge_it e_beg, e_end;
for (auto [e_beg, e_end] = boost::edges(G); e_beg != e_end; ++e_beg){
      std::cout << boost::source(*e_beg, G) << " "
                << boost::target(*e_beg, G) << '\n';
}
```


## Path reconstruction

## Directed & Undirected Graph


## Weighted Graph

For an undirected graph
```cpp
typedef boost::adjacency_list<boost::vecS, boost::vecS, boost::undirectedS,
                              boost::no_property,
                              boost::property<boost::edge_weight_t, int>
                              > weighted_graph;
typedef boost::graph_traits<weighted_graph>::edge_iterator edge_it;      // edge iterator
typedef boost::graph_traits<weighted_graph>::edge_descriptor edge_desc;  // edge descriptor

typedef boost::property_map<graph, boost::edge_weight_t>::type weight_map; // weight map
// initialize the graph with vertex count `n`, obtain empty weight map
weighted_graph G(5);
weight_map weights = boost::get(boost::edge_weight, G);
// add an edge {0, 1} and set weight to 3
const edge_desc edge = boost::add_edge(0, 1, G).first;
weights[edge] = 3;
```