---
title: "Overview"
weight: 100
---

*Draft: crem, 2019-12-xx*

Document contains summary of ideas on potential reimplementation of Lc0 search
algorithm to address limitations of search algorithm.

## Desired improvements

Here is the aspects in which the current search algorithm needs improvements:

* **CPU cache locality.**  
During the forward tree traversal phase, to decide which subtree to route a visit to, current search algorithm has to access **Q** and **N** values from all children, which leads to many CPU cache misses. I believe that this is the main reason of **nps** limit that we have on high end systems (both GPU and CPU are not loaded fully).
* **Transposition support.**  
Currently search tree is a tree. If two different move sequences lead to the same position, it's not used by the search algorithm in any way, making engine to do lots of redundant work, especially in endgames.
* **Scalability.**  
The current implementation of the MCTS algorithm doesn't scale above ≈3 GPUs and ≈3 threads. It would be nice to make it scale better.
* **Dependence on NN eval latency.**  
This is a part of the scalability issue. Currently we rely a lot on latency of a single eval, but cannot utilize larger evaluation throughput (e.g. not able to gather batches larger than 1000).
* **Running out of memory.**  
As Lc0 keeps entire search tree in memory, it's pretty memory hungry and no realistic amount of RAM would allow it to run multi-day search.
* **Discarding tree on take-back.**  
It's usual for people to go back and forth in game tree when analyzing their games. Lc0 discards entire tree in that case, even though sometimes it has useful subtrees steel in memory.


## Unwritten

* Copying Q and N to parents (also can kill nodes).
* Hash and sharding
* Message passing
* Transposition handling and repetition
* Node serialization
* Nodes GC and material key
* Large batches v3