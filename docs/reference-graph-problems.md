---
layout: default
title: Common Graph Problems
parent: References
---

Common Graph Problems
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



# Shortest path

{: .def }
 A shortest path is any path whose path weight is equal to the shortest path weight.

## using Breadth-First Search
- when all edge weights are equal to one.


## using Dijkstra’s Algorithm
- **Input:** a weighted, directed or undirected graph $$G = (V, E)$$, vertices $$s,t \in V$$, where all edge weights are **nonnegative**.
- **Output:** distance between $$s$$ and $$t$$

### `boost::dijkstra_shortest_paths`
- using the [`weighted_graph`](./reference-bgl.html#weighted-graph)

```cpp
#include <boost/graph/dijkstra_shortest_paths.hpp>
// Time complexity O(V log V + E)
int dijkstra_dist(const weighted_graph& G, int s, int t){
  int n = boost::num_vertices(G);
  std::vector<int> dist_map(n);  // external property
  // apply Dijkstra's algorithm
  boost::dijkstra_shortest_paths(G, s,
      boost::distance_map(
        boost::make_iterator_property_map(dist_map.begin(),
                                          boost::get(boost::vertex_index, G))
      )
  );
  return dist_map[t];
}
```

One can extract useful information from the distance map
```cpp
// distance of node 0 to the furthest
int d = *std::max_element(std::begin(dist_map), std::end(dist_map));
```

## using Bellman-Ford Algorithm
- with negative weights





# Minimum spanning tree (MST)

{: .def }
An acyclic subgraph of $$G$$ connecting all vertices in $$V$$ and having the minimum sum of edge weights


## using Kruskal’s Algorithm
- **Input:** a connected, undirected, weighted graph $$G = (V, E)$$
- **Output:** an edge set $$E' ⊆ E$$ that forms the minimum spanning tree.
- **Note:** works with negative weights


### `boost::krusal_minimum_spanning_tree`
- using the [`weighted_graph`](./reference-bgl.html#weighted-graph)

```cpp
#include <boost/graph/kruskal_min_spanning_tree.hpp>
// O(E·log·E)
void kruskal(const weighted_graph& G){
  std::vector<edge_desc> mst; // vector to store MST edges (not a property map!)
  boost::kruskal_minimum_spanning_tree(G, std::back_inserter(mst));
  for (std::vector<edge_desc>::iterator it = mst.begin(); it != mst.end(); ++it){
    std::cout << boost::source(*it, G) << " " 
              << boost::target(it, G) << '\n';
  }
}

```


# Maximum matching
- using Edmond’s Algorithm

# Strongly connected components
- using Tarjan’s Algorithm


# Connected components

# Biconnected components

# Articulation Points

# Bipartite Graph






