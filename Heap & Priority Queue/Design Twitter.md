---

---
---
[LC Link](https://leetcode.com/problems/design-twitter/)

---
---

```python
class Twitter:
    def __init__(self):
        self.count = 0
        self.tweetMap = defaultdict(list)  # userId -> list of [count, tweetIds]
        self.followMap = defaultdict(set)  # userId -> set of followeeId

    def postTweet(self, userId: int, tweetId: int) -> None:
        self.tweetMap[userId].append([self.count, tweetId])
        self.count -= 1

    def getNewsFeed(self, userId: int) -> List[int]:
        res = []
        minHeap = []

        self.followMap[userId].add(userId)
        for followeeId in self.followMap[userId]:
            if followeeId in self.tweetMap:
                index = len(self.tweetMap[followeeId]) - 1
                count, tweetId = self.tweetMap[followeeId][index]
                heapq.heappush(minHeap, [count, tweetId, followeeId, index - 1])

        while minHeap and len(res) < 10:
            count, tweetId, followeeId, index = heapq.heappop(minHeap)
            res.append(tweetId)
            if index >= 0:
                count, tweetId = self.tweetMap[followeeId][index]
                heapq.heappush(minHeap, [count, tweetId, followeeId, index - 1])
        return res

    def follow(self, followerId: int, followeeId: int) -> None:
        self.followMap[followerId].add(followeeId)

    def unfollow(self, followerId: int, followeeId: int) -> None:
        if followeeId in self.followMap[followerId]:
            self.followMap[followerId].remove(followeeId)

```

The problem at hand involves designing a simplified version of Twitter. The tasks include posting a tweet, getting the 10 most recent tweets in a user's news feed, and allowing users to follow/unfollow other users. Here's an in-depth breakdown:

### Problem-Solving Approach:

1. **Data Tracking**: To replicate the functionality of Twitter, we need a way to track tweets and followers. Thus, the use of dictionaries (`defaultdict`).

2. **Time-Ordering**: Tweets must be displayed in reverse chronological order. As such, we need a mechanism that prioritizes newer tweets over older ones. 

### Solving Intuition:

- Use **two dictionaries**: One (`tweetMap`) to track the tweets of each user and the other (`followMap`) to track who each user is following.
  
- Tweets should be retrievable in **reverse chronological order**. This can be achieved using a **min-heap** to retrieve the 10 most recent tweets. 

### Key Components:

#### 1. `__init__`:

- `count`: An inverse time counter. A decreasing count means newer tweets will have a lower count, which in the context of a min-heap, ensures that newer tweets have higher priority.
- `tweetMap`: This maps a userId to a list of their tweets, where each tweet is represented by a `[count, tweetId]` pair.
- `followMap`: This maps a userId to a set of userIds they are following.

#### 2. `postTweet`:

- A user posts a tweet, which is added to their list in `tweetMap`. The current `count` (which is always decreasing) is used to record when the tweet was made.

#### 3. `getNewsFeed`:

- Gather the 10 most recent tweets from the user and all the people they follow. This is achieved using a min-heap.
  
#### 4. `follow` and `unfollow`:

- These modify the `followMap` to add or remove relationships between users.

### Time Complexity:

1. `postTweet`: O(1) - Constant time to append a tweet.
2. `getNewsFeed`: O(NlogK) where N is the number of users the person is following and K is the number of tweets fetched (in this case 10).
3. `follow` and `unfollow`: O(1) due to the use of sets in `followMap`.

### Space Complexity:

O(U + T) where U is the number of users and T is the total number of tweets across all users.

### Why The Code Is Written The Way It Is:

1. **`self.count -= 1`**: A decreasing count ensures that newer tweets, having lower values, will come to the top of the min-heap faster.
  
2. **minHeap**: It helps efficiently get the top 10 tweets. It's not max-heap because by decreasing the count for newer tweets, we effectively turn a min-heap into a max-heap in terms of chronological order.
  
3. **`self.followMap[userId].add(userId)`**: This ensures that the user can also see their own tweets in the feed.

### Base Cases and Edge Cases:

1. **User not following anyone**: They should only see their own tweets.
2. **Unfollowing a user you aren't following**: The code should handle it gracefully without errors.
3. **Following oneself**: This should be allowed as seen in the `getNewsFeed` method.

### Optimizing Clues:

1. **Limit Tweet History**: Instead of storing all tweets for every user, store only the last 10 to reduce space usage. This requires maintaining a queue-like data structure for each user's tweets.
  
2. **Cache News Feed**: If no changes in the relationships or tweets, we can cache the result of the news feed to avoid recomputation.

### Mnemonics to Remember:

1. **"Count Down Tweets"**: Remember that tweets are counted in reverse to ensure that newer tweets are prioritized in a min-heap.
  
2. **"Heap for Top News"**: The min-heap is crucial for fetching the top tweets efficiently.

3. **"Two Maps to Tweet-Track"**: `tweetMap` for tweets and `followMap` for followers. 

In essence, this problem is a blend of data structure manipulation (using dictionaries and sets) and algorithm optimization (using heaps). The design effectively simulates the core functionality of Twitter.