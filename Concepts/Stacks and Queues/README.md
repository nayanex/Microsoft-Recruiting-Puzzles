# Interface Guide

```python
# Declaration: we will use deque from the collections module
import collections
queue = collections.deque()

# If you want to initializar it with some initial values:
queue = collections.deque([1, 2, 3])

# Enqueueing / adding elements:
queue.append(4)
queue.append(5)

# Check element at front of queue (next element to be removed)
queue[0] # 3

# Get size
len(queue) # 3

while queue:
    print(queue.popleft())
    
if not queue:
    print("Queue is empty!")
```

> There is also a data structure called a **deque**, short for double-ended queue, and pronounced "deck". In a deque, you can add or delete elements from both ends. A normal queue designates adding to one end and deleting to another end.


# Monotonic

