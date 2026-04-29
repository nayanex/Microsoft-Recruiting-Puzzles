# Stacks Interface Guide

```python
# Declaration: we will just use a list
stack = []

# Pushing elements:
stack.append(1)
stack.append(2)
stack.append(3)

# Popping elements:
stack.pop() # 3
stack.pop() # 2

# Check if empty
not stack # False

# Check element at top
stack[-1] # 1

# Get size
len(stack) # 1
```

# Queues Interface Guide

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

---
To train your brain to "click" on the Stack pattern, you have to stop looking at the **data** (brackets vs. temperatures) and start looking at the **temporal relationship** between the elements.

The secret "click" happens when you realize both problems are about **Nested Dependencies.**

---

## 1. The Universal Question: "Who is my Match?"
To make the association, ask yourself this specific question:
> **"Does the 'resolution' of my current item depend on the most recent unresolved item I saw?"**

If the answer is **Yes**, it’s a Stack.

### The "Closing Brackets" Version (Literal Match)
* **The Debt:** An opening bracket `(` is a promise to find a `)`.
* **The Resolution:** When you see a `)`, you don't look for the *first* bracket ever opened; you look for the **most recent** one.
* **Why a Stack?** If you have `([{}])`, the `{}` must be resolved before the `[]` can be resolved. The "inner" problem must be finished first.



### The "Monotonic Stack" Version (Logical Match)
* **The Debt:** A cold day `73°` is a promise to find a warmer day.
* **The Resolution:** When `74°` arrives, it doesn't look for the first day of the year; it looks at the **most recent** day that was colder than it.
* **Why a Stack?** If you have temperatures `75, 71, 69` and a `72` arrives, it "matches" with the `69` first, then the `71`. It can't even "see" the `75` until the more recent, smaller "debts" are paid.



---

## 2. Training the "Association" (The Mental Trigger)

To build the muscle memory, categorize problems by their **Interaction Type** rather than their topic.

### Trigger A: "The Obstacle" (Monotonic)
If a new element "blocks" or "overpowers" previous elements, use a Monotonic Stack.
* **The Signal:** "Find the next greater/smaller..." or "Calculate the area constrained by the shortest..."
* **The Logic:** You keep a stack of elements that are "surviving" a trend. When the trend breaks (a bigger number appears), the "survivors" are resolved.

### Trigger B: "The Mirror" (Symmetry/Nesting)
If the problem has a "start" and an "end" that must wrap around other start/end pairs, use a Stack.
* **The Signal:** Brackets, HTML tags, directory paths (`cd ..`), or undo/redo functionality.
* **The Logic:** You are diving deeper into a hole. To get out of the hole, you have to pass through the levels in the exact reverse order you entered them.



---

## 3. The "Instant Recognition" Drill
Next time you see a problem, run this 3-step mental simulation:

1.  **Linear Scan:** Imagine moving left to right.
2.  **The "Wait" State:** Do I know the answer for this element immediately? (No? Push it to the "Wait List").
3.  **The "LIFO" Check:** When I finally find an answer, which element on the "Wait List" does it apply to? 
    * If it applies to the **Oldest** element $\rightarrow$ **Queue**.
    * If it applies to the **Most Recent** element $\rightarrow$ **Stack**.
