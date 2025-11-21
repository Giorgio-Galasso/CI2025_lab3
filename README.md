# Laboratory 3: Shortest Path Finding on Graphs

This repository contains the solution for Laboratory 3. The main objective of the project is to compute the shortest paths between all pairs of nodes (All-Pairs Shortest Path) on procedurally generated graphs, under various configurations (size, density, noise, negative weights).

## Strategies Implemented

To solve the problem efficiently under different constraints, I implemented and compared two main approaches: **Uninformed Search** and **Informed Search**.

### 1\. Uninformed Search Strategies

These algorithms explore the graph without any prior knowledge of the goal's position.

  * **Dijkstra's Algorithm:** Used for graphs with **positive weights**. It is the most efficient standard solution for this case.
  * **Bellman-Ford Algorithm:** Used specifically when the graph contains **negative edge weights** (`negative_values=True`). Since Dijkstra cannot handle negative weights, Bellman-Ford is necessary to ensure correctness and to detect negative cycles.

### 2\. Informed Search Strategies

These algorithms use a heuristic to estimate the cost to the goal, prioritizing the exploration of promising nodes.

  * **A\* (A-Star):** I implemented A\* using **Euclidean Distance** as the heuristic.
      * *Implementation Note:* Since the provided problem generator returns a distance matrix, I reconstructed the node coordinates using the deterministic random seed to calculate the heuristic.
      * *Results:* A\* proved to be significantly more efficient than Dijkstra in terms of "Visited Nodes," especially on dense graphs.
  * **Johnson's Algorithm (Reweighting):** To handle **negative weights** while still benefiting from the speed of A\*, I implemented Johnson's reweighting technique. This allows transforming negative weights into positive ones (using potentials calculated via Bellman-Ford), enabling the safe use of A\* even in complex scenarios.

## Benchmarking

The project includes scripts to compare the performance of the algorithms. The benchmarks generate CSV reports containing:

  * **Execution Time:** The wall-clock time taken to find the path.
  * **Nodes Visited:** The number of nodes extracted from the priority queue (a metric of algorithmic efficiency).
  * **Improvement %:** The percentage of nodes saved by using A\* compared to Dijkstra.

## Experimental Findings
Analyzing the benchmark results, several key insights emerged regarding the performance of Informed vs. Uninformed strategies:
1. Computational Trade-offA* demonstrated a massive reduction in the search space, exploring significantly fewer nodes than Dijkstra in low-noise scenarios. However, on smaller graphs, Dijkstra was often slightly faster in terms of raw execution time.
- Reason: A* incurs a computational overhead for calculating the heuristic (Euclidean distance requires square roots) at every step. Dijkstra's per-node operations are simpler (additions only). The time saved by visiting fewer nodes is sometimes outweighed by this mathematical overhead on small datasets.
2. The Impact of Noise ($N$): The Noise level proved to be the most critical parameter affecting A*'s performance.
- Low Noise ($N \approx 0$): The heuristic (Euclidean distance) is an excellent predictor of the actual path cost. A* is highly efficient, moving almost directly toward the target.
- High Noise ($N \to 1.0$): The correlation between physical distance and edge weight weakens. The heuristic becomes less accurate ("misleading"), causing A* to explore significantly more nodes and retreat more often. As $N$ increases, A*'s behavior and performance degrade.

__Conclusion__ 
While A* is algorithmically superior in terms of node efficiency (Search Space pruning), Dijkstra remains a valid and robust competitor for smaller graphs or high-noise environments where the geometric heuristic loses its predictive power.

## Acknowledgments & Tools

This solution was developed with the support of external resources:

  * **Algorithmic References:** The core logic for Dijkstra, Bellman-Ford, A\*, and Johnson's algorithms is based on standard implementations found in algorithm literature and online resources.
  * **AI Assistance:** I utilized an AI coding assistant to help generate comprehensive **code comments** and improve the readability and formatting of the script. The logical implementation and problem-solving strategy remain my own work.