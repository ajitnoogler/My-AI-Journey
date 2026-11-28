<h1 align="center">Python Interview Lab</h1>

<p align="center">Coding patterns commonly asked at Google, Meta, Amazon, Apple, Netflix, Microsoft, Cisco, Oracle and similar companies. Each program has a short explanation. Topic based, not a verified list per company.</p>

<p align="center"><img src="https://img.shields.io/badge/programs-77-blue" alt="programs"> <img src="https://img.shields.io/badge/modules-11-green" alt="modules"> <img src="https://img.shields.io/badge/language-Python-yellow" alt="python"></p>

---

## Course Contents

| # | Module | Programs |
|---|--------|----------|
| 01 | [Arrays and Strings](#01-arrays-and-strings) | 15 |
| 02 | [Math and Numbers](#02-math-and-numbers) | 6 |
| 03 | [Hashing and Counting](#03-hashing-and-counting) | 6 |
| 04 | [Linked List, Stack, Queue](#04-linked-list-stack-queue) | 5 |
| 05 | [Trees and Graphs](#05-trees-and-graphs) | 6 |
| 06 | [Sorting and Searching](#06-sorting-and-searching) | 5 |
| 07 | [Recursion and Dynamic Programming](#07-recursion-and-dynamic-programming) | 7 |
| 08 | [Python Language Questions](#08-python-language-questions) | 7 |
| 09 | [Systems and Infra Rounds](#09-systems-and-infra-rounds) | 8 |
| 10 | [Date and Time](#10-date-and-time) | 8 |
| 11 | [OOP and Design](#11-oop-and-design) | 4 |

**Levels:** 🟢 Easy &nbsp; 🟡 Medium &nbsp; 🔴 Hard

> Use your browser find (Ctrl+F or Cmd+F) to search for a topic, for example "heap", "regex" or "date".

---

## 01. Arrays and Strings

### #1 Reverse string and palindrome  
🟢 Easy

> **Explanation:** Slicing with `[::-1]` reverses the string. A palindrome equals its own reverse, so lower-case first to ignore case. O(n).

```python
s = "Level"
t = s.lower()
print(t == t[::-1], s[::-1])
```

### #2 Valid anagram  
🟢 Easy

> **Explanation:** Two words are anagrams if every letter appears the same number of times. Counter builds and compares the counts. O(n).

```python
from collections import Counter
print(Counter("listen") == Counter("silent"))
```

### #3 First non-repeating character  
🟢 Easy

> **Explanation:** Count every character once, then scan the string again and return the first one whose count is 1. O(n).

```python
from collections import Counter
s = "swiss"; c = Counter(s)
print(next((ch for ch in s if c[ch] == 1), None))
```

### #4 Two Sum  
🟢 Easy

> **Explanation:** Store each value with its index in a dict. For each number, check if target minus number was already seen. One pass, O(n) time and space.

```python
def two_sum(nums, target):
    seen = {}
    for i, n in enumerate(nums):
        if target - n in seen: return [seen[target - n], i]
        seen[n] = i
print(two_sum([2, 7, 11, 15], 9))
```

### #5 Valid parentheses  
🟢 Easy

> **Explanation:** Push openers on a stack. Each closer must match the most recent opener. The string is valid only if the stack ends empty. O(n).

```python
def valid(s):
    st, pair = [], {")": "(", "]": "[", "}": "{"}
    for ch in s:
        if ch in pair:
            if not st or st.pop() != pair[ch]: return False
        else: st.append(ch)
    return not st
print(valid("{[()]}"))
```

### #6 Longest substring without repeating chars  
🟡 Medium

> **Explanation:** Sliding window with a last-seen index per character. When a repeat appears inside the window, move the left edge just past it. O(n).

```python
def longest(s):
    seen, l, best = {}, 0, 0
    for r, ch in enumerate(s):
        if ch in seen and seen[ch] >= l: l = seen[ch] + 1
        seen[ch] = r; best = max(best, r - l + 1)
    return best
print(longest("abcabcbb"))
```

### #7 Maximum subarray (Kadane)  
🟡 Medium

> **Explanation:** At each element, either extend the running sum or restart from this element. Track the best sum seen. O(n) time, O(1) space.

```python
def kadane(a):
    cur = best = a[0]
    for x in a[1:]:
        cur = max(x, cur + x); best = max(best, cur)
    return best
print(kadane([-2, 1, -3, 4, -1, 2, 1, -5, 4]))
```

### #8 Merge intervals  
🟡 Medium

> **Explanation:** Sort by start. If the next interval starts before the last one ends, extend the last end, else append a new interval. O(n log n).

```python
def merge(iv):
    iv.sort(); out = [iv[0]]
    for s, e in iv[1:]:
        if s <= out[-1][1]: out[-1][1] = max(out[-1][1], e)
        else: out.append([s, e])
    return out
print(merge([[1, 3], [2, 6], [8, 10]]))
```

### #9 Move zeroes in place  
🟢 Easy

> **Explanation:** Pointer k marks the next slot for a non-zero value. Swap each non-zero element into it. Order is kept, no extra list. O(n).

```python
a = [0, 1, 0, 3, 12]
k = 0
for i in range(len(a)):
    if a[i]: a[k], a[i] = a[i], a[k]; k += 1
print(a)
```

### #10 Product of array except self  
🟡 Medium

> **Explanation:** Pass 1 stores the product of everything to the left, pass 2 multiplies in everything to the right. No division, so zeros are safe. O(n).

```python
def prod(a):
    out, p = [1] * len(a), 1
    for i in range(len(a)): out[i] = p; p *= a[i]
    p = 1
    for i in range(len(a) - 1, -1, -1): out[i] *= p; p *= a[i]
    return out
print(prod([1, 2, 3, 4]))
```

### #11 Rotate list by k and common prefix  
🟢 Easy

> **Explanation:** Rotation is the last k items placed in front of the rest. os.path.commonprefix finds the shared leading characters of strings. O(n).

```python
import os
a = [1, 2, 3, 4, 5]; k = 2 % len(a)
print(a[-k:] + a[:-k])
print(os.path.commonprefix(["flower", "flow", "flight"]))
```

### #12 3Sum  
🟡 Medium

> **Explanation:** Sort, fix one number, then use two pointers on the rest to find pairs that cancel it. Skip duplicates so triplets are unique. O(n^2).

```python
def three_sum(a):
    a.sort(); out = []
    for i in range(len(a) - 2):
        if i and a[i] == a[i - 1]: continue
        l, r = i + 1, len(a) - 1
        while l < r:
            s = a[i] + a[l] + a[r]
            if s < 0: l += 1
            elif s > 0: r -= 1
            else:
                out.append([a[i], a[l], a[r]]); l += 1
                while l < r and a[l] == a[l - 1]: l += 1
    return out
print(three_sum([-1, 0, 1, 2, -1, -4]))
```

### #13 Container with most water  
🟡 Medium

> **Explanation:** Two pointers at both ends. Area depends on the shorter wall, so move the shorter side inward to try for a taller one. O(n).

```python
def water(h):
    l, r, best = 0, len(h) - 1, 0
    while l < r:
        best = max(best, min(h[l], h[r]) * (r - l))
        if h[l] < h[r]: l += 1
        else: r -= 1
    return best
print(water([1, 8, 6, 2, 5, 4, 8, 3, 7]))
```

### #14 Flatten nested list  
🟢 Easy

> **Explanation:** A recursive generator. If an item is a list, delegate to it with yield from, else yield the value. Handles any depth lazily.

```python
def flat(x):
    for i in x:
        if isinstance(i, list): yield from flat(i)
        else: yield i
print(list(flat([1, [2, [3, 4]], 5])))
```

### #15 Rotate matrix 90 degrees  
🟡 Medium

> **Explanation:** Reverse the row order, then transpose with zip(*). Together these give a clockwise rotation. O(n^2).

```python
m = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
print([list(r) for r in zip(*m[::-1])])
```

<p align="right"><a href="#course-contents">Back to top</a></p>

---

## 02. Math and Numbers

### #1 Fibonacci  
🟢 Easy

> **Explanation:** Keep only the last two values and update them in a loop. Avoids the exponential cost of plain recursion. O(n) time, O(1) space.

```python
def fib(n):
    a, b = 0, 1
    for _ in range(n): a, b = b, a + b
    return a
print([fib(i) for i in range(10)])
```

### #2 Sieve of Eratosthenes  
🟡 Medium

> **Explanation:** Assume all numbers are prime, then for each prime cross out its multiples starting at its square. O(n log log n).

```python
def sieve(n):
    p = [True] * (n + 1); p[0:2] = [False, False]
    for i in range(2, int(n ** 0.5) + 1):
        if p[i]: p[i*i::i] = [False] * len(p[i*i::i])
    return [i for i, v in enumerate(p) if v]
print(sieve(50))
```

### #3 GCD and LCM  
🟢 Easy

> **Explanation:** math.gcd uses Euclid algorithm. LCM follows from a * b / gcd(a, b).

```python
from math import gcd
a, b = 12, 18
print(gcd(a, b), a * b // gcd(a, b))
```

### #4 Armstrong number  
🟢 Easy

> **Explanation:** A number equals the sum of its digits each raised to the number of digits. 153 = 1^3 + 5^3 + 3^3.

```python
n = 153
print(n == sum(int(d) ** len(str(n)) for d in str(n)))
```

### #5 Set bits and power of two  
🟢 Easy

> **Explanation:** bin(n).count of 1 counts set bits. n & (n - 1) clears the lowest set bit, so a power of two (exactly one bit) gives 0.

```python
n = 16
print(bin(n).count("1"), n > 0 and n & (n - 1) == 0)
```

### #6 FizzBuzz  
🟢 Easy

> **Explanation:** Build the word by string concatenation. An empty string is falsy, so the or falls back to the number itself. O(n).

```python
for i in range(1, 16):
    print("Fizz" * (i % 3 == 0) + "Buzz" * (i % 5 == 0) or i)
```

<p align="right"><a href="#course-contents">Back to top</a></p>

---

## 03. Hashing and Counting

### #1 Group anagrams  
🟡 Medium

> **Explanation:** Words with the same sorted letters are anagrams, so the sorted word is the dict key. O(n * k log k) for k-letter words.

```python
from collections import defaultdict
g = defaultdict(list)
for w in ["eat", "tea", "tan", "ate", "nat", "bat"]:
    g["".join(sorted(w))].append(w)
print(list(g.values()))
```

### #2 Top K frequent elements  
🟡 Medium

> **Explanation:** Counter tallies frequencies and most_common(k) returns the k largest. About O(n log k).

```python
from collections import Counter
print([x for x, _ in Counter([1, 1, 1, 2, 2, 3]).most_common(2)])
```

### #3 Longest consecutive sequence  
🟡 Medium

> **Explanation:** Put numbers in a set. Only start counting from a number with no predecessor, so each run is walked once. O(n).

```python
def longest(a):
    s, best = set(a), 0
    for n in s:
        if n - 1 not in s:
            m = n
            while m + 1 in s: m += 1
            best = max(best, m - n + 1)
    return best
print(longest([100, 4, 200, 1, 3, 2]))
```

### #4 Subarray sum equals K  
🟡 Medium

> **Explanation:** Keep a running prefix sum and a count of earlier prefix sums. Any earlier prefix equal to total minus k ends a valid subarray. O(n).

```python
def count(a, k):
    pre, c, tot = {0: 1}, 0, 0
    for x in a:
        tot += x; c += pre.get(tot - k, 0)
        pre[tot] = pre.get(tot, 0) + 1
    return c
print(count([1, 1, 1], 2))
```

### #5 LRU cache  
🟡 Medium

> **Explanation:** OrderedDict keeps usage order. move_to_end marks a key as recent and popitem(last=False) evicts the oldest. get and put are O(1).

```python
from collections import OrderedDict
class LRU:
    def __init__(self, cap): self.c, self.d = cap, OrderedDict()
    def get(self, k):
        if k not in self.d: return -1
        self.d.move_to_end(k); return self.d[k]
    def put(self, k, v):
        self.d[k] = v; self.d.move_to_end(k)
        if len(self.d) > self.c: self.d.popitem(last=False)
```

### #6 Missing number and duplicates  
🟢 Easy

> **Explanation:** Missing number: expected sum n(n+1)/2 minus actual sum. Duplicates: values seen more than once. count() makes this O(n^2), use Counter or a set for O(n).

```python
a = [3, 0, 1]
print(len(a) * (len(a) + 1) // 2 - sum(a))
b = [1, 3, 4, 2, 2]
print([x for x in set(b) if b.count(x) > 1])
```

<p align="right"><a href="#course-contents">Back to top</a></p>

---

## 04. Linked List, Stack, Queue

### #1 Reverse a linked list  
🟢 Easy

> **Explanation:** Walk the list and flip each next pointer to the previous node. O(n) time, O(1) space.

```python
class Node:
    def __init__(self, v, nxt=None): self.v, self.next = v, nxt
def reverse(h):
    prev = None
    while h: h.next, prev, h = prev, h, h.next
    return prev
h = reverse(Node(1, Node(2, Node(3))))
while h: print(h.v, end=" "); h = h.next
```

### #2 Detect cycle (Floyd)  
🟡 Medium

> **Explanation:** A slow pointer moves 1 step and a fast one moves 2. If there is a loop they meet, else fast reaches the end. O(n), O(1) space.

```python
def has_cycle(h):
    s = f = h
    while f and f.next:
        s, f = s.next, f.next.next
        if s is f: return True
    return False
```

### #3 Merge sorted lists  
🟢 Easy

> **Explanation:** heapq.merge lazily merges already sorted inputs. In interviews you may need to splice nodes with two pointers. O(n + m).

```python
import heapq
print(list(heapq.merge([1, 3, 5], [2, 4, 6])))
```

### #4 Min stack  
🟡 Medium

> **Explanation:** Store each value together with the minimum so far. Then push, pop and get_min are all O(1).

```python
class MinStack:
    def __init__(self): self.s = []
    def push(self, x): self.s.append((x, min(x, self.s[-1][1]) if self.s else x))
    def pop(self): return self.s.pop()[0]
    def get_min(self): return self.s[-1][1]
```

### #5 Queue using two stacks  
🟢 Easy

> **Explanation:** Push to the input stack. When popping and the output stack is empty, move everything over, which reverses order into FIFO. Amortized O(1).

```python
class Q:
    def __init__(self): self.i, self.o = [], []
    def push(self, x): self.i.append(x)
    def pop(self):
        if not self.o:
            while self.i: self.o.append(self.i.pop())
        return self.o.pop()
```

<p align="right"><a href="#course-contents">Back to top</a></p>

---

## 05. Trees and Graphs

### #1 Tree depth and inorder  
🟢 Easy

> **Explanation:** Depth is 1 plus the deeper child. Inorder visits left, node, right, which gives sorted order for a BST. O(n).

```python
class T:
    def __init__(self, v, l=None, r=None): self.v, self.l, self.r = v, l, r
def depth(t): return 0 if not t else 1 + max(depth(t.l), depth(t.r))
def inorder(t): return inorder(t.l) + [t.v] + inorder(t.r) if t else []
t = T(2, T(1), T(3))
print(depth(t), inorder(t))
```

### #2 Validate BST  
🟡 Medium

> **Explanation:** Pass an allowed (low, high) range down the tree. Each node must fall inside it. Comparing only with the parent is a common bug. O(n).

```python
# uses class T from the previous program
def valid(t, lo=float("-inf"), hi=float("inf")):
    if not t: return True
    return lo < t.v < hi and valid(t.l, lo, t.v) and valid(t.r, t.v, hi)
```

### #3 Level order traversal (BFS)  
🟡 Medium

> **Explanation:** Process a queue one whole level at a time, collecting the children for the next level. O(n).

```python
from collections import deque
def levels(t):
    out, q = [], deque([t] if t else [])
    while q:
        out.append([n.v for n in q])
        q = deque(c for n in q for c in (n.l, n.r) if c)
    return out
```

### #4 Number of islands (DFS)  
🟡 Medium

> **Explanation:** Scan the grid. On each unvisited land cell, flood-fill it with DFS to sink the whole island and count one. O(rows * cols).

```python
def islands(g):
    def dfs(i, j):
        if 0 <= i < len(g) and 0 <= j < len(g[0]) and g[i][j] == "1":
            g[i][j] = "0"
            for di, dj in ((1, 0), (-1, 0), (0, 1), (0, -1)): dfs(i + di, j + dj)
    n = 0
    for i in range(len(g)):
        for j in range(len(g[0])):
            if g[i][j] == "1": dfs(i, j); n += 1
    return n
```

### #5 Dijkstra shortest path  
🔴 Hard

> **Explanation:** A min-heap always pops the closest unfinished node, then relaxes its edges. Needs non-negative weights. O((V + E) log V).

```python
import heapq
def dijkstra(g, s):
    d, pq = {s: 0}, [(0, s)]
    while pq:
        w, u = heapq.heappop(pq)
        if w > d.get(u, 1e9): continue
        for v, c in g[u]:
            if w + c < d.get(v, 1e9):
                d[v] = w + c; heapq.heappush(pq, (d[v], v))
    return d
g = {"A": [("B", 1), ("C", 4)], "B": [("C", 2)], "C": []}
print(dijkstra(g, "A"))
```

### #6 Topological sort (Kahn)  
🟡 Medium

> **Explanation:** Start with nodes of in-degree 0, remove them and lower neighbors in-degree. If fewer than n nodes come out, there is a cycle. O(V + E).

```python
from collections import deque
def topo(n, edges):
    adj, indeg = [[] for _ in range(n)], [0] * n
    for a, b in edges: adj[a].append(b); indeg[b] += 1
    q, out = deque(i for i in range(n) if not indeg[i]), []
    while q:
        u = q.popleft(); out.append(u)
        for v in adj[u]:
            indeg[v] -= 1
            if not indeg[v]: q.append(v)
    return out if len(out) == n else []
```

<p align="right"><a href="#course-contents">Back to top</a></p>

---

## 06. Sorting and Searching

### #1 Binary search  
🟢 Easy

> **Explanation:** Halve the search range each step by comparing with the middle. Input must be sorted. O(log n).

```python
def bs(a, t):
    lo, hi = 0, len(a) - 1
    while lo <= hi:
        m = (lo + hi) // 2
        if a[m] == t: return m
        lo, hi = (m + 1, hi) if a[m] < t else (lo, m - 1)
    return -1
```

### #2 Merge sort  
🟡 Medium

> **Explanation:** Split in half, sort each half, merge. Stable and always O(n log n). pop(0) is slow here, use indexes in production code.

```python
def msort(a):
    if len(a) < 2: return a
    l, r = msort(a[:len(a) // 2]), msort(a[len(a) // 2:])
    out = []
    while l and r: out.append(l.pop(0) if l[0] <= r[0] else r.pop(0))
    return out + l + r
```

### #3 Quick sort  
🟡 Medium

> **Explanation:** Pick a pivot and split into smaller, equal and larger parts, then recurse. Average O(n log n), worst O(n^2). This version is not in place.

```python
def qsort(a):
    if len(a) < 2: return a
    p = a[len(a) // 2]
    return qsort([x for x in a if x < p]) + [x for x in a if x == p] + qsort([x for x in a if x > p])
```

### #4 Kth largest element (heap)  
🟡 Medium

> **Explanation:** nlargest keeps a heap of size k, so the kth largest is the last result. O(n log k).

```python
import heapq
print(heapq.nlargest(2, [3, 2, 1, 5, 6, 4])[-1])
```

### #5 Search in rotated sorted array  
🟡 Medium

> **Explanation:** At each step one half is still sorted. Check whether the target lies in that half and discard the other. O(log n).

```python
def search(a, t):
    lo, hi = 0, len(a) - 1
    while lo <= hi:
        m = (lo + hi) // 2
        if a[m] == t: return m
        if a[lo] <= a[m]:
            if a[lo] <= t < a[m]: hi = m - 1
            else: lo = m + 1
        else:
            if a[m] < t <= a[hi]: lo = m + 1
            else: hi = m - 1
    return -1
```

<p align="right"><a href="#course-contents">Back to top</a></p>

---

## 07. Recursion and Dynamic Programming

### #1 Climbing stairs  
🟢 Easy

> **Explanation:** Ways to reach step n equals ways to n-1 plus ways to n-2, which is Fibonacci. O(n) time, O(1) space.

```python
def climb(n):
    a, b = 1, 1
    for _ in range(n - 1): a, b = b, a + b
    return b
print(climb(5))
```

### #2 Coin change  
🟡 Medium

> **Explanation:** dp[a] is the fewest coins for amount a, built from dp[a - coin] + 1. Returns -1 if impossible. O(amount * coins).

```python
def coins(c, amt):
    dp = [0] + [float("inf")] * amt
    for a in range(1, amt + 1):
        for x in c:
            if x <= a: dp[a] = min(dp[a], dp[a - x] + 1)
    return dp[amt] if dp[amt] != float("inf") else -1
print(coins([1, 2, 5], 11))
```

### #3 Longest common subsequence  
🟡 Medium

> **Explanation:** dp[i][j] extends the diagonal on a match, else takes the better of the top or left cell. O(n * m).

```python
def lcs(a, b):
    dp = [[0] * (len(b) + 1) for _ in range(len(a) + 1)]
    for i in range(len(a)):
        for j in range(len(b)):
            dp[i+1][j+1] = dp[i][j] + 1 if a[i] == b[j] else max(dp[i][j+1], dp[i+1][j])
    return dp[-1][-1]
print(lcs("abcde", "ace"))
```

### #4 0/1 Knapsack  
🟡 Medium

> **Explanation:** 1D dp over capacity. Loop capacity backwards so each item is used at most once. O(items * capacity).

```python
def knap(w, v, cap):
    dp = [0] * (cap + 1)
    for wi, vi in zip(w, v):
        for c in range(cap, wi - 1, -1): dp[c] = max(dp[c], dp[c - wi] + vi)
    return dp[cap]
print(knap([1, 3, 4], [15, 20, 30], 4))
```

### #5 Permutations and subsets  
🟡 Medium

> **Explanation:** itertools generates them directly. Interviewers often want the backtracking version. Sizes are n! and 2^n.

```python
from itertools import permutations, combinations
print(list(permutations([1, 2, 3]))[:3])
a = [1, 2, 3]
print([c for r in range(len(a) + 1) for c in combinations(a, r)])
```

### #6 Longest increasing subsequence  
🟡 Medium

> **Explanation:** Keep a tails array and place each number with bisect, replacing the first tail that is not smaller. Length of tails is the answer. O(n log n).

```python
import bisect
def lis(a):
    t = []
    for x in a:
        i = bisect.bisect_left(t, x)
        if i == len(t): t.append(x)
        else: t[i] = x
    return len(t)
print(lis([10, 9, 2, 5, 3, 7, 101, 18]))
```

### #7 Edit distance  
🔴 Hard

> **Explanation:** dp over prefixes: insert, delete or replace costs 1, a match costs 0. One rolling row saves memory. O(n * m).

```python
def edit(a, b):
    dp = list(range(len(b) + 1))
    for i in range(1, len(a) + 1):
        prev, dp[0] = dp[0], i
        for j in range(1, len(b) + 1):
            prev, dp[j] = dp[j], prev if a[i-1] == b[j-1] else 1 + min(prev, dp[j], dp[j-1])
    return dp[-1]
print(edit("horse", "ros"))
```

<p align="right"><a href="#course-contents">Back to top</a></p>

---

## 08. Python Language Questions

### #1 Mutable default argument trap  
🟢 Easy

> **Explanation:** A default list is created once when the function is defined, so every call shares it. Use None as the default and create the list inside.

```python
def add(x, lst=[]):
    lst.append(x); return lst
print(add(1), add(2))   # [1, 2] [1, 2] : shared list
def safe(x, lst=None):
    lst = [] if lst is None else lst
    lst.append(x); return lst
```

### #2 Shallow vs deep copy  
🟢 Easy

> **Explanation:** copy.copy duplicates only the outer list, so nested lists are still shared. deepcopy duplicates everything.

```python
import copy
a = [[1, 2], [3]]
s, d = copy.copy(a), copy.deepcopy(a)
a[0].append(9)
print(s[0], d[0])   # [1, 2, 9] [1, 2]
```

### #3 is vs == and None check  
🟢 Easy

> **Explanation:** == compares values, is compares object identity. Use is None for None checks.

```python
a, b = [1], [1]
print(a == b, a is b)   # True False
x = None
print(x is None)
```

### #4 Late binding in closures  
🟡 Medium

> **Explanation:** A lambda looks up i when it is called, after the loop ended. Bind the current value with a default argument i=i.

```python
fs = [lambda: i for i in range(3)]
print([f() for f in fs])    # [2, 2, 2]
fs = [lambda i=i: i for i in range(3)]
print([f() for f in fs])    # [0, 1, 2]
```

### #5 Decorator with arguments  
🟡 Medium

> **Explanation:** Three layers: repeat(n) returns the decorator, which returns the wrapper. functools.wraps keeps the original name and docstring.

```python
import functools
def repeat(n):
    def deco(fn):
        @functools.wraps(fn)
        def w(*a, **k):
            for _ in range(n): r = fn(*a, **k)
            return r
        return w
    return deco
@repeat(3)
def hi(): print("hi")
hi()
```

### #6 Generator vs list memory  
🟢 Easy

> **Explanation:** A generator produces values lazily so it uses tiny constant memory. A list stores every item at once.

```python
import sys
print(sys.getsizeof([i for i in range(10**6)]))
print(sys.getsizeof(i for i in range(10**6)))
```

### #7 GIL: threads vs processes  
🟡 Medium

> **Explanation:** The GIL lets one thread run Python bytecode at a time. CPU-bound work needs processes, I/O-bound work is fine with threads or asyncio.

```python
import time
from concurrent.futures import ThreadPoolExecutor, ProcessPoolExecutor
def cpu(n): return sum(i * i for i in range(n))
if __name__ == "__main__":
    for Ex in (ThreadPoolExecutor, ProcessPoolExecutor):
        t = time.time()
        with Ex(4) as ex: list(ex.map(cpu, [3 * 10**6] * 4))
        print(Ex.__name__, round(time.time() - t, 2))
# CPU bound: processes win. I/O bound: threads or asyncio win.
```

<p align="right"><a href="#course-contents">Back to top</a></p>

---

## 09. Systems and Infra Rounds

### #1 Parse log, top error IP  
🟡 Medium

> **Explanation:** Filter lines whose status starts with 5, then count by IP with Counter and take the top. O(n).

```python
import re
from collections import Counter
log = ["10.0.0.1 GET 500", "10.0.0.2 GET 200", "10.0.0.1 POST 500"]
errs = Counter(l.split()[0] for l in log if re.search(r" 5\d\d$", l))
print(errs.most_common(1))
```

### #2 IPv4 and CIDR validation  
🟢 Easy

> **Explanation:** The ipaddress module validates addresses, checks is_private, and tests membership in a network. Better than hand-written regex.

```python
import ipaddress
print(ipaddress.ip_address("192.168.1.10").is_private)
net = ipaddress.ip_network("10.0.0.0/24")
print(net.num_addresses, ipaddress.ip_address("10.0.0.5") in net)
```

### #3 Longest prefix match for routes  
🔴 Hard

> **Explanation:** Collect every route that contains the IP and pick the longest prefix length, which is how routers choose. Large tables use a trie.

```python
import ipaddress
routes = {"10.0.0.0/8": "A", "10.1.0.0/16": "B", "0.0.0.0/0": "C"}
def lpm(ip):
    ip = ipaddress.ip_address(ip)
    m = [(ipaddress.ip_network(p), n) for p, n in routes.items() if ip in ipaddress.ip_network(p)]
    return max(m, key=lambda x: x[0].prefixlen)[1]
print(lpm("10.1.2.3"))   # B
```

### #4 Token bucket rate limiter  
🔴 Hard

> **Explanation:** Tokens refill at a fixed rate up to a cap. A request is allowed if a token is available, which permits short bursts.

```python
import time
class Bucket:
    def __init__(self, rate, cap): self.rate, self.cap, self.t, self.tok = rate, cap, time.time(), cap
    def allow(self):
        now = time.time()
        self.tok = min(self.cap, self.tok + (now - self.t) * self.rate); self.t = now
        if self.tok >= 1: self.tok -= 1; return True
        return False
```

### #5 Producer and consumer with Queue  
🟡 Medium

> **Explanation:** queue.Queue is thread-safe and the maxsize gives backpressure. None is a sentinel telling the consumer to stop.

```python
import queue, threading
q = queue.Queue(maxsize=5)
def prod():
    for i in range(5): q.put(i)
    q.put(None)
def cons():
    while (x := q.get()) is not None: print("got", x)
for f in (prod, cons): threading.Thread(target=f).start()
```

### #6 Retry decorator with backoff  
🟡 Medium

> **Explanation:** Wait 0.5s, 1s, 2s between attempts and re-raise on the last one. Add random jitter in production to avoid retry storms.

```python
import time, functools
def retry(n=3, base=0.5):
    def deco(fn):
        @functools.wraps(fn)
        def w(*a, **k):
            for i in range(n):
                try: return fn(*a, **k)
                except Exception:
                    if i == n - 1: raise
                    time.sleep(base * 2 ** i)
        return w
    return deco
```

### #7 Word frequency from a file  
🟢 Easy

> **Explanation:** Regex extracts lower-cased words and Counter tallies them. For huge files, read in chunks or line by line.

```python
from collections import Counter
import re
words = re.findall(r"\w+", open("a.txt").read().lower())
print(Counter(words).most_common(5))
```

### #8 SQL with sqlite3 (group, having)  
🟡 Medium

> **Explanation:** GROUP BY aggregates per department and HAVING filters the groups after aggregation. Parameterized inserts prevent SQL injection.

```python
import sqlite3
c = sqlite3.connect(":memory:")
c.execute("create table emp(name text, dept text, sal int)")
c.executemany("insert into emp values(?,?,?)", [("a", "IT", 90), ("b", "IT", 70), ("c", "HR", 60)])
print(c.execute("select dept, avg(sal) from emp group by dept having avg(sal) > 65").fetchall())
```

<p align="right"><a href="#course-contents">Back to top</a></p>

---

## 10. Date and Time

### #1 Validate date with datetime.strptime  
🟢 Easy

> **Explanation:** strptime raises ValueError for impossible dates, including Feb 29 in non-leap years and month 13, so try/except is a full validator. This is the answer to give first.

```python
from datetime import datetime
def valid(s, fmt="%Y-%m-%d"):
    try: datetime.strptime(s, fmt); return True
    except ValueError: return False
print(valid("2024-02-29"), valid("2023-02-29"), valid("2024-13-01"))
```

### #2 Validate date without libraries  
🟡 Medium

> **Explanation:** Check the month is 1 to 12, then the day against a days-per-month list where February depends on the leap rule (divisible by 400, or by 4 but not 100). O(1).

```python
def is_leap(y): return y % 400 == 0 or (y % 4 == 0 and y % 100 != 0)
def valid(d, m, y):
    if y < 1 or not 1 <= m <= 12: return False
    days = [31, 29 if is_leap(y) else 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]
    return 1 <= d <= days[m - 1]
print(valid(29, 2, 2024), valid(29, 2, 2023), valid(31, 4, 2024))
```

### #3 Validate dd/mm/yyyy with regex  
🟡 Medium

> **Explanation:** Regex checks only the shape (two digits, two digits, four digits). The date() constructor then checks that the day exists. Never rely on regex alone for dates.

```python
import re
from datetime import date
def valid(s):
    m = re.fullmatch(r"(\d{2})/(\d{2})/(\d{4})", s)
    if not m: return False
    d, mo, y = map(int, m.groups())
    try: date(y, mo, d); return True
    except ValueError: return False
print(valid("29/02/2024"), valid("31/04/2024"), valid("1/1/2024"))
```

### #4 Parse multiple date formats  
🟡 Medium

> **Explanation:** Try each format in turn and return the first that parses. Note that dd/mm and mm/dd are ambiguous for dates like 03/04/2024, so the order of formats decides the result.

```python
from datetime import datetime
FMTS = ("%Y-%m-%d", "%d/%m/%Y", "%d-%b-%Y", "%m/%d/%Y")
def parse(s):
    for f in FMTS:
        try: return datetime.strptime(s, f).date()
        except ValueError: pass
    return None
print(parse("29-Feb-2024"), parse("2023-02-29"))
```

### #5 Day of week and days between dates  
🟢 Easy

> **Explanation:** Subtracting two date objects gives a timedelta, and .days is the gap. strftime with %A gives the weekday name.

```python
from datetime import date
a, b = date(2024, 1, 1), date(2024, 12, 25)
print(a.strftime("%A"), (b - a).days)
```

### #6 Add business days  
🟡 Medium

> **Explanation:** Step one day at a time and count only weekdays (weekday() below 5). Holidays are not handled, a real version needs a holiday set.

```python
from datetime import date, timedelta
def add_business_days(d, n):
    while n:
        d += timedelta(days=1)
        if d.weekday() < 5: n -= 1
    return d
print(add_business_days(date(2024, 12, 20), 5))
```

### #7 Age from date of birth  
🟢 Easy

> **Explanation:** Subtract the years, then subtract 1 if this year birthday has not happened yet. The tuple comparison (month, day) does that check neatly.

```python
from datetime import date
def age(dob, today=None):
    t = today or date.today()
    return t.year - dob.year - ((t.month, t.day) < (dob.month, dob.day))
print(age(date(1990, 6, 15), date(2024, 6, 14)))   # 33
```

### #8 Timezone conversion with zoneinfo  
🟡 Medium

> **Explanation:** Attach a timezone to the datetime, then astimezone converts it and handles daylight saving automatically. Store times in UTC and convert only for display.

```python
from datetime import datetime
from zoneinfo import ZoneInfo
t = datetime(2024, 3, 10, 9, 0, tzinfo=ZoneInfo("Asia/Kolkata"))
print(t.astimezone(ZoneInfo("America/New_York")))
```

<p align="right"><a href="#course-contents">Back to top</a></p>

---

## 11. OOP and Design

### #1 Singleton  
🟢 Easy

> **Explanation:** `__new__` returns the same instance every time. A module-level object is often the simpler Python answer.

```python
class Single:
    _i = None
    def __new__(cls):
        if cls._i is None: cls._i = super().__new__(cls)
        return cls._i
print(Single() is Single())
```

### #2 Bank account with custom exception  
🟢 Easy

> **Explanation:** A custom exception gives a meaningful error. Validate before changing state so the balance is never corrupted.

```python
class Insufficient(Exception): pass
class Account:
    def __init__(self, bal=0): self.bal = bal
    def withdraw(self, x):
        if x > self.bal: raise Insufficient(f"balance {self.bal}")
        self.bal -= x
```

### #3 Parking lot  
🟡 Medium

> **Explanation:** A list of free slots plus a plate to slot map. park pops a free slot and leave returns it. Use deque for O(1) pops.

```python
class Lot:
    def __init__(self, n): self.free, self.cars = list(range(n)), {}
    def park(self, plate):
        if not self.free: return None
        s = self.free.pop(0); self.cars[plate] = s; return s
    def leave(self, plate): self.free.append(self.cars.pop(plate))
```

### #4 Event emitter (observer)  
🟡 Medium

> **Explanation:** A dict maps event names to handler lists, and emit calls each handler. This is the base of pub/sub designs.

```python
class Emitter:
    def __init__(self): self.h = {}
    def on(self, e, fn): self.h.setdefault(e, []).append(fn)
    def emit(self, e, *a): [fn(*a) for fn in self.h.get(e, [])]
em = Emitter(); em.on("alert", print); em.emit("alert", "link down")
```

<p align="right"><a href="#course-contents">Back to top</a></p>

---

