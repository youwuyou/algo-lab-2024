---
layout: default
title: CGAL
parent: References
---

The Computational Geometry Algorithms Library (CGAL)
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

<table>
  <tr>
    <th>2D Kernel Objects</th>
    <th>Representations</th>
  </tr>
  <tr>
    <td>Point_2</td>
    <td rowspan="3">two FTs (Cartesian coordinates)</td>
  </tr>
  <tr>
    <td>Vector_2</td>
  </tr>
  <tr>
    <td>Direction_2</td>
  </tr>
  <tr>
    <td>Line_2</td>
    <td>three FTs (coefficients of line equation)</td>
  </tr>
  <tr>
    <td>Ray_2</td>
    <td rowspan="2">two points</td>
  </tr>
  <tr>
    <td>Segment_2</td>
  </tr>
  <tr>
    <td>Triangle_2</td>
    <td>three points (corners)</td>
  </tr>
  <tr>
    <td>Iso_rectangle_2</td>
    <td>two points, opposite corners</td>
  </tr>
  <tr>
    <td>Circle_2</td>
    <td>point and FT (center and squared radius)</td>
  </tr>
</table>

# Geometric Operations

## Predicates
Constant size results, one can use the unit cost model

- `incircle(p,q,r,?)`
- `leftturn(p,q,?)`

## Constructions

Result is not necessarily constant size

- `intersection(s,t)`
- `circumcenter(p,q,r)`

# Kernels

## `epic`

```cpp
#include <CGAL/Exact_predicates_inexact_constructions_kernel.h>

typedef CGAL::Exact_predicates_inexact_constructions_kernel IK;
```

## `epec`
- Constructions use `double`

```cpp
#include <CGAL/Exact_predicates_exact_constructions_kernel.h>

typedef CGAL::Exact_predicates_exact_constructions_kernel EK;
```

## `sqrt`

- Constructions use an exact number type supporting `+`, `-`, `*`, `/` and roots.


```cpp
CGAL::Exact_predicates_exact_constructions_kernel_with_sqrt
```

## Combining Kernels in a Program

```cpp
#include <CGAL/Exact_predicates_inexact_constructions_kernel.h>
#include <CGAL/Exact_predicates_exact_constructions_kernel.h>

typedef CGAL::Exact_predicates_inexact_constructions_kernel IK;
typedef CGAL::Exact_predicates_exact_constructions_kernel EK;

int main(){
  IK::Point_2 p(2,1), q(1, 0), r(-1, -1);
  IK::Line_2  l(p,q);
  IK::FT d = CGAL::squared_distance(r, l);
  std::cout << d << '\n';

  // do something that needs predicates only, e.g., ...
  std::cout << (CGAL::left_turn(p,q,r)? "y" : "n") << '\n';

  // now we use non-trivial constructions
  EK::Point_2 ep(p.x(), p.y()),
              eq(q.x(), q.y()),
              er(r.x(), r.y());
  EK::Circle_2 c(ep, eq, er);
  if (!c.has_on_boundary(ep))
    throw std::runtime_error("ep not on c");
}
```

# Intersections `CGAL::intersection(r, t)`
- the return type may be a triangle, a line, a rectangle etc...

## Determining the Return Type with `boost::get<T>`

```cpp
#include <CGAL/Exact_predicates_exact_constructions_kernel.h>

typedef CGAL::Exact_predicates_exact_constructions_kernel EK;

typedef EK::Point_2 P;
typedef EK::Segment_2 S;

int main(){

  P p[] = { P(0,0) P(2,0), P(1,0), P(3,0), P(.5, 1), P(.5, -1) };
  S s[] = { S(p[0], p[1]), S(p[2], p[3]), S(p[4], p[5])};

  for (int i = 0; i < 3; ++i){
    for (int j = i+1; j < 3; ++j){
      if (CGAL::do_intersect(s[i], s[j])){
        auto o = CGAL::intersect(s[i], s[j]);
        if (const P* op = boost::get<P>(&*o)){
          std::cout << "point: " << *op << '\n';
        }
        else if (const S* os = boost::get<S>(&*os)){
          std::cout << "segment: " 
                    << os->source()
                    << " "
                    << os->target()
                    << '\n'
        }
        else {
          throw std::runtime_error("strange segment intersection\n");
        }
      }
      else
        std::cout << "no intersection\n";
    }
  }
}
```

# Minimum Enclosing Circles
- **Input:** A set of n points in $$R^{2}$$
- **Output:** Their minimum enclosing circle

## [`CGAL::Min_circle_2<Traits>`](https://doc.cgal.org/latest/Bounding_volumes/classCGAL_1_1Min__circle__2.html)

- expected linear time, use `epec` internally

```cpp
#include <vector>
#include <CGAL/Exact_predicates_exact_constructions_kernel.h>
#include <CGAL/Min_circle_2.h>
#include <CGAL/Min_circle_2_traits_2.h>

// typedefs
typedef CGAL::Exact_predicates_exact_constructions_kernel EK;
typedef CGAL::Min_circle_2_traits_2<EK> Traits;
typedef CGAL::Min_circle_2<Traits> Min_circle;

int main(){
  const int n = 100;
  std::vector<EK::Point_2> P;

  // Construct {(0,0), (-1, 0), (2, 0), (-3, 0), ... }
  for (int i = 0; i < n; ++i){
    P.push_back((((i % 2 == 0)? i : -i), 0));
  }

  // Build from a range of points
  Min_circle mc(P.begin(), P.end(), true); // `true` to randomize input order
  Traits::Circle c = mc.circle(); // construct and return the circle
  std::cout << c.center() << " " << c.squared_radius() << " " << '\n';
}
```

## Speed Consideration

- using `int` is slower than `long` in the "Antenna" example