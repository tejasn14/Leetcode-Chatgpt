---

---
---
[LC Link](https://leetcode.com/problems/evaluate-reverse-polish-notation/)
### First Principles Thinking

The fundamental truth about Reverse Polish Notation (RPN) is that it eliminates the need for parentheses to indicate orders of operation. The notation relies on the position of the operators and operands to dictate what operations occur and in what sequence.

1. **Operands**: Numbers to be operated upon.
2. **Operators**: '+', '-', '*', and '/'.

To evaluate an RPN expression, you read the elements from left to right. When you find an operator, you apply it to the most recently encountered operands and replace them with the result. This result then becomes an operand for any further operations.

### Brute Force Approach

#### Explanation
In a brute-force way, you can simply follow the procedure laid down by RPN:

1. Initialize an empty stack.
2. Traverse each token in the list.
  - If it's an operand, push it onto the stack.
  - If it's an operator, pop the top two elements from the stack, perform the operation, and push the result back onto the stack.
3. The stack will finally contain a single integer, which is the result of the expression.

#### Programming/Coding Principles
- **Data Structure**: Use a stack to hold operands.
- **Looping**: A single 'for' loop to traverse through the array of tokens.
- **Base Case**: Empty stack or a stack with a single element.
- **Update Criteria**: Pop two elements, perform operation, push result back.
- **Edge Cases**: Since the input is guaranteed to be a valid RPN, additional checks for invalid expressions are not required.

#### Python Code for Brute Force
```python
def evalRPN(tokens):
    stack = []
    for token in tokens:
        if token in ['+', '-', '*', '/']:
            b = stack.pop()
            a = stack.pop()
            if token == '+':
                stack.append(a + b)
            elif token == '-':
                stack.append(a - b)
            elif token == '*':
                stack.append(a * b)
            else:
                stack.append(int(a / b))
        else:
            stack.append(int(token))
    return stack[0]
```

#### Time and Space Complexity
- **Time Complexity**: \(O(n)\), where \(n\) is the number of tokens.
- **Space Complexity**: \(O(n)\), for the stack.

### Optimized Approach

#### Intuition and Train of Thought
In this case, the brute-force approach is already fairly optimized. However, you can make slight modifications to make the code more concise.

#### Step-by-step Transition
We'll keep the stack-based approach because RPN inherently works well with stacks. We can, however, condense the code using Python's `eval` function, though that might be considered cheating in some contexts.

#### Python Code for Optimized Solution
```python
def evalRPN(tokens):
    stack = []
    for token in tokens:
        if token in ['+', '-', '*', '/']:
            b, a = stack.pop(), stack.pop()
            stack.append(int(eval(f"{a}{token}{b}")))
        else:
            stack.append(int(token))
    return stack[0]
```

#### Time and Space Complexity
Same as the brute-force approach: \(O(n)\) time complexity and \(O(n)\) space complexity.

### Strategies and Techniques
- Understanding how to use stacks effectively is key to solving this problem.
- The pattern of pushing and popping from a stack is a common technique for problems that deal with ordered sequences and nested structures.

Remembering this problem can be easy if you recall the fundamentals of RPN and its close relationship with stack data structures. Think "RPN = Stack" as a mnemonic.


---
---

```python
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        stack = []
        for c in tokens:
            if c == "+":
                stack.append(stack.pop() + stack.pop())
            elif c == "-":
                a, b = stack.pop(), stack.pop()
                stack.append(b - a)
            elif c == "*":
                stack.append(stack.pop() * stack.pop())
            elif c == "/":
                a, b = stack.pop(), stack.pop()
                stack.append(int(float(b) / a))
            else:
                stack.append(int(c))
        return stack[0]

```

The given problem involves evaluating Reverse Polish Notation (RPN) expressions. The RPN notation is a postfix mathematical notation in which every operator follows all of its operands. The key insight to solve problems like this is to use a stack.

Let's break down the given solution using the mentioned approach:

### 1. First Principles of Problem-solving Approach:

The core principle of evaluating RPN expressions is to keep track of numbers until an operator is encountered. When an operator is encountered, the appropriate number of values are popped from the stack, the operation is performed, and the result is pushed back onto the stack.

### 2. Solving Intuition:

Using a stack is the most intuitive approach for RPN evaluation. As you process each token from left to right, if it's a number, you push it to the stack. If it's an operator, you pop the necessary number of operands from the stack, perform the operation, and push the result back to the stack.

### 3. Time/Space Complexity:

**Time Complexity**: O(n) - Where 'n' is the number of tokens. Each token is processed once.

**Space Complexity**: O(n) - In the worst case, the stack will hold all the numbers.

### 4. Raw Algorithm:

1. Initialize an empty stack.
2. For each token in tokens:
   - If the token is a number, push it onto the stack.
   - If the token is an operator, pop the required number of operands from the stack, perform the operation, and push the result back onto the stack.
3. The final result will be the only value left on the stack.

### 5. Programming Approach:

- **Why the stack**: The stack is used to keep track of numbers in the RPN expression until they are needed for evaluation.
- **Why the conditional checks for operators**: To determine when to perform mathematical operations.
- **Special consideration for division**: In Python 2, the division of two integers with `/` will truncate the result to an integer. Using `float()` ensures we get a floating-point result, and then we truncate to integer using `int()`.
- **The final return**: After processing all the tokens, the stack will contain only one value: the result of the RPN expression.

### 6. Base cases and Edge Cases:

- Base Case: A simple RPN like `["2", "1", "+"]` should return 3.
- Edge Cases: Need to handle operations carefully. Division by zero isn't addressed in this code, but in real-world situations, you'd want to add a check for that.

### 7. Further Optimizing Clues:

The solution is quite optimal for the given problem. One possible micro-optimization is to replace the sequence of `elif` statements with a dictionary mapping operators to lambda functions. However, this might make the code less readable for some.

### 8. Mnemonics to Remember the Solution:

**"Push, Pop, Perform!"**:
- **Push** numbers onto the stack.
- **Pop** them off when you encounter operators.
- **Perform** the operation and push the result back.

By following this simple three-step mnemonic, you can easily recall the core logic behind evaluating RPN expressions using a stack.
