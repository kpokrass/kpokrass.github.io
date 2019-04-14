---
layout: post
title:      "Graph Theory with NetworkX"
date:       2019-04-14 21:58:07 +0000
permalink:  graph_theory_with_networkx
---

Graph theory is the mathematical study of structures used to model relationships between objects, be them physical, biological, social, or informational. Because many practical problems can be represented by a graph, graph theory’s applications extend beyond the realm of mathematics to areas such as linguistics and the social sciences. 

A graph is composed of two main elements: nodes (sometimes called vertices) and edges (sometimes called links). Nodes are the entities/objects of the analysis and edges are the connections between nodes. The edges can be undirected, in that the orientation of the two nodes does not matter for the connection, or directed (sometimes called an arrow or arc), where the nodes joined by a directed edge are an ordered pair.

Network theory is a branch of graph theory that allows the researcher to analyze the role distinct elements (nodes) play on the other elements (also nodes) to which they are connected (edges) by assigning attributes to the graph components. Attributes can be assigned to both nodes and edges and provide additional information about the object. A common attribute for edges is to indicate the strength of the relationship between two nodes by “weighting” the edge with a particular value. 

NetworkX is a python package that can be used for graph creation and analysis, ultimately leading to the development of predictive models for real-world phenomena. It comes with many algorithms for detailed network analysis and allows for a high level of flexibility in terms of creating and defining the graph components.

### Installation

