---

---
---
[LC Link](https://leetcode.com/problems/course-schedule/)

---
---

```python
class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        # dfs
        preMap = {i: [] for i in range(numCourses)}

        # map each course to : prereq list
        for crs, pre in prerequisites:
            preMap[crs].append(pre)

        visiting = set()

        def dfs(crs):
            if crs in visiting:
                return False
            if preMap[crs] == []:
                return True

            visiting.add(crs)
            for pre in preMap[crs]:
                if not dfs(pre):
                    return False
            visiting.remove(crs)
            preMap[crs] = []
            return True

        for c in range(numCourses):
            if not dfs(c):
                return False
        return True

```

### Problem Understanding:

Given a list of prerequisites for courses, determine if it's possible for someone to finish all courses. The courses are represented by numbers `0` through `numCourses-1`. A prerequisite is represented as a pair of numbers, where the first number is a course that has a prerequisite of the second number.

The problem can be mapped as detecting a cycle in a directed graph. If there's a cycle, it's impossible to finish all courses. Otherwise, it's feasible.

### 1st Principles of Problem-Solving:

**Core Principle**: In the graph of courses (represented as nodes) and prerequisites (represented as directed edges), if there is a cycle, then it's impossible to complete all courses because one would be stuck in a loop of prerequisites.

### Solving Intuition:

Understanding that the problem is equivalent to detecting cycles in a directed graph, we can utilize a common algorithm for cycle detection, called Depth First Search (DFS). 

### Raw Algorithm:

1. Create a prerequisite map (a.k.a adjacency list) where each course maps to a list of its prerequisites.
2. Iterate through each course, and for each course, do a DFS to see if you can reach a course that is currently being visited. If so, then there's a cycle.
3. If you manage to finish DFS for all courses without detecting a cycle, return True, otherwise False.

### Why The Code Is Written The Way It Is:

**1. Creating `preMap` (adjacency list):**
- This represents our graph. Each course is a key, and the values are lists of courses that are prerequisites for the key.

**2. `visiting` set:**
- It tracks courses currently being visited during a DFS. It's crucial for cycle detection. If during DFS, we reach a course that's currently being visited, then a cycle exists.

**3. `dfs` function:**
- Represents our Depth First Search. 
- If `crs` is in the visiting set, there's a cycle, hence, return False.
- If there are no prerequisites for a course, it means we've reached the "base" course (with no further prerequisites), return True.
- Recursively traverse the prerequisites of the course. If any of those returns False (i.e., a cycle was detected deeper in the DFS), then return False for the current course too.
- After visiting all prerequisites for a course, remove the course from the visiting set and empty its prerequisites in the `preMap` (this is a form of memoization to speed up further DFS checks).
  
**4. Main loop:**
- Iterates through all courses. For each course, perform DFS to check for cycles. If any course's DFS returns False, then return False for the entire function.

### Time Complexity:

**O(N + E)** where `N` is the number of courses and `E` is the number of prerequisites. This is because in the worst case, we will visit every course and every prerequisite.

### Space Complexity:

**O(N + E)** for storing the graph (`preMap`) and the visiting set.

### Base Cases and Edge Cases:

**Base Cases**:
- If a course has no prerequisites, it can be immediately completed.
- If during DFS, a course is already being visited, then there's a cycle.

**Edge Cases**:
- If no courses or prerequisites are provided, then all courses can be completed.

### Optimizing Clues:

- One can also use a more sophisticated cycle detection technique like "Union-Find" or "Topological Sort" to solve this problem.
- Using a "visited" set in addition to the "visiting" set can help optimize further. If a course is already visited and doesn't have a cycle, then we don't need to DFS from that course again.

### Mnemonics to Remember:

- **"Course before Horse"**: Always ensure you finish the prerequisite courses before moving on to the dependent ones.
- **"DFS Detects Cycles Best"**: For this problem, DFS is a primary technique to detect cycles in the graph.

In essence, the solution revolves around the understanding that the problem is a cycle detection problem in a directed graph where nodes are courses and edges are prerequisites. The DFS traversal helps in identifying these cycles effectively.