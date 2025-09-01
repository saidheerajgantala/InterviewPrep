# 📝 Design Twitter

## **Problem Statement**

* Design a simplified version of Twitter where users can post tweets, follow/unfollow another user, and see the 10 most recent tweets in the user's news feed.
* Implement the `Twitter` class:
  * `Twitter()` Initializes your twitter object.
  * `void postTweet(int userId, int tweetId)` Composes a new tweet with ID `tweetId` by the user `userId`. Each call to this function will be made with a unique `tweetId`.
  * `List<Integer> getNewsFeed(int userId)` Retrieves the 10 most recent tweet IDs in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user themself. Tweets must be ordered from most recent to least recent.
  * `void follow(int followerId, int followeeId)` The user with ID `followerId` started following the user with ID `followeeId`.
  * `void unfollow(int followerId, int followeeId)` The user with ID `followerId` started unfollowing the user with ID `followeeId`.
* Example 1:
  * Input:
    ```
    ["Twitter", "postTweet", "getNewsFeed", "follow", "postTweet", "getNewsFeed", "unfollow", "getNewsFeed"]
    [[], [1, 5], [1], [1, 2], [2, 6], [1], [1, 2], [1]]
    ```
  * Output:
    ```
    [null, null, [5], null, null, [6, 5], null, [5]]
    ```
  * Explanation:
    ```
    Twitter twitter = new Twitter();
    twitter.postTweet(1, 5); // User 1 posts a new tweet (id = 5).
    twitter.getNewsFeed(1);  // User 1's news feed should return a list with 1 tweet id -> [5]. return [5]
    twitter.follow(1, 2);    // User 1 follows user 2.
    twitter.postTweet(2, 6); // User 2 posts a new tweet (id = 6).
    twitter.getNewsFeed(1);  // User 1's news feed should return a list with 2 tweet ids -> [6, 5]. Tweet id 6 should precede tweet id 5 because it is posted after tweet id 5.
    twitter.unfollow(1, 2);  // User 1 unfollows user 2.
    twitter.getNewsFeed(1);  // User 1's news feed should return a list with 1 tweet id -> [5], since user 1 is no longer following user 2.
    ```
* Constraints:
  * 1 <= userId, followerId, followeeId <= 500
  * 0 <= tweetId <= 10^4
  * All the tweets have unique IDs.
  * At most 3 * 10^4 calls will be made to postTweet, getNewsFeed, follow, and unfollow.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to design a simplified Twitter with the following features:
  * Post tweets
  * Follow/unfollow users
  * Get news feed (10 most recent tweets from followed users and self)
* One approach would be to use hash maps to store:
  * Users and their tweets
  * Users and their followers/followees
* For the news feed, we can gather all tweets from followed users and self, sort them by timestamp, and return the 10 most recent ones

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize data structures:
   1. A hash map to store users and their tweets
   2. A hash map to store users and their followees
   3. A counter to keep track of the timestamp for tweets
2. For `postTweet`:
   1. Create a tweet with the given tweetId and the current timestamp
   2. Add the tweet to the user's tweet list
   3. Increment the timestamp counter
3. For `getNewsFeed`:
   1. Gather all tweets from the user and their followees
   2. Sort the tweets by timestamp (most recent first)
   3. Return the IDs of the 10 most recent tweets
4. For `follow`:
   1. Add the followeeId to the followerId's followee set
5. For `unfollow`:
   1. Remove the followeeId from the followerId's followee set

---

## **Step 3: Brute Force Implementation (Code)**

```python
class Twitter:
    def __init__(self):
        self.tweets = {}  # userId -> list of (tweetId, timestamp)
        self.following = {}  # userId -> set of followeeIds
        self.timestamp = 0
    
    def postTweet(self, userId, tweetId):
        if userId not in self.tweets:
            self.tweets[userId] = []
        self.tweets[userId].append((tweetId, self.timestamp))
        self.timestamp += 1
    
    def getNewsFeed(self, userId):
        # Gather all tweets from user and followees
        all_tweets = []
        
        # Add user's own tweets
        if userId in self.tweets:
            all_tweets.extend(self.tweets[userId])
        
        # Add tweets from followees
        if userId in self.following:
            for followeeId in self.following[userId]:
                if followeeId in self.tweets:
                    all_tweets.extend(self.tweets[followeeId])
        
        # Sort tweets by timestamp (most recent first)
        all_tweets.sort(key=lambda x: x[1], reverse=True)
        
        # Return the 10 most recent tweet IDs
        return [tweetId for tweetId, _ in all_tweets[:10]]
    
    def follow(self, followerId, followeeId):
        if followerId not in self.following:
            self.following[followerId] = set()
        self.following[followerId].add(followeeId)
    
    def unfollow(self, followerId, followeeId):
        if followerId in self.following and followeeId in self.following[followerId]:
            self.following[followerId].remove(followeeId)
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity:
  * `postTweet`: O(1) - Adding a tweet to a user's tweet list is a constant time operation.
  * `getNewsFeed`: O(n log n) where n is the total number of tweets from the user and their followees. We need to gather all tweets and sort them.
  * `follow`: O(1) - Adding an element to a set is a constant time operation.
  * `unfollow`: O(1) - Removing an element from a set is a constant time operation.
* Space Complexity: O(n + m) where n is the total number of tweets and m is the total number of follow relationships.
* This approach is inefficient for the `getNewsFeed` operation, especially when users have many tweets or followees.

---

## **Step 5: Visualizing Redundant Computation**

* For the example:
  * `postTweet(1, 5)`: User 1 posts tweet 5 with timestamp 0
  * `getNewsFeed(1)`: Gather all tweets from user 1 (just tweet 5), sort them (only one tweet), return [5]
  * `follow(1, 2)`: User 1 follows user 2
  * `postTweet(2, 6)`: User 2 posts tweet 6 with timestamp 1
  * `getNewsFeed(1)`: Gather all tweets from user 1 (tweet 5) and user 2 (tweet 6), sort them by timestamp, return [6, 5]
  * `unfollow(1, 2)`: User 1 unfollows user 2
  * `getNewsFeed(1)`: Gather all tweets from user 1 (just tweet 5), sort them (only one tweet), return [5]
* The redundant computation is in the `getNewsFeed` operation, where we gather and sort all tweets even though we only need the 10 most recent ones

---

## **Why are we using this algorithm?**

* For optimization, we'll use a min-heap to efficiently find the 10 most recent tweets.
* The property that matches this problem is that we only need the k most recent tweets, not all tweets sorted.
* By using a min-heap of size 10, we can efficiently keep track of the 10 most recent tweets.
* Alternatives like the brute force approach would be less efficient when users have many tweets or followees.
* Precondition: We need to be able to compare tweets based on their timestamps.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Min-Heap)**

* Instead of gathering all tweets and sorting them, we can use a min-heap to efficiently find the 10 most recent tweets
* We'll use a max-heap (or a min-heap with negative timestamps) to keep the most recent tweets at the top
* We'll iterate through the tweets of the user and their followees, adding them to the heap
* If the heap size exceeds 10, we'll remove the oldest tweet
* This approach avoids sorting all tweets and is more efficient when users have many tweets or followees

### **Step 2: Flow Steps (Optimized - Min-Heap)**

1. Initialize data structures:
   1. A hash map to store users and their tweets
   2. A hash map to store users and their followees
   3. A counter to keep track of the timestamp for tweets
2. For `postTweet`:
   1. Create a tweet with the given tweetId and the current timestamp
   2. Add the tweet to the user's tweet list
   3. Increment the timestamp counter
3. For `getNewsFeed`:
   1. Initialize a max-heap
   2. Add the user's own tweets to the heap
   3. Add tweets from followees to the heap
   4. If the heap size exceeds 10, remove the oldest tweet
   5. Extract the 10 most recent tweet IDs from the heap
4. For `follow`:
   1. Add the followeeId to the followerId's followee set
5. For `unfollow`:
   1. Remove the followeeId from the followerId's followee set

### **Step 3: Implementation (Optimized - Min-Heap)**

```python
import heapq

class Twitter:
    def __init__(self):
        self.tweets = {}  # userId -> list of (tweetId, timestamp)
        self.following = {}  # userId -> set of followeeIds
        self.timestamp = 0
    
    def postTweet(self, userId, tweetId):
        if userId not in self.tweets:
            self.tweets[userId] = []
        self.tweets[userId].append((tweetId, self.timestamp))
        self.timestamp += 1
    
    def getNewsFeed(self, userId):
        # Initialize a max-heap (using negative timestamps for max-heap behavior)
        heap = []
        
        # Helper function to add tweets to the heap
        def add_tweets_to_heap(user_id):
            if user_id in self.tweets:
                for tweet_id, timestamp in self.tweets[user_id]:
                    # Use negative timestamp for max-heap behavior
                    heapq.heappush(heap, (-timestamp, tweet_id))
                    # If heap size exceeds 10, remove the oldest tweet
                    if len(heap) > 10:
                        heapq.heappop(heap)
        
        # Add user's own tweets
        add_tweets_to_heap(userId)
        
        # Add tweets from followees
        if userId in self.following:
            for followee_id in self.following[userId]:
                add_tweets_to_heap(followee_id)
        
        # Extract the 10 most recent tweet IDs (in reverse order to get most recent first)
        result = []
        while heap:
            result.append(heapq.heappop(heap)[1])
        
        # Reverse to get most recent first
        return result[::-1]
    
    def follow(self, followerId, followeeId):
        if followerId not in self.following:
            self.following[followerId] = set()
        self.following[followerId].add(followeeId)
    
    def unfollow(self, followerId, followeeId):
        if followerId in self.following and followeeId in self.following[followerId]:
            self.following[followerId].remove(followeeId)
```

### **Alternative Approach: Using a Priority Queue with Pointers**

Another efficient approach is to use a priority queue with pointers to the tweet lists:

```python
import heapq

