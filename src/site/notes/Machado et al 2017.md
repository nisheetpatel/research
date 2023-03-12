---
{"dg-publish":true,"permalink":"/machado-et-al-2017/","created":"","updated":""}
---

[[Hierarchical RL\|Hierarchical RL]]

# A Laplacian framework for option discovery in RL
## Core ideas
- They come up with a method to automatically learn *useful* options
- The method relies on *proto-value functions* (PVFs) - eigenvectors of the graph laplacian - to learn options automatically. Hence the title.
- They learn useful options by defining *eigenpurposes* - intrinsic reward functions of PVFs
- By doing so, agents learn policies for options called *eigenbehaviors* that traverse the principal directions of the state-space
- Turns out that the different options automatically act at different time and thus are helpful for exploration
- They show the usefulness on tabular domains as well as Atari games, though the approach for Atari uses linear function approximation and doesn't seem very powerful

## Some details and definitions
- Graph laplacian is formally defined as $L = D - A$, where $D$ is the degree matrix and $A$ is the adjacency matrix. They use the normalized graph laplacian: $L = D^{-1/2}(D-A)D^{-1/2}$. Intuitively, the graph laplacian captures the actual distance-based geometry of the state-space rather than in the Euclidean space. Thus, if two states are very close but separated by a wall, then they will be close in actual space but not actually reachable. The graph laplacian takes care of this.
- Proto value functions are eigenvectors of the graph laplacian
	- Typically, however, the *smallest* eigenvectors (or the *smoothest*) are the most useful ones. Read Mahadevan & Maggioni 2007 for understanding why.
- Eigenpurposes are intrinsic rewards of PVFs
- Eigenbehaviors are policies that maximize the eigenpurposes
- Since they are not related to actual rewards in the environment, they are just 

## Points to note
- In large state-spaces such as in Atari, learning PVFs accurately is not straightforward. They propose a method based on constructing this via a sampling-based approach, but I suspect it won't perform nearly as well as you'd want it to, partly because they use linear function approximation (for reasons I don't understand right away; might have to dig into more details)0