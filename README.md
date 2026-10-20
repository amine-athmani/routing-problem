# ICPC Communication Routing Challenge 2021

Our solution for the [ICPC Communication Routing Challenge, powered by Huawei](https://codeforces.com/blog/entry/94906) (October 2021).

## Problem

A communication network is given as a multigraph: nodes, and edges with a distance and a capacity. Several parallel edges can link the same two nodes. Each demand asks to route a flow of a given rate from one node to another. A route is valid only if:

- every edge on it has enough capacity left for the flow rate,
- each group of edges and each node stays under its flow limit (100 flows per edge group, 200 per node),
- it never uses a forbidden pair of consecutive edges at a node.

The program reads the instance from stdin and must print its routes within about two seconds.

## Approach

A greedy constructive heuristic in pure Python:

- Demands are routed one at a time, optionally sorted by flow rate. Each accepted route reduces the remaining capacities and limits before the next demand.
- Each route comes from a BFS that checks capacity, group and node limits, and forbidden edge pairs as it extends the path. Among parallel edges, it picks the shortest one (`min_dist`) or the one with the most capacity left (`max_cap`).
- The nodes one and two hops from the destination are computed first, so the search can close a path as soon as it reaches that neighbourhood. The search starts from whichever endpoint has the smaller neighbourhood, and the route is reversed afterwards if needed.
- Paths found between two nodes are cached and reused for later demands with the same endpoints, provided they are still feasible.
- A time guard stops the search before the 2-second limit.

## Files

- `Node.py`, `Edge.py`, `Group.py`, `Demand.py`, `Flow.py`, `Instance.py`: the data model.
- `reader.py` and `writer.py`: input parsing and output.
- `heuristics.py` and `main.py`: the solver, run on `data/input.txt` for local testing.
- `submission*.py` and `Submissions/`: single-file versions sent to the judge, named after the score each one reached. `submission_cache-parents.py` is the last version, with the path cache and the two-hop neighbourhood search.

```bash
python main.py                                       # local run on data/input.txt
python submission_cache-parents.py < data/input.txt  # judge format
```

## License

MIT
