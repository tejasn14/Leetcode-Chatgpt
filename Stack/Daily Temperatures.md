---

---
---
[LC Link](https://leetcode.com/problems/daily-temperatures/)
### First Principles Thinking

The fundamental nature of the problem involves finding the "next greater element" for each temperature in the list. Specifically, for each temperature, we want to find the next temperature in the list that is higher.

### Brute Force Approach

#### Explanation

The most straightforward way to find the next warmer temperature for each day is to iterate through each day and compare its temperature with all future days until a warmer day is found.

#### Programming/Coding Principles

- **Data Structure**: An array to hold the answer for each day.
- **Looping**: Nested for loops for each day and its subsequent days.
- **Base Case**: None. We are iterating through the entire array.
- **Update Criteria**: Whenever a warmer day is found, set answer[i] and break the inner loop.
- **Edge Cases**: If no warmer day is found, set answer[i] to 0.

#### Python Code for Brute Force

```python
def dailyTemperatures(temperatures):
    n = len(temperatures)
    ans = [0] * n
    
    for i in range(n):
        for j in range(i + 1, n):
            if temperatures[j] > temperatures[i]:
                ans[i] = j - i
                break
                
    return ans
```

#### Time and Space Complexity

- **Time Complexity**: \(O(n^2)\), as we are using nested loops.
- **Space Complexity**: \(O(n)\) for the answer array.

### Optimized Approach

#### Intuition and Train of Thought

We can optimize the problem by keeping track of temperatures that are waiting to find a warmer day using a stack. 

#### Step-by-step Transition

1. Use a stack to hold the temperatures (or rather their indices) for which we haven't found a warmer day yet.
2. Loop through the temperatures array once. For each temperature, pop all the colder temperatures from the stack and update the answer for those days.

#### Programming/Coding Principles

- **Data Structure**: A stack to keep track of indices.
- **Looping**: A single for loop for iteration.
- **Base Case**: The stack is empty.
- **Update Criteria**: Pop from the stack if a warmer day is found.
- **Edge Cases**: None, as the stack will automatically handle any remaining days.

#### Python Code for Optimized Solution

```python
def dailyTemperatures(temperatures):
    n = len(temperatures)
    ans = [0] * n
    stack = []
    
    for i in range(n):
        while stack and temperatures[i] > temperatures[stack[-1]]:
            j = stack.pop()
            ans[j] = i - j
        stack.append(i)
    
    return ans
```

#### Time and Space Complexity

- **Time Complexity**: \(O(n)\) since each temperature is pushed and popped from the stack at most once.
- **Space Complexity**: \(O(n)\) for the stack and answer array.

### Strategies and Techniques

- The use of a stack is crucial here, as it enables efficient tracking of "next greater element" problems.
  
#### Mnemonic

To remember this solution, you could use the acronym **WDS**: **W**armer **D**ays with **S**tack. This will help you recall that a stack is the primary data structure used to efficiently find warmer days.

---
---

```python
class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        res = [0] * len(temperatures)
        stack = []  # pair: [temp, index]

        for i, t in enumerate(temperatures):
            while stack and t > stack[-1][0]:
                stackT, stackInd = stack.pop()
                res[stackInd] = i - stackInd
            stack.append((t, i))
        return res

```

The `dailyTemperatures` problem is about finding out how many days you would have to wait until a warmer temperature for each day. If there's no future day for which this is possible, put `0` instead.

Here's a detailed breakdown of the solution:

### 1. First Principles of Problem-solving Approach:
The problem can be perceived as trying to find the next greater element for each element in the list. This can be solved using a stack-based approach.

### 2. Solving Intuition:
For every temperature, we check if it's higher than the last temperature we've seen. If it is, we know how many days (indices difference) we had to wait for a warmer day. We can efficiently do this by maintaining a stack.

### 3. Time/Space Complexity:
**Time Complexity**: O(n). Each temperature is pushed and popped from the stack exactly once.

**Space Complexity**: O(n). This is because we use a stack to store temperature-index pairs, and in the worst case, it can hold all the temperatures. Also, the `res` list that stores the output.

### 4. Raw Algorithm:
1. Iterate through the `temperatures` list.
2. For each temperature, check if the stack is not empty and if the current temperature is greater than the last temperature stored in the stack.
3. If yes, pop the stack and calculate the difference between the current day and the day from the stack. Update the result for that day.
4. Push the current temperature and its index onto the stack.
5. Return the result.

### 5. Programming Approach:

- **Why the `stack`**: The stack is used to remember the temperatures that we've seen so far and are waiting for a warmer day.
- **Pair `[temp, index]`**: We store both the temperature and its index in the stack so that we can find out how many days we had to wait.
- **Enumeration `for i, t in enumerate(temperatures)`**: Enumerate provides both the index `i` and the temperature `t` as we iterate. This is useful for our stack-based approach.
- **The while loop `while stack and t > stack[-1][0]`**: For each temperature, as long as the stack isn't empty and the current temperature is warmer than the last one in the stack, we keep updating our result.
- **The append `stack.append((t, i))`**: After processing the current temperature (and updating any results for previous temperatures), we push it onto the stack.

### 6. Base cases and Edge Cases:
- Base Case: For an empty list, return an empty list. For a list with one temperature, return `[0]`.
- Edge Cases: For descending temperatures like `[5,4,3,2,1]`, the result would be all zeros since no day has a warmer day after it.

### 7. Further Optimizing Clues:
The given solution is efficient. One could think about removing the tuple pair and using two stacks (one for temperatures and another for indices), but that wouldn't offer a significant improvement.

### 8. Mnemonics to Remember the Solution:
**"Wait, Check, Update!"**:
- **Wait**: Use a stack to "wait" for warmer days.
- **Check**: As you get a new temperature, "check" if it's warmer than the ones you're waiting for.
- **Update**: If you find a warmer day, "update" the result for the day you were waiting for.

By focusing on these three actions, you can recall the stack-based approach for this problem.
