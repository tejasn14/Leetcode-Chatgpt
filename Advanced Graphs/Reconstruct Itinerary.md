---

---
---
[LC Link](https://leetcode.com/problems/reconstruct-itinerary/)

---
---

```python
class Solution:
    def findItinerary(self, tickets: List[List[str]]) -> List[str]:
        adj = {u: collections.deque() for u, v in tickets}
        res = ["JFK"]

        tickets.sort()
        for u, v in tickets:
            adj[u].append(v)

        def dfs(cur):
            if len(res) == len(tickets) + 1:
                return True
            if cur not in adj:
                return False

            temp = list(adj[cur])
            for v in temp:
                adj[cur].popleft()
                res.append(v)
                if dfs(v):
                    return res
                res.pop()
                adj[cur].append(v)
            return False

        dfs("JFK")
        return res

```

Alright, this problem involves finding an itinerary from a list of tickets, such that the itinerary visits all given tickets and uses all of them exactly once. The tickets represent directed edges in a graph, and the challenge is to find a path that traverses each edge exactly once, a.k.a., an Eulerian path.

### First Principles of Problem-Solving:

**Core Principle**: The solution is to be found in the Eulerian path, i.e., visiting every edge exactly once. When multiple paths exist, choose the smallest one in lexicographical order.

### Solving Intuition:

1. **Graph Modeling**: Every ticket represents a directed edge from a departure airport to an arrival airport. Thus, the first step is to model the tickets as a graph.
   
2. **Depth First Search (DFS)**: Use DFS to explore possible paths in the graph and backtrack when we hit a dead-end.
   
3. **Ordering of Paths**: When multiple edges exist from a node, we should choose them in lexicographical order, hence sorting the tickets first.

### Raw Algorithm:

1. Convert the list of tickets into an adjacency list representation of a graph.
   
2. Sort the tickets lexicographically so that while exploring, we take the smallest edge first.
   
3. Start DFS traversal from the "JFK" airport, explore the paths, and backtrack when needed.
   
4. Continue the traversal until we use all tickets.

### Why the Code is Written This Way:

1. **Adjacency List (`adj`)**: 
   - The dictionary with deque data structure captures the graph's representation.
   - A deque is used because it provides O(1) time complexity for append and popleft operations, which are frequently used in this approach.
   
2. **DFS Implementation (`dfs` function)**: 
   - The recursive function is used to traverse the graph.
   - It explores possible paths and backtracks when a path is found or when it hits a dead-end.
   
3. **Temporary Storage (`temp`)**: 
   - It's a safeguard for backtracking. When backtracking, we can restore the state from `temp`.
   
4. **Decision Making in DFS**: 
   - The check `len(res) == len(tickets) + 1` ensures we have used all tickets.
   - The check `cur not in adj` ensures that we don't run into a KeyError.

### Time Complexity:
- In the worst case, where we might need to backtrack frequently, the time complexity is exponential, **O(2^N)**, where N is the number of tickets. However, in practice, due to pruning and the nature of the problem, it will usually be much faster.

### Space Complexity:
- The space is dominated by the DFS call stack and the adjacency list. The space complexity is **O(N)**, where N is the number of tickets.

### Base Cases and Edge Cases:

**Base Cases**:
- The starting airport is always "JFK".

**Edge Cases**:
- There can be multiple tickets between the same pair of airports.
- The itinerary needs to be lexicographically smallest when multiple valid itineraries exist.

### Optimizing Clues:

- An optimization can be using a priority queue (heap) instead of sorting the tickets. This ensures that the smallest airport code is always chosen first without the need to sort the entire list of tickets.

### Mnemonics to Remember:

- **"Tickets to Paths"**: To recall that the problem is about converting a list of tickets into paths in a graph.
- **"JFK Start"**: A reminder that the starting point is always "JFK".
- **"DFS for Depth"**: Use Depth First Search to explore the depth of possible itineraries.

In essence, this solution transforms a list of flight tickets into a graph problem where the challenge is to find an Eulerian path. Using DFS for traversal, backtracking when required, and ensuring the lexicographical order gives the desired itinerary.