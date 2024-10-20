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

## using Breadth-First Search (BFS)
- when all edge weights are equal to one.

### `boost::breadth_first_search`
- using the unweighted, directed graph [`directed_graph`](./reference-bgl.html#unweighted-directed-graph)

```cpp
#include <boost/graph/breadth_first_search.hpp>

typedef boost::default_color_type color;
const color black = boost::color_traits<color>::black(); // visited by BFS
const color white = boost::color_traits<color>::white(); // not visited by BFS

int n = boost::num_vertices(G);
std::vector<color> vertex_color(n); // exterior property map

boost::breadth_first_search(G, s, boost::color_map(boost::make_iterator_property_map(vertex_color.begin(), boost::get(boost::vertex_index, G))));

// Check if some source vertex `s` can reach every vertex
bool can_reach_all = (std::find(vertex_color.begin(), vertex_color.end(), white) == vertex_color.end());
```

## using Dijkstra’s Algorithm
- **Input:** a weighted, directed or undirected graph $$G = (V, E)$$, vertices $$s,t \in V$$, where all edge weights are **nonnegative**.
- **Output:** distance between $$s$$ and $$t$$

### `boost::dijkstra_shortest_paths`
- using the weighted undirected [`weighted_graph`](./reference-bgl.html#weighted-undirected-graph) or directed graph [`weighted_directed_graph`](./reference-bgl.html#weighted-directed-graph)

```cpp
#include <boost/graph/dijkstra_shortest_paths.hpp>

// Time complexity O(V log V + E)
int dijkstra_dist(const weighted_graph& G, int s, int t) {
  int n = boost::num_vertices(G);
  std::vector<int> dist_map(n);  // external property

  // apply Dijkstra's algorithm
  boost::dijkstra_shortest_paths(
      G, s,
      boost::distance_map(
          boost::make_iterator_property_map(
              dist_map.begin(), boost::get(boost::vertex_index, G)))
  );

  return dist_map[t];
}
```

One can extract useful information from the distance map
```cpp
// distance of node 0 to the furthest
int d = *std::max_element(std::begin(dist_map), std::end(dist_map));
```

### Reconstructing the Path

Additional to `dist_map` recording distances from source node to other nodes in the graph, we need to keep track of the predecessor list.

>  *"for each vertex u in V, `p[u]` will be the predecessor of u in the shortest paths tree (unless `p[u] = u`, in which case u is either the source or a vertex unreachable from the source)."*

```cpp
// Given some weigthed graph `G`
int n = boost::num_vertices(G);
std::vector<int> dist_map(n);
std::vector<vertex_desc> pred_map(n); // for recording predecessors
// apply Dijkstra's algorithm
int s = 0; // source node `0`
boost::dijkstra_shortest_paths(G, s,
  boost::distance_map(
    boost::make_iterator_property_map(dist_map.begin(),
                      boost::get(boost::vertex_index, G))
  ).predecessor_map(
    boost::make_iterator_property_map(pred_map.begin(),
                      boost::get(boost::vertex_index, G))
  )
);
// Reconstruct path from source `s` to `t`
int t = 3; // destination node `3`
std::vector<vertex_desc> path;
path.clear(); path.push_back(t);
while (s != t){
  t = pred_map[t]; // set the end to its predecessor
  path.push_back(t);
}
std::reverse(path.begin(), path.end());
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


### `boost::kruskal_minimum_spanning_tree`
- using the weighted, undirected graph [`weighted_graph`](./reference-bgl.html#weighted-undirected-graph)

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

A **matching** in a graph is a set of edges where no two of which share an endpoint, formally:

{: .def}
A set of edges $$M ⊆ E(G)$$ in a graph $$G$$ is called a **matching** if $$e ∩ e' = \empty$$ for any pair of edges $$e, e' \in M$$.

---

If a matching $$M$$ cannot be appended with more edges, without violating the constraint that it remains a matching, it is a **maximal matching**.

{: .def}
  A **maximal matching** is a matching $$M$$ of a graph $$G$$ that is not a subset of any other matching.

---
**Maximum matching** is also known as maximum-cardinality matching, and is a matching that contains the largest possible number of edges. There may be many maximum matchings.

{: .def}
  A matching $$M$$ is said to be **maximum** if for any other matching $$M'$$, $$\lvert M \rvert \geq \lvert M' \rvert $$

{: .warning}
Greedy algorithms may fail. Every maximum matching is maximal, but not every maximal matching is a maximum matching.

---
Another concept is the **perfect matching**, a matching is perfect if it covers all vertices of a graph.

{: .def}
  A matching is perfect if $$\lvert M \rvert = \frac{\lvert V(G) \rvert}{2} $$




## using Edmond’s Algorithm
- **Input:** an unweighted, undirected graph $$G = (V, E)$$
- **Output:** a set of edges $$M ⊆ E$$ such that $$\lvert M \rvert$$ is maximum and no two edges in $$M$$ share any endpoint.

### [`boost::edmonds_maximum_cardinality_matching`](https://www.boost.org/doc/libs/1_76_0/libs/graph/doc/maximum_matching.html)
- using the unweighted, undirected graph [`graph`](./reference-bgl.html#unweighted-undirected-graph)

```cpp
#include <boost/graph/max_cardinality_matching.hpp>
typedef boost::graph_traits<graph>::vertex_descriptor vertex_desc;

void maximum_matching(const graph& G) {
  int n = boost::num_vertices(G);
  std::vector<vertex_desc> mate_map(n); // exterior property map
  const vertex_desc NULL_VERTEX = boost::graph_traits<graph>::null_vertex();
  // O(mn ɑ(m,n)), where ɑ(m,n) <= 4
  boost::edmonds_maximum_cardinality_matching(
      G, boost::make_iterator_property_map(mate_map.begin(),
      boost::get(boost::vertex_index, G)));

  int matching_size = boost::matching_size(G,
      boost::make_iterator_property_map(mate_map.begin(),
      boost::get(boost::vertex_index, G)));

  for (int i = 0; i < n; ++i) {
    if (mate_map[i] != NULL_VERTEX && i < mate_map[i]) {
      std::cout << " " << mate_map[i] << '\n';
    }
  }
}
```



# Strongly connected components

{: .def}
  A **strongly connected component** of a directed graph $$G = (V, E)$$ is any maximal subset of vertices $$C ⊆ V$$ such that all vertices in $$C$$ are pairwise reachable.

## using Tarjan’s Algorithm
- **Input:** a directed, unweighted graph $$G = (V, E)$$
- **Output:** the number of strongly connected components in $$G$$.
- **Note:** Tarjan's algorithm is based on DFS

### `boost::strong_components`
- using the unweighted, directed graph [`directed_graph`](./reference-bgl.html#unweighted-directed-graph)

```cpp
#include <boost/graph/strong_components.hpp>
void strong_connected_comp(const graph &G){
  int n = boost::num_vertices(G);
  std::vector<int> scc_map(n); // exterior property map
  // Tarjan's Algorithm O(V+E)
  int nscc = boost::strong_components(G,
      boost::make_iterator_property_map(scc_map.begin(),
      boost::get(boost::vertex_index, G)
      )
  );
  std::cout << "Index of SCC current vertex belongs to: " << nscc << '\n';
  for (int i = 0; i < n; ++i){
    std::cout << i << " " << scc_map[i] << '\n';
  }
}
```

# Connected components

{: .def}
A **connected components** of a undirected graph $$G = (V, E)$$ is any maximal subset of vertices $$C⊆V$$ such that all vertices in $$C$$ are pairwise reachable.

# Biconnected components

{: .def}
A **biconnected graph** is an undirected graph that is connected, and remains connected even if a vertex is removed.

{: .def}
A **biconnected component** is any maximal subgraph of $$G$$ that is biconnected.

## Articulation Points

{: .def}
An **articulation point** of an undirected graph is any vertex part of two or more biconnected components

## using a variant of Tarjan's Algorithm [^1]
- **Input:** an undirected graph $$G = (V, E)$$
- **Output:** the number of biconnected components in $$G$$.

### [`boost::biconnected_components`](https://www.boost.org/doc/libs/1_61_0/libs/graph/doc/biconnected_components.html)
- using the unweighted, undirected graph [`graph`](./reference-bgl.html#unweighted-undirected-graph)

```cpp
#include <boost/graph/biconnected_components.hpp>
#include <boost/property_map/property_map.hpp>

// Run biconnected components algorithm on graph G
std::map<edge_desc, int> map;
int nbc = boost::biconnected_components(G, boost::make_assoc_property_map(map));
```

# Bipartite Graph
{: .def}
A graph $$G = (V,E)$$ is **bipartite** if $$V$$ can be split in two subsets $$U, V$$ such that all edges in $$E$$ have an extremity in each.


---

[^1]: 
    [R Tarjan, Depth-first search and linear graph algorithms, SIAM journal on computing 1 (2), 146-160](https://epubs.siam.org/doi/10.1137/0201010)