class Twitter:
    def __init__(self):
        self.tweets = {}  # userId -> list of (tweetId, timestamp)
        self.following = {}  # userId -> set of followeeIds
        self.timestamp = 0
    
    def postTweet(self, userId, tweetId):
        if userId not in self.tweets:
            self.tweets[userId] = []
        self.tweets[userId].append((tweetId, self.timestamp))
        self.timestamp += 1
    
    def getNewsFeed(self, userId):
        # Get all relevant users (self and followees)
        users = {userId}
        if userId in self.following:
            users.update(self.following[userId])
        
        # Initialize a max-heap with the most recent tweet from each user
        heap = []
        for user in users:
            if user in self.tweets and self.tweets[user]:
                idx = len(self.tweets[user]) - 1
                tweet_id, timestamp = self.tweets[user][idx]
                heapq.heappush(heap, (-timestamp, tweet_id, user, idx - 1))
        
        # Extract the 10 most recent tweets
        result = []
        while heap and len(result) < 10:
            _, tweet_id, user, idx = heapq.heappop(heap)
            result.append(tweet_id)
            if idx >= 0:
                next_tweet_id, next_timestamp = self.tweets[user][idx]
                heapq.heappush(heap, (-next_timestamp, next_tweet_id, user, idx - 1))
        
        return result
    
    def follow(self, followerId, followeeId):
        if followerId not in self.following:
            self.following[followerId] = set()
        self.following[followerId].add(followeeId)
    
    def unfollow(self, followerId, followeeId):
        if followerId in self.following and followeeId in self.following[followerId]:
            self.following[followerId].remove(followeeId)
```

### **Step 4: Complexity Analysis (Optimized - Min-Heap)**

* Time Complexity:
  * `postTweet`: O(1) - Adding a tweet to a user's tweet list is a constant time operation.
  * `getNewsFeed`: O(n log 10) where n is the total number of tweets from the user and their followees. We add each tweet to the heap, which takes O(log 10) time, and we do this for at most n tweets.
  * `follow`: O(1) - Adding an element to a set is a constant time operation.
  * `unfollow`: O(1) - Removing an element from a set is a constant time operation.
* Space Complexity: O(n + m) where n is the total number of tweets and m is the total number of follow relationships.
* This approach is more efficient than the brute force approach for the `getNewsFeed` operation, especially when users have many tweets or followees.

### **Step 5: Visualizing Computation (Optimized - Min-Heap)**

* For the example:
  * `postTweet(1, 5)`: User 1 posts tweet 5 with timestamp 0
  * `getNewsFeed(1)`: Add tweet 5 to the heap, extract it, return [5]
  * `follow(1, 2)`: User 1 follows user 2
  * `postTweet(2, 6)`: User 2 posts tweet 6 with timestamp 1
  * `getNewsFeed(1)`: Add tweets 5 and 6 to the heap, extract them in order of timestamp, return [6, 5]
  * `unfollow(1, 2)`: User 1 unfollows user 2
  * `getNewsFeed(1)`: Add tweet 5 to the heap, extract it, return [5]
* The min-heap approach efficiently finds the 10 most recent tweets without sorting all tweets

### **Complexity Summary**

| Approach | Time (getNewsFeed) | Space | Notes |
|---|---|---|---|
| Brute Force (Sort) | O(n log n) | O(n + m) | Inefficient for many tweets |
| Optimized (Min-Heap) | O(n log 10) | O(n + m) | Efficient for finding top 10 |
| Priority Queue with Pointers | O(10 log u) | O(n + m) | u is the number of users |

---

## **Final Takeaways**

* The key to efficiently designing Twitter is to optimize the `getNewsFeed` operation, which is the most computationally intensive.
* By using a min-heap, we can efficiently find the 10 most recent tweets without sorting all tweets.
* The priority queue with pointers approach further optimizes the `getNewsFeed` operation by only considering the most recent tweets from each user.
* This problem demonstrates how choosing the right data structures can significantly improve the efficiency of an algorithm.

---

## **Real-life Analogy**

* Imagine you're a personal assistant who needs to compile a daily news digest for your boss. The brute force approach would be like gathering all news articles from all sources, sorting them by time, and then selecting the 10 most recent ones. The min-heap approach would be like keeping a running list of the 10 most recent articles, updating it as you encounter new articles. The priority queue with pointers approach would be like keeping track of the most recent article from each source and only checking for newer articles from those sources when needed.

---

## **Summary**

* We designed a simplified Twitter by first considering a brute force approach that gathers and sorts all tweets, resulting in O(n log n) time complexity for the `getNewsFeed` operation. We then optimized using a min-heap to efficiently find the 10 most recent tweets, reducing the time complexity to O(n log 10). We also explored a priority queue with pointers approach that further optimizes the `getNewsFeed` operation to O(10 log u) where u is the number of users. The key insight is to avoid sorting all tweets and focus only on finding the 10 most recent ones. 