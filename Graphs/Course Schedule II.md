---

---
---
[LC Link](https://leetcode.com/problems/course-schedule-ii/)

---
---

```python
class Solution:
    def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
        prereq = {c: [] for c in range(numCourses)}
        for crs, pre in prerequisites:
            prereq[crs].append(pre)

        output = []
        visit, cycle = set(), set()

        def dfs(crs):
            if crs in cycle:
                return False
            if crs in visit:
                return True

            cycle.add(crs)
            for pre in prereq[crs]:
                if dfs(pre) == False:
                    return False
            cycle.remove(crs)
            visit.add(crs)
            output.append(crs)
            return True

        for c in range(numCourses):
            if dfs(c) == False:
                return []
        return output

```

### Problem Understanding:

Given a number of courses to take, labeled from `0` to `numCourses-1`, and a list of prerequisites where prerequisites[i] = [course_i, prerequisite_i], determine the order in which the courses should be taken to complete all courses. If there are multiple valid orders, return any of them. If it's impossible to finish all courses, return an empty array.

### 1st Principles of Problem-Solving:

**Core Principle**: This problem can be viewed as a topological sort on a directed graph where each course is a node and the prerequisites establish directed edges. A valid sequence of courses is a topological ordering.

### Solving Intuition:

Topological Sort is a linear ordering of vertices in a directed graph such that for every directed edge (U, V), vertex U comes before V in the ordering. An understanding of topological sorting and its application to course scheduling is crucial.

### Raw Algorithm:

1. Create an adjacency list (`prereq`) to represent our graph where the courses are keys and their prerequisites are values.
2. Utilize DFS to visit each course. As we visit a course's prerequisites, add them to the `output` list.
3. If a cycle is detected during DFS, it's impossible to take all courses, so return an empty list.

### Why The Code Is Written The Way It Is:

**1. `prereq` dictionary:**
- Represents the graph. Each course maps to its list of prerequisites.

**2. `output`, `visit`, and `cycle` lists/sets:**
- `output`: Stores the valid order of courses.
- `visit`: Tracks courses that have been processed (courses we've finished DFS on and confirmed there's no cycle from them).
- `cycle`: Tracks courses currently being visited during DFS. Useful for cycle detection.

**3. `dfs` function:**
- Does a depth-first search on the course.
- If the course is in the `cycle` set, a cycle exists.
- If it's already in `visit`, it's already processed so return `True`.
- Otherwise, add it to `cycle` and then traverse its prerequisites.
- After processing all prerequisites, remove the course from `cycle`, add to `visit`, and append to `output`.

**4. Main Loop:**
- For each course, apply DFS. If DFS returns `False`, return an empty list because it's impossible to take all courses.

### Time Complexity:

**O(N + E)** where `N` is the number of courses and `E` is the number of prerequisites. This is due to visiting every course and its prerequisites at most once.

### Space Complexity:

**O(N + E)** mainly for storing the graph (`prereq`), the `visit` set, the `cycle` set, and the `output` list.

### Base Cases and Edge Cases:

**Base Cases**:
- If a course doesn't have prerequisites, it can be directly added to the order.

**Edge Cases**:
- If no courses or prerequisites are provided, return an empty list or a list with courses in any order (since there are no constraints).

### Optimizing Clues:

- Once a course is determined to be without cycles (i.e., it's in the `visit` set), there's no need to process it again.
- Topological sorting can also be achieved using in-degrees and a queue to iteratively remove nodes with an in-degree of 0.

### Mnemonics to Remember:

- **"DFS for Degree-Free Sort"**: For this problem, DFS is an excellent approach for topological sorting.
- **"Course Order = No Cycles"**: Remember, the valid order of courses implies no cyclic dependencies.

In a nutshell, the problem is a classic case for topological sorting on a directed graph. Understanding the DFS traversal with a means to detect cycles helps in effectively determining the order of courses. If a cycle is detected, it's indicative of an impossible course order.