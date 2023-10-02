---

---
---
[LC Link](https://leetcode.com/problems/walls-and-gates/)

---
---

```python
class Solution:
    """
    @param rooms: m x n 2D grid
    @return: nothing
    """

    def walls_and_gates(self, rooms: List[List[int]]):
        ROWS, COLS = len(rooms), len(rooms[0])
        visit = set()
        q = deque()

        def addRooms(r, c):
            if (
                min(r, c) < 0
                or r == ROWS
                or c == COLS
                or (r, c) in visit
                or rooms[r][c] == -1
            ):
                return
            visit.add((r, c))
            q.append([r, c])

        for r in range(ROWS):
            for c in range(COLS):
                if rooms[r][c] == 0:
                    q.append([r, c])
                    visit.add((r, c))

        dist = 0
        while q:
            for i in range(len(q)):
                r, c = q.popleft()
                rooms[r][c] = dist
                addRooms(r + 1, c)
                addRooms(r - 1, c)
                addRooms(r, c + 1)
                addRooms(r, c - 1)
            dist += 1

```

### Problem Understanding:

Given a 2D grid (`rooms`) where:
- A room is represented by an `INF` (large value, e.g., 2**31-1).
- A gate is represented by a `0`.
- A wall is represented by a `-1`.

The task is to update each room's value with its distance to its nearest gate. If a room cannot reach a gate, then it remains at its `INF` value.

### 1st Principles of Problem-Solving:

**Core Principle**: Spread the "distance influence" of the gates throughout the rooms until every room that can be reached by a gate has been updated with its distance to its nearest gate.

### Solving Intuition:

The problem can be seen as a BFS traversal. Gates are the starting points. From each gate, we traverse outwards to update rooms with their distances.

### Raw Algorithm:

1. Initialize an empty queue and a visited set.
2. Add all gates to the queue (as these are starting points).
3. BFS traversal:
   a. For each gate/room, check its neighbors.
   b. If a neighbor is a room (not a wall, and not visited before), update its distance, and enqueue it.
   c. Each level of BFS represents an increase in distance from the gates.
4. Repeat the process until the queue is empty.

### Why The Code Is Written The Way It Is:

**1. Initializing `visit` and `q`:**
- `visit` (set) helps in ensuring a room/gate is not processed multiple times.
- `q` (queue) is used for BFS traversal.

**2. `addRooms` function:**
- Encapsulates the logic of adding valid neighboring rooms to the queue.
- Checks boundaries, if a cell is a wall, or if it has been visited already.

**3. Double Loop Initialization:**
- Iterates through the `rooms` to find all gates (value `0`) and adds them to the queue to begin BFS traversal.

**4. BFS with `while q:` loop:**
- A typical BFS traversal pattern. Continues until all gates and rooms reachable from gates have been processed.

**5. Nested `for` loop within the BFS loop:**
- Each iteration of this loop represents a "distance level". All rooms processed in the same iteration are at the same distance from a gate.

**6. `rooms[r][c] = dist`:**
- Updates the room's value with its distance from its nearest gate.

### Time Complexity:

**O(n*m)** where `n` is the number of rows and `m` is the number of columns in `rooms`. This is because each room/gate can be visited at most once.

### Space Complexity:

**O(n*m)** as in the worst-case scenario, all cells in `rooms` could be added to the queue and the visited set.

### Base Cases and Edge Cases:

**Base Cases**:
- Ensure the room is within the grid's boundaries.
- Only non-wall rooms can be visited or updated.

**Edge Cases**:
- If all rooms are walls, nothing is updated.
- If there are no gates, all rooms remain at their initial values (`INF`).

### Optimizing Clues:

- Instead of having an external `dist` variable, you can include distance in the queue itself. This way, each cell in the queue is a tuple of its coordinates and its distance from a gate.

### Mnemonics to Remember:

- **"Gates Dictate"**: Remember that gates influence the rooms around them, updating their values.
- **"Walls Halt, Rooms Exalt"**: Walls are boundaries that cannot be passed, while rooms can be updated with distances.

In essence, the solution revolves around BFS where the gates act as the seed points, and the influence (or distance) spreads outwards until all reachable rooms are updated. The use of BFS ensures minimal distances are recorded for each room.