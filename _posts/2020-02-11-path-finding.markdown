---
layout: post
title:  "Path Finding Algorithm"
categories: algorithm
published: false
---

This article is to summerize some of the most popular and useful path finding algorithms. I also include the `Python` implementation as a reference. Lets go writing some codes!

# Define a graph

There are many ways to define a graph $$G(\mathcal{V}, \mathcal{E})$$, where $$\mathcal{V}$$ is the set of nodes/vertexs and $$\mathcal{E}$$ is the set of the edges. Assume the graph is __static__ so its structure doesn't change. Let $$n = | \mathcal{V}|$$ be the number of nodes. Without loss of generality, we could use the integer from $$0$$ to $$n-1$$ to represent the nodes $$\{v_0, \cdots, v_{n-1}\}$$. 

## Adjacent matrix
