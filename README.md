import networkx as nx
import matplotlib.pyplot as plt
from collections import deque


def bfs_spanning_tree(graph, start):
    visited = set([start])
    queue = deque([start])
    parent = {start: None}
    order = []

    while queue:
        u = queue.popleft()
        order.append(u)

        for v in graph.get(u, []):
            if v not in visited:
                visited.add(v)
                parent[v] = u
                queue.append(v)

    return order, parent


# -------- Input Section --------
graph = {}
n = int(input("Enter number of nodes: "))

for i in range(n):
    node = input(f"Enter node {i + 1}: ")
    neighbors = input(f"Enter neighbors of {node} (space separated): ").split()
    graph[node] = neighbors

start = input("Enter starting node for BFS: ")

# -------- Run BFS --------
bfs_order, parent_map = bfs_spanning_tree(graph, start)

# -------- Original Graph --------
G = nx.DiGraph()
for u in graph:
    for v in graph[u]:
        G.add_edge(u, v)

# -------- BFS Spanning Tree --------
T = nx.DiGraph()
for child, parent in parent_map.items():
    if parent is not None:
        T.add_edge(parent, child)

# -------- Visualization --------
plt.figure(figsize=(14, 6))

# Original Graph
plt.subplot(1, 2, 1)
posG = nx.spring_layout(G)
nx.draw(
    G, posG,
    with_labels=True,
    node_color="lightgray",
    node_size=2000,
    font_size=12,
    arrows=True
)
plt.title("Original Graph")

# BFS Spanning Tree
plt.subplot(1, 2, 2)
posT = nx.spring_layout(T)
nx.draw(
    T, posT,
    with_labels=True,
    node_color="lightgreen",
    node_size=2000,
    font_size=12,
    arrows=True
)
plt.title(f"BFS Spanning Tree\nTraversal: {' -> '.join(bfs_order)}")

plt.show()
