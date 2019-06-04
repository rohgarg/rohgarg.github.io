---
layout: post
title: Polygon Triangulation
date: 2013-04-16 12:33
tags: [computational geometry, polygon, triangulation]
category: Computational Geometry
author: Rohan Garg
summary: Notes on polygon triangulation
---

<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    extensions: [
      "MathMenu.js",
      "MathZoom.js",
      "AssistiveMML.js",
      "a11y/accessibility-menu.js"
    ],
    jax: ["input/TeX", "output/CommonHTML"],
    TeX: {
      extensions: [
        "AMSmath.js",
        "AMSsymbols.js",
        "noErrors.js",
        "noUndefined.js",
      ]
    }
  });
</script>

<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

#### Polygons

 * Definition of a polygon: A set of connected line segments which are
   connected end-to-end and form a cycle
    - Simple polygons (non-adjacent edges should not intersect)
      vs. non-simple polygons
 * Jordan Curve Theorem: Every simple polygon divides the plane into
   two components.
    - circa 1877; complex proof
 * Art gallery problem: What is the min. number of point lights are
   required to fully illuminate a polygon room of n vertices (G(n))?
    - It can be a simple or non-simple polygon of 
    - The light originating from a source at point x can reach point
      y, iff, all points on the line segment xy are in the interior of
      the polygon
    - For 2-D, it is easy to see that $$ 1 \le G(n) \le n $$.
        + The upper bound is not true for all 3-D polyhedra
    - \$$ G(n) \ge \lfloor(n/3)\rfloor $$
        + Proof Sketch: Every polygon can be triangulated. The graph
                        of the triangulation is in 3-COLOR. Place guards
                        at all the vertices with the same color. Apply
                        the extended pigeon-hole principle.
       + _Question_: Is this bound tight?
       +  There are (n-3) diagonals, and (n-2) triangles. 
          * Sum of angles = $$ (n-2) \times \pi $$