NetworkX requires python 3.5, 3.6, or 3.7 (at the date of this publication) and utilizes many matplotlib functions. Thus, to get started with NetworkX, the user must have the correct version of python installed. The official documentation for installing NetworkX can be found [here](https://networkx.github.io/documentation/stable/install.html). The code block below summarizes the commands needed to do this from your Jupyter Notebook:

```
! pip install networkx
```

The code above installs NetworkX system-wide. You can also install it to just your user directory with the code below:

```
! pip install –user networkx
```

### Getting Started

To import NetworkX into your notebook, follow the code below:

```
import networkx as nx
```

Because most of the NetworkX functions take in a graph object as an argument, the first step is to instantiate a graph. NetworkX provides four basic graph classes: Graph, DiGraph, MultiGraph, and MultiDiGraph. The Graph class implements an undirected graph and ignores multiple edges between two nodes. The DiGraph class is a subclass of the Graph class and implements a graph with directed edges. The MultiGraph class creates a graph that allows for multiple, undirected edges between nodes. Lastly, the MultiDiGraph class creates a graph that allows for multiple, directed edges between nodes.

This discussion will instantiate the Graph class. However, the other classes can be used by substituting the desired class for “Graph” in the code below:

```
G = nx.Graph()
```

### Adding Nodes and Edges

Nodes and edges can be added individually with the `add_node() `and `add_edge() `methods demonstrated in the following code:

```
# A node can be any hashable object, even another graph
G.add_node(1)
G.add_node(‘two’)
G.add_node(‘C’)

# Edges are adding in the form of source to target
G.add_edge(1, ‘two’)
G.add_edge(‘two’, ‘C’)
G.add_edge(‘C’, 1)
```

Nodes and edges can also be added from iterables.

```
list_nodes = [‘A’, ‘B’, 3, 4, ‘five’]
G.add_nodes_from(list_nodes)

list_edges = [(‘A’, 3), (‘B’, 4), (3, ‘B’), (‘five’, 4)]
G.add_edges_from(list_edges)
```

Nodes and edges can even be added from a pandas dataframe. It should be noted that this method creates a new graph and thus should be assigned to a variable for future manipulation. Set the source and target parameters to the names of the columns housing the nodes for each edge in the graph. If the dataframe includes columns specifying node/edge attributes, set the edge_attr parameter to True. Use the create_using parameter to specify which graph type to create (the default is nx.Graph).

```
H = nx.convert_matrix.from_pandas_edgelist(dataframe, source=’column_name1’, target=’column_name2’, edge_attr=True, create_using=nx.Graph)
```

Attributes can be added to the nodes and edges upon initial addition or after they are already a part of the graph.

```
# Adding attributes while adding to the graph
G.add_node(6, {‘name’: ‘Number Six’, ‘size’:400, ‘color’:’palegreen’})
G.add_edge(‘A’, 6, {‘weight’: 3})

# Adding attributes to existing nodes and edges
G[1][‘name’] = ‘Number One’
G[1][‘size’] = 150
G[1][‘color’] = ‘lightskyblue’
G[‘two][‘C’][‘weight’] = 5
```

The nodes, edges, and their attributes for a graph can be called with the following commands.

```
# Call the nodes of a graph
G.nodes()

# Call the edges of a graph
G.edges()

# Call specific attribute of the nodes
nx.get_node_attributes(G, ‘name’)

# Call specific attribute of the edges
nx.get_edge_aatributes(G, ‘weight’)

# Call all the attributes of a specific node
G[1]

# Call all the attributes of a specific edge
G[‘A’][6]

# Call a specific attribute of a specific node
G[1][‘size']

# Call a specific attribute of a specific edge
G[‘two'][‘C’][‘weight’]
```

### Visualizing the Graph

The nodes and their connections can be visualized in a default form with the following code:

```
nx.draw(G, with_labels=True)
```

Some of the ways a graph can be customized are demonstrated in the block below. A more complete outline of graph customizations can be found [here](https://networkx.github.io/documentation/stable/reference/generated/networkx.drawing.nx_pylab.draw_networkx.html#networkx.drawing.nx_pylab.draw_networkx).

```
import matplotlib.pyplot as plt
%matplotlib inline

# Set the layout of the graph
pos 1= nx.kamada_kawai_layout(G)

# Create a list of the node labels to display on the graph
n_labels = nx.get_node_attributes(G, ‘name’)

# Create a list of the edge weights to format the graph
e_weight = nx.get_edge_attributes(G, ‘weight’)

# Create a list of the node colors for the graph
n_colors = nx.get_node_attributes(G, ‘color’)

# Draw the graph with the customizations
nx.draw(G, pos=pos1, node_color=n_color, labels=n_labels, width=e_weight)
```

### Analyzing the Graph


A path in graph theory is defined as a sequence of edges that connects two distinct nodes. A simple path is one that has no repeated nodes. This may, or may not, differ from the shortest path which is a path between two nodes with the fewest number of edges. For directed graphs, the paths are traced obeying the restrictions of the arcs. For weighted graphs, the path length is the sum of the edge weights. Thus, the shortest weighted path may not necessarily have the fewest edges. Path lengths give an idea of how “tight” a network is and can be used to determine how quickly/easily something moves within the network.

```
# Determine if a path exists between two nodes
nx.has_path(G, ‘two’, ‘five’)

# Return all the simple paths
list(nx.all_simple_paths(G, ‘A’, ‘five’)

# Return the shortest path
nx.shortest_path(G, ‘A’, ‘five’)

# Calculate the length of the shortest path
nx.shortest_path_length(G, ‘A’, ‘five’)
```

#### Network Centrality

The concept of centrality aids in the identification of the “important” nodes in a network. The importance of a node is relative to the aim of the study, and thus their multiple ways to measure centrality. A complete list of NetworkX centrality algorithms can be found [here](https://networkx.github.io/documentation/stable/reference/algorithms/centrality.html).

***Degree Centrality***

The degree of a node refers to the number of edges connected to a particular node. For directed graphs, there is both an inflow degree and an outflow for each node.

```
G.degree(‘B’)
```

NetworkX calculates degree centrality by normalizing a node’s degree by n – 1, where n is the total number of nodes in the graph.

```
# Get the degree centrality of a specific node
nx.degree_centrality(G)[‘B’]

# Get the degree centrality for all nodes in the graph
degree_centrality = nx.degree_centrality(G)

# Sort the nodes based on degree centrality in descending order
sorted(degree_centrality.items(), key=lambda x:x[1], reverse=True)
```

***Closeness Centrality***

The measure of closeness centrality is a deeper analysis of the shortest path for nodes as it is standardized by possible minimums for graphs of the same size and connections. In simple terms it is the average length of the shortest path between a particular node and all other nodes on the graph. Therefore, central nodes are closer to a greater number of other nodes in the graph.

```
# Get the closeness centrality of a specific node
nx.closeness_centrality(G)[1]

# Get the closeness centrality for all the nodes in a graph
close_centrality = nx.closeness_centrality(G)

# Sort the nodes based on closeness centrality in ascending order
sorted(close_centrality.items(), key=lambda x:x[1], reverse=False)
```

***Betweenness Centrality***

The measure of betweenness centrality quantifies the number of times a node is present in the shortest path between other nodes in the graph. A node with a high betweenness has a higher probability of occurring in the randomly chosen shortest path between two randomly chosen nodes.

```
# Get the betweenness centrality of a specific node
nx.betweenness_centrality(G)[1]

# Get the betweenness centrality for all the nodes in a graph
between_centrality = nx.betweenness_centrality(G)

# Sort the nodes based on betweenness centrality in descending order
sorted(between_centrality.items(), key=lambda x:x[1], reverse=True)

# The weights of the edges can be included in the measure by setting the weight parameter to the name of the corresponding edge attribute
nx.betweenness_centrality(G, weight=’attr_name’)
```

***Eigenvector Centrality***

Eigenvector centrality measures how well a node is connected to other nodes that have high connectivity in the graph. Simply, it is the measure of the influence of a node in a network.

```
# Get the eigenvector centrality of a specific node
nx. eigenvector_centrality(G)[1]

# Get the betweenness centrality for all the nodes in a graph
eigen_centrality = nx. eigenvector_centrality(G)

# Sort the nodes based on betweenness centrality in descending order
sorted(eigen_centrality.items(), key=lambda x:x[1], reverse=True)

# The weights of the edges can be included in the measure by setting the weight parameter to the name of the corresponding edge attribute
nx.eigenvector_centrality(G, weight=’attr_name’)
```

### Conclusion

This discussion has provided the basic framework for creating and analyzing a graph utilizing NetworkX. Graph theory has a multitude of applications for modern problems and NetworkX is a powerful tool in the emerging field of network science. It is expected that the importance of network analysis and modeling will continue to grow as our world becomes more connected.









