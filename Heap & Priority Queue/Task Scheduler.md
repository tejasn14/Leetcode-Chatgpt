---

---
---
[LC Link](https://leetcode.com/problems/task-scheduler/)

---
---

```python
class Solution:
    def leastInterval(self, tasks: List[str], n: int) -> int:
        count = Counter(tasks)
        maxHeap = [-cnt for cnt in count.values()]
        heapq.heapify(maxHeap)

        time = 0
        q = deque()  # pairs of [-cnt, idleTime]
        while maxHeap or q:
            time += 1

            if not maxHeap:
                time = q[0][1]
            else:
                cnt = 1 + heapq.heappop(maxHeap)
                if cnt:
                    q.append([cnt, time + n])
            if q and q[0][1] == time:
                heapq.heappush(maxHeap, q.popleft()[0])
        return time


# Greedy algorithm
class Solution(object):
    def leastInterval(self, tasks: List[str], n: int) -> int:
        counter = collections.Counter(tasks)
        max_count = max(counter.values())
        min_time = (max_count - 1) * (n + 1) + \
                    sum(map(lambda count: count == max_count, counter.values()))
    
        return max(min_time, len(tasks))
```

### Problem

Given a list of tasks, you need to execute them under the constraint that two identical tasks can't be executed consecutively within a time frame `n`. If it's not the case, you'd have to introduce idle slots. The goal is to find the least number of intervals (or time) to execute all tasks.

### Solution 1: Using Priority Queue (MaxHeap) and Queue

#### Problem-Solving Approach:

1. **Priority Queue (MaxHeap)**: A heap data structure will allow us to prioritize tasks that are more frequent. 

2. **Queue**: A queue will help us track when tasks that have been executed will become available again (based on the idle time `n`).

#### Solving Intuition:

- By using a max heap, you can always pick the most frequent task to execute. 
- Once a task is executed, it's not immediately available for execution until after `n` intervals.
- Use a queue to keep track of tasks that are cooling down. If a task's cooling down period is over, it can be reinserted into the max heap to be scheduled again.

#### Time Complexity:

- The main loop will iterate for each task; thus, the time complexity is approximately O(N log N), where N is the number of tasks.

#### Space Complexity:

- The heap and queue will store each unique task once, leading to a space complexity of O(N).

#### Why The Code Is Written The Way It Is:

1. **`Counter(tasks)`**: This is used to count the frequency of each task. 

2. **`maxHeap`**: We use a max heap, but Python only provides a min heap. By adding negative values, we simulate a max heap.

3. **`q = deque()`**: Represents the cool-down queue for tasks.

4. **`while maxHeap or q`**: This loop continues as long as there are tasks to be executed or tasks in the cool-down phase.

#### Base Cases and Edge Cases:

- An empty task list will return 0 time.
- If `n = 0`, tasks can be executed consecutively without any idle time.

### Solution 2: Greedy Algorithm

#### Problem-Solving Approach:

1. **Greedy**: The principle here is to determine the minimum time required based on the most frequent task. This is because the most frequent task will likely determine the number of idle slots.

#### Solving Intuition:

- If a task appears `max_count` times, and the idle time is `n`, then the minimum intervals would be `(max_count - 1) * (n + 1)`. 
- Add to this the number of tasks that have the maximum count.
- Compare the minimum time derived from this calculation with the total number of tasks, and take the maximum of the two.

#### Time Complexity:

- O(N), where N is the number of tasks. This is because we go through the tasks once to count them.

#### Space Complexity:

- O(1), because the space required for the counter is constant (26 letters of the alphabet, if tasks are represented by uppercase English letters).

#### Why The Code Is Written The Way It Is:

1. **`max(counter.values())`**: Determine the most frequent task's frequency.

2. **`sum(map(lambda count: count == max_count, counter.values()))`**: Count the number of tasks that have the maximum frequency.

3. **`max(min_time, len(tasks))`**: The total intervals required can't be less than the total number of tasks.

#### Base Cases and Edge Cases:

- An empty task list will return 0 time.
- If `n = 0`, tasks can be executed consecutively without any idle time.

#### Optimizing Clues:

For Solution 1:

1. **Batch Processing**: Instead of processing one task at a time, process tasks in batches where each batch is of size `n + 1`.

For Solution 2:

1. **Optimized Counting**: If tasks are represented by uppercase English letters, use an array of size 26 for counting.

#### Mnemonics:

- **"Heap for Priority, Queue for Cooldown"**: Remember that the first solution uses a heap for task prioritization and a queue for managing cooldowns.
- **"Most Frequent Dictates the Beat"**: For the greedy solution, the most frequent task largely determines the rhythm and the number of idle slots.

Both solutions have their merits. The heap-based approach is more intuitive and simulates the scheduling process, while the greedy approach is mathematically elegant and efficient.