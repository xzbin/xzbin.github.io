---
title: networkX learning
date: 2018-08-25 15:49:27
categories: 
    - 工程
tags:
    - 图
---

最近一直在看知识图谱相关内容，[networkx](https://networkx.github.io/)是python下一款优秀的图计算处理工具，下文是教程学习笔记。

## A.1 Create a graph

```python
# 创建一个空图
g = nx.Graph()

# 生成一个图 10个节点 9条边的图： 线性连接
gg = nx.path_graph(10)

# 另外两种创建图的方式
g1 = nx.DiGraph(gg)
edgelist = [(0, 1), (1, 2), (2, 3)]
g2 = nx.Graph(edgelist)
print(g2.nodes())

```



## A.2 Nodes

```python
# 增加一个节点
g.add_node("1", {"name": "xzbin", "age": 15})
# 增加一组节点
g.add_nodes_from([2, 1, 3, 45, 7])


# 将图gg中的节点增加到 图g中 下图中两种方式均可以 (此时有个问题：下面两种方式增加节点：会有点的重复)
g.add_nodes_from(gg)
g.add_node(gg)
```



## A.3 Edges

```python
# 方式1
g.add_edge(1, 2)

# 方式2
e = (2, 3)
g.add_edge(*e)

# 方式3
g.add_edges_from([("4", "2"), ("1", "6")])

# 方式4
g.add_edges_from(gg.edges())

# 增加一个节点
g.add_node("spam")
# 增加一组节点
g.add_nodes_from("spam")

```



## A.4 What to user as nodes and edges

## A.5 Accessing edges and neighbors

```python

```

## A.6 Adding attributes to graphs, nodes, and edges

图、节点、边都可以有属性（比如 权重、颜色、标签）。属性都是key-value类型。

### A.6.1 Graph attributes

```python
gg = nx.Graph(day="Friday")
print(gg.graph)

gg.graph['day'] = 'one'
print(gg.graph)
```

### A.6.2 Node attributes

```python
gg = nx.Graph(day="Friday")
gg.add_node('people', name='xzbin')
print(gg.node['people']['name'])
gg.node['people']['name'] = 'xzbin_new'
print(gg.node['people']['name'])
```

### A.6.3 Edge Attributes

```python
gg = nx.Graph(day="Friday")
gg.add_edge(1, 2, {'name': 'xzbin'})
print(gg.get_edge_data(1, 2)['name'])
gg.get_edge_data(1, 2)['name'] = 'xzbin_new'
print(gg.get_edge_data(1, 2)['name'])
```

## A.7 Directed graphs

有向图

```python
g = nx.DiGraph()
g.add_weighted_edges_from([(1, 2, 0.5), (2, 4, 0.9)])
print(g.out_degree(1), g.degree(2))
print(g.in_degree(2, weight='weight'))

print(g.successors(2), g.predecessors(2))

print(g.neighbors(2), g.neighbors(1), g.neighbors(4)) # 需要注意

gg = nx.Graph(g)
print(gg.neighbors(2))
```

## A.8 Multigraphs

允许两个点之间存在多个边

```python
g = nx.MultiDiGraph()
g.add_weighted_edges_from([(1, 2, 0.3), (1, 2, 0.31)])
print(g.get_edge_data(1, 2))
print(nx.shortest_path_length(g, 1, 2,weight=True))

```

## A.9 Graph generators and graph operations

```python
g = nx.Graph()
g.add_edges_from([(1, 2), (2, 3), (4, 5), (3, 5)])
sub_g = nx.subgraph(g, (1, 2, 3))

sub_g = nx.Graph()
sub_g.add_edge(7, 8)
gg = nx.union(g, sub_g)
print(gg.edges())

sub_g1 = nx.Graph()
sub_g1.add_edge(11, 10)
gg = nx.compose(g,sub_g1)
print(gg.edges())

# 各种典型图
import matplotlib.pyplot as plt
petersen = nx.petersen_graph()
tutte = nx.tutte_graph()
maze = nx.sedgewick_maze_graph()
tet = nx.tetrahedral_graph()
nx.draw(tet)
plt.show()
print(petersen.edges())

k5 = nx.complete_graph(5)
k3_5 = nx.complete_bipartite_graph(3,5)
barbell = nx.barbell_graph(10,10)
lollipop = nx.lollipop_graph(10,20)

er = nx.erdos_renyi_graph(100,0.15)
ws = nx.watts_strogatz_graph(20,3,0.1)
ba = nx.barabasi_albert_graph(100,5)
red = nx.random_lobster(100,0.9,0.9)

nx.write_edgelist(k5,'D:\\g.txt')

import matplotlib.pyplot as plt
nx.draw(lollipop)
plt.show()
```

## A.10 Analyzing graphs

```python
g = nx.Graph()
g.add_edges_from([(1, 2), (1, 3)])
g.add_node("spam")
# print(list(nx.connected_components(g)))

# print(g.degree())
print(sorted(d for n, d in g.degree().items()))

print(nx.clustering(g))
```

## A.11 Drawing graphs

前文中 已经体现