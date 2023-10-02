---

---
---
[LC Link](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

### First Principles Thinking

In the "Best Time to Buy and Sell Stock" problem, the primary objective is to find the maximum profit that can be made by buying and selling stock. The fundamental principles include:

1. The buying day must precede the selling day.
2. Maximum profit is calculated as the difference between the selling price and the buying price.
3. To maximize profit, we should aim to buy at the lowest possible price and sell at the highest possible price afterward.

### Brute Force Approach

#### Explanation of the Naive Solution
The brute-force approach involves iterating through each pair of days to find the maximum possible profit.

#### Programming/Coding Principles
- **Raw Algorithm**: Nested for-loop to check all pairs of days.
- **Data Structure**: None.
- **For Loop**: Two nested for-loops to check all pairs.
- **Update Criteria**: Update max_profit whenever we find a higher profit.
- **Base Case & Edge Case**: Handle cases when the array has fewer than 2 elements.

#### Python Code for the Brute Force Approach
```python
def maxProfit(prices):
    if len(prices) < 2:
        return 0
    
    max_profit = 0
    for i in range(len(prices)):
        for j in range(i+1, len(prices)):
            max_profit = max(max_profit, prices[j] - prices[i])
    
    return max_profit
```

#### Analysis of Time and Space Complexity
- **Time Complexity**: \(O(n^2)\)
- **Space Complexity**: \(O(1)\)

### Optimized Approaches

#### Intuition and Train of Thought
To optimize, we can keep track of the minimum price encountered so far and the maximum profit that can be made by selling at each day's price.

#### Step-by-step Transition
1. Initialize variables to keep track of minimum price and maximum profit.
2. Iterate through the array, updating the minimum price and maximum profit.

#### Programming/Coding Principles
- **Raw Algorithm**: Single pass using for-loop.
- **Data Structure**: None.
- **For Loop**: Single loop to iterate through the array.
- **Update Criteria**: Update min_price and max_profit when a better option is found.
- **Base Case & Edge Case**: Handle arrays with fewer than 2 elements.

#### Python Code for the Optimized Solution
```python
def maxProfit(prices):
    if len(prices) < 2:
        return 0
    
    min_price = prices[0]
    max_profit = 0
    
    for price in prices:
        min_price = min(min_price, price)
        max_profit = max(max_profit, price - min_price)
    
    return max_profit
```

#### Analysis of Time and Space Complexity
- **Time Complexity**: \(O(n)\)
- **Space Complexity**: \(O(1)\)

### Strategies and Techniques
The single-pass optimization using the concept of maintaining minimum and maximum variables is a common technique for such problems. It's helpful to note that sometimes the maximum profit can be found in a single pass, eliminating the need for a nested loop and thus reducing the time complexity.

As you aspire to master LeetCode problems, remember that often a single pass through the data can yield the solution, provided you maintain the right state variables. Keep practicing, and these patterns will become second nature.

---
---

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        res = 0
        
        lowest = prices[0]
        for price in prices:
            if price < lowest:
                lowest = price
            res = max(res, price - lowest)
        return res

```


Alright, this problem deals with computing the maximum profit one could achieve by purchasing and selling a single share of stock on different days. The solution given uses a simple one-pass algorithm to find the maximum difference between the highest peak and lowest valley.

## First Principles of Problem-solving:

The basic principle here is **Iterative Comparison**. This means as we iterate through the list, we continuously compare and update our result based on the current and past elements.

## Intuition:

The problem can be thought of as finding the maximum peak after the minimum valley. So, as we iterate, we remember the lowest price we've seen and then calculate the difference between the current price and this lowest price. By doing this iteratively, we can find the maximum possible profit.

## Time and Space Complexity:

**Time Complexity:** O(n) - We go through the list once.
**Space Complexity:** O(1) - Constant extra space is used regardless of the input size.

## Raw Algorithm:

1. Initialize `res` to 0, which will store the maximum profit.
2. Start with the first price as the `lowest`.
3. For each `price` in `prices`:
    - Check if the `price` is lower than the `lowest` we've seen. If it is, update `lowest`.
    - Calculate potential profit, which is the difference between the current `price` and `lowest`. Compare it to `res` and update `res` if this potential profit is higher.
4. Return `res`.

## Programming Approach:

**Why the for loop?**

The for loop is used to iterate over each price, enabling us to compare the current price with the lowest we've seen and update our maximum profit accordingly.

**Why the variables `lowest` and `res`?**

- `lowest` is used to keep track of the lowest price we've encountered as we iterate. This is essential because buying at the lowest price maximizes potential profit.
- `res` is used to store the maximum profit we can achieve. We update this whenever we find a higher potential profit.

**Why the conditional checks inside the loop?**

The condition `if price < lowest` helps us ensure that we are always working with the lowest price encountered so far, allowing us to maximize profit. The following line, `res = max(res, price - lowest)`, updates our profit if the current difference (or potential profit) between the current price and the lowest price is greater than the stored profit.

## Further Optimizing Clues:

The given algorithm is quite optimized for this particular problem. However, when solving such problems:
1. Consider if there's any repeated computation that can be avoided.
2. Always keep in mind if there are simpler or more efficient data structures to represent the data.

## Mnemonics to remember:

**L.P.M.**:
- **L**owest tracking.
- **P**rofit maximization.
- **M**ovement through prices.

## Base and Edge Cases:

- If the list of prices is empty or has a single element, the profit is 0, as there's no transaction possible.
- If the prices are in decreasing order, the maximum profit is 0 as there's no suitable day to buy where selling later would result in a profit.
- If the prices are in increasing order, the profit would be the difference between the last and the first element.

In essence, understanding that you want to find the maximum peak after the minimum valley is the key to cracking this problem. As you move through the list, keep track of the minimum and calculate potential profits, updating the result when necessary.