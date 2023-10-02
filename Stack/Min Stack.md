---

---
---
[LC Link](https://leetcode.com/problems/min-stack/)
### First Principles Thinking

1. **Functionality Required**: The stack should support push, pop, top, and getMin operations.
2. **Time Complexity**: All operations should have a constant time complexity of \(O(1)\).

The fundamental constraint here is to perform each operation in constant time while keeping track of the minimum element. A typical stack would give \(O(1)\) for push, pop, and top, but \(O(n)\) for getMin as we'd have to traverse to find the minimum. The challenge is to get \(O(1)\) for getMin as well.

### Brute Force Approach

The naive approach to get the minimum element would be to traverse the stack and find it, but that would take \(O(n)\) time, which violates our constraints.

#### Programming/Coding Principles

1. **Raw Algorithm**: To brute force, you'd iterate through the stack every time you need to get the minimum.
2. **Best Data Structure**: A simple stack.
3. **Loop**: For-loop to find the minimum.
4. **Edge Cases**: Stack is empty.

However, the brute force approach is not applicable here due to the time complexity constraint.

### Optimized Approaches

#### Explanation

We can keep track of the minimum element for each state of the stack. This way, for any given state (after a push or a pop), we know the minimum element. We can use two stacks for this:

1. One for the elements themselves (`mainStack`).
2. Another to keep track of the minimum at each state (`minStack`).

#### Programming/Coding Principles

1. **Raw Algorithm**: Use two stacks, one for the elements and the other for the corresponding minimum elements.
2. **Best Data Structure**: Two stacks.
3. **Loop**: No loop needed as we're just pushing and popping from stacks.
4. **Base Cases**: When the stack is empty, return a specific value.
5. **Edge Cases**: For `getMin`, return some predefined constant if `minStack` is empty.

#### Python Code

```python
class MinStack:

    def __init__(self):
        self.mainStack = []
        self.minStack = []

    def push(self, val: int):
        self.mainStack.append(val)
        if not self.minStack or val <= self.minStack[-1]:
            self.minStack.append(val)

    def pop(self):
        if self.mainStack:
            if self.mainStack[-1] == self.minStack[-1]:
                self.minStack.pop()
            self.mainStack.pop()

    def top(self) -> int:
        return self.mainStack[-1] if self.mainStack else None

    def getMin(self) -> int:
        return self.minStack[-1] if self.minStack else None
```

#### Analysis

- **Time Complexity**: \(O(1)\) for all operations.
- **Space Complexity**: \(O(2n) = O(n)\) where \(n\) is the number of elements in the stack, as we are using two stacks.

### Strategies and Techniques

1. **Using Auxiliary Data Structure**: When the main data structure can't support the operation directly, think of an auxiliary data structure to fill the gap.
2. **State Keeping**: Keep track of the state of your data structure. Here, we kept track of the minimum element at each state of the stack.

This pattern can be generalized to similar problems where you need to keep track of the maximum/minimum or any other property of a data structure while performing operations.


---
---

```python
class MinStack:
    def __init__(self):
        self.stack = []
        self.minStack = []

    def push(self, val: int) -> None:
        self.stack.append(val)
        val = min(val, self.minStack[-1] if self.minStack else val)
        self.minStack.append(val)

    def pop(self) -> None:
        self.stack.pop()
        self.minStack.pop()

    def top(self) -> int:
        return self.stack[-1]

    def getMin(self) -> int:
        return self.minStack[-1]

```

This problem involves designing a stack that supports push, pop, top, and retrieving the minimum element in constant time. Let's break down the solution using the outlined approach.

### 1. First Principles of Problem-solving Approach:

Given the requirements, it's evident that we not only need to remember the elements we're inserting but also the minimum at each stage. A typical stack data structure provides a way to remember elements in LIFO order, but not to retrieve the minimum in O(1) time.

### 2. Solving Intuition:

To track the current minimum efficiently, we can use an auxiliary stack (`minStack`) that keeps track of the minimum value seen so far.

### 3. Time/Space Complexity:

**Time Complexity**:
- `push`: O(1) - Constant time to push a value onto both stacks.
- `pop`: O(1) - Constant time to pop a value from both stacks.
- `top`: O(1) - Constant time to retrieve the top element.
- `getMin`: O(1) - Constant time to get the current minimum.

**Space Complexity**: O(n) - In the worst case, all elements are stored in both the main stack and the auxiliary `minStack`.

### 4. Raw Algorithm:

1. **Initialization** (`__init__`): Create two empty stacks: one for the actual values (`stack`) and the other to track the minimums (`minStack`).
2. **Push**: When adding a value to the stack, push it onto the main stack. Compare this value to the current top of the `minStack`. Push the lesser of the two values onto the `minStack`.
3. **Pop**: Pop values from both the main stack and the `minStack`.
4. **Top**: Return the top value from the main stack.
5. **Get Minimum**: Return the top value from the `minStack`, which represents the current minimum.

### 5. Programming Approach:

- **Why two stacks**: One stack (`stack`) holds the actual values, while the other (`minStack`) holds the minimum values observed up to that point.
- **Why conditional in push**: To decide what value should be pushed onto `minStack`: the new value or the current minimum.
- **Why no conditions in pop**: Since both stacks are synchronized (pushed and popped together), there's no need for extra conditions.
- **Why the list data structure**: Lists in Python can be used efficiently as stacks by appending to the end and popping from the end.

### 6. Base cases and Edge Cases:

- Base Case: Pushing and popping one element should work seamlessly, and `getMin` should return the sole element after one push.
- Edge Cases: The solution takes care of multiple scenarios like multiple pushes, pushes and pops intermingled, and more.

### 7. Further Optimizing Clues:

The presented solution is quite optimal for the problem requirements, achieving O(1) time complexity for all operations. There's a space trade-off since two stacks are used, but it's a necessary compromise to achieve the time complexity goals.

### 8. Mnemonics to Remember the Solution:

**"Double Stack, Half the Hassle!"**:
- One stack (`stack`) for values, another (`minStack`) for the minimums.
- When pushing, decide based on the current minimum: either the new value or the last minimum.
- Pops are synchronized: both stacks release their top together.
- Getting the current minimum? Just peek at the top of `minStack`.

By maintaining two stacks in tandem, this solution ensures that the minimum element can always be retrieved in constant time, regardless of operations performed on the main stack.
