```python
class ListNode:
    def __init__(self, val):
        self.val = val
        self.next = None
    
one = ListNode(1)
two = ListNode(2)
three = ListNode(3)
one.next = two
two.next = three
head = one

print(head.val)
print(head.next.val)
print(head.next.next.val)
```

### Summary Comparison

| Operation | Array (Dynamic) | Linked List |
| --- | --- | --- |
| **Accessing $i$-th element** | **$O(1)$** (Instant) | $O(n)$ (Must walk) |
| **Insert/Delete at Start** | $O(n)$ (Must shift everything) | **$O(1)$** (Just change pointers) |
| **Insert/Delete at Middle** | $O(n)$ (Shifting + Finding) | **$O(1)$** (If you are already there) |
| **Memory Overhead** | Low (Just data) | **High** (Data + Pointers) |


## Traversal

```python
def get_sum(head):
    ans = 0
    while head:
        ans += head.val
        head = head.next
    
    return ans
```

Traversal can also be done recursively:

```python
def get_sum(head):
    if not head:
        return 0
    
    return head.val + get_sum(head.next)
```

Here is the information transformed into a clean Markdown format.

# Types of Linked Lists

## Singly Linked List

This is the most common type of linked list. In a singly linked list, each node only has a pointer to the **next** node. This means you can only move forward in the list when iterating. The pointer used to reference the next node is usually called `next`.

### Adding an Element at Position $i$

To add an element to a linked list so that it becomes the element at position $i$, you must follow these steps:

1. **Locate the Predecessor:** You need a pointer to the element currently at position $i - 1$.
2. **Identify the Successor:** Let the element currently at position $i$ be called $x$. After insertion, $x$ will be pushed to position $i + 1$.
3. **Re-route Pointers:**
* The **new node** should point to $x$ (the old node at position $i$) as its `next` node.
* The **node at $i - 1$** should point to the **new node** as its `next` node.

---

### Conceptual Logic (Python Example)

```python
class ListNode:
    def __init__(self, val):
        self.val = val
        self.next = None

# Let prev_node be the node at position i - 1
def add_node(prev_node, node_to_add):
    node_to_add.next = prev_node.next
    prev_node.next = node_to_add

```

Let's say you want to delete the element at position `i`. Again, you need to have a pointer to the element currently at position `i - 1`. The element at position `i + 1`, call it `x`, will be shifted over to be at position i after the deletion. Therefore, you should set x as the next node to the element currently at position `i - 1`. Here's some code and images demonstrating:

```python
class ListNode:
    def __init__(self, val):
        self.val = val
        self.next = None

# Let prev_node be the node at position i - 1
def delete_node(prev_node):
    prev_node.next = prev_node.next.next
```

Because the node being deleted could only have been reached from `prevNode` and we have now severed that connection, it is no longer part of the list.

## Doubly Linked List

A doubly linked list is like a singly linked list, but each node also contains a pointer to the previous node. This pointer is usually called `prev`, and it allows iteration in both directions.

In a singly linked list, we needed a reference to the node at `i - 1` if we wanted to add or remove at i. This is because we needed to perform operations on the `prevNode`. With a doubly linked list, we only need a reference to the node at `i`. This is because we can simply reference the prev pointer of that node to get the node at `i - 1`, and then do the exact same operations as above.

With a doubly linked list, we need to do extra work to also update the `prev` pointers.

```python
class ListNode:
    def __init__(self, val):
        self.val = val
        self.next = None
        self.prev = None

# Let node be the node at position i
def add_node(node, node_to_add):
    prev_node = node.prev
    node_to_add.next = node
    node_to_add.prev = prev_node
    prev_node.next = node_to_add
    node.prev = node_to_add

# Let node be the node at position i
def delete_node(node):
    prev_node = node.prev
    next_node = node.next
    prev_node.next = next_node
    next_node.prev = prev_node
```

## Linked Lists with Sentinel Nodes

> We call the start of a linked list the `head` and the end of a linked list the `tail`.

Sentinel nodes sit at the start and end of linked lists and are used to make operations and the code needed to execute those operations cleaner. The idea is that, even when there are no nodes in a linked list, you still keep pointers to a `head` and `tail`. The real head of the linked list is `head.next` and the real tail is `tail.prev`. The sentinel nodes themselves are not part of our linked list.

> The previous code we looked at is prone to errors. For example, if we are trying to delete the last node in the list, then `nextNode` will be `null`, and trying to access `nextNode.next` would result in an error. With sentinel nodes, we don't need to worry about this scenario because the last node's `next` points to the sentinel tail.

The sentinel nodes also allow us to easily add and remove from the front or back of the linked list. Recall that addition and removal is only *O(1)* if we have a reference to the node at the position we are performing the operation on. With the sentinel tail node, we can perform operations at the end of the list in *O(1)*.

```python
class ListNode:
    def __init__(self, val):
        self.val = val
        self.next = None
        self.prev = None

def add_to_end(node_to_add):
    node_to_add.next = tail
    node_to_add.prev = tail.prev
    tail.prev.next = node_to_add
    tail.prev = node_to_add

def remove_from_end():
    if head.next == tail:
        return

    node_to_remove = tail.prev
    node_to_remove.prev.next = tail
    tail.prev = node_to_remove.prev

def add_to_start(node_to_add):
    node_to_add.prev = head
    node_to_add.next = head.next
    head.next.prev = node_to_add
    head.next = node_to_add

def remove_from_start():
    if head.next == tail:
        return
    
    node_to_remove = head.next
    node_to_remove.next.prev = head
    head.next = node_to_remove.next

head = ListNode(None)
tail = ListNode(None)
head.next = tail
tail.prev = head
```

## Dummy pointers

As mentioned earlier, we usually want to keep a reference to the `head` to ensure we can always access any element. Sometimes, it's better to traverse using a "dummy" pointer and to keep `head` at the head.

```python
def get_sum(head):
    ans = 0
    dummy = head
    while dummy:
        ans += dummy.val
        dummy = dummy.next
    
    # same as before, but we still have a pointer at the head
    return ans
```

Using the `dummy` pointer allows us to traverse the linked list without losing a reference to the `head`.

As an exercise, implement a doubly linked list `1 <-> 2 <-> 3`.

```python
class ListNode:
    def __init__(self, val):
        self.val = val
        self.next = None
        self.prev = None


# Create nodes
node1 = ListNode(1)
node2 = ListNode(2)
node3 = ListNode(3)

# Link them: 1 <-> 2 <-> 3
node1.next = node2

node2.prev = node1
node2.next = node3

node3.prev = node2

# Test forward
curr = node1
while curr:
    print(curr.val, end=" ")
    curr = curr.next

print()

# Test backward
curr = node3
while curr:
    print(curr.val, end=" ")
    curr = curr.prev
```
Output:

```
1 2 3
3 2 1
```

## Fast and slow pointers

Fast and slow pointers is an implementation of the two pointers technique that we learned in the arrays and strings chapter. The idea is to have two pointers that don't move side by side. This could mean they move at different "speeds" during iteration, begin iteration from different locations, or any other abstract difference.

When the pointers move at different speeds, usually the "fast" pointer moves two nodes per iteration, whereas the "slow" pointer moves one node per iteration (although this is not always the case). Here's some pseudocode:

```bash
// head is the head node of a linked list
function fn(head):
    slow = head
    fast = head

    while fast and fast.next:
        Do something here
        slow = slow.next
        fast = fast.next.next
```
> The reason we need the while condition to also check for `fast.next` is because if `fast` is at the final node, then `fast.next` is null, and trying to access `fast.next.next` would result in an error (you would be doing `null.next`).

Let's look at some examples where fast and slow pointers can come in handy.

---

> Example 1: Given the head of a linked list with an **odd** number of nodes `head`, return the value of the node in the middle.
>
> For example, given a linked list that represents `1 -> 2 -> 3 -> 4 -> 5`, return `3`.

As mentioned in the previous article, the easiest way to solve this problem would be to just convert the linked list into an array by iterating over it, and then just returning the number in the middle.

```text
function fn(head):
    array = int[]
    while head:
        array.push(head.val)
        head = head.next

    return array[array.length // 2]
```

This is basically “cheating”, and would never pass as an acceptable solution in an interview. You may have realized that the difficulty in this problem comes from the fact that we don’t know how long the linked list is. One thing we could do is iterate through the linked list once with a dummy pointer to find the length, then iterate from the head again once we know the length to find the middle.

The most elegant solution comes from using the fast and slow pointer technique. If we have one pointer moving twice as fast as the other, then by the time it reaches the end, the slow pointer will be halfway through since it is moving at half the speed.

```text
def get_middle(head):
    slow = head
    fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    
    return slow.val
```

In Python:

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def middleNode(self, head: Optional[ListNode]) -> Optional[ListNode]:
        array = []
        while head:
            array.append(head)
            head = head.next

        return array[len(array) // 2]
```

The pointers use *O(1)* space, and if there are *n* nodes in the linked list, the time complexity is *O(n)* for the traversals.

---

> Example 2: [141. Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)
>
> Given the `head` of a linked list, determine if the linked list has a cycle.
>
> There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer.


If a linked list has a cycle, you can imagine some group of nodes forming a circle, and traversal never ends as it moves around that circle infinitely. One way to try to solve this problem would be to just iterate through the list for an arbitrarily large amount of iterations. If there isn't a cycle, then we will eventually reach the end of the list. If there is a cycle, then we will never reach an end and after a huge amount of iterations, we can conclude that there is probably a cycle.

The problem with this approach is that it isn't an actual general solution. What if there is no cycle, but there just happens to be more nodes than the iteration cutoff? If we increase the iteration cutoff, we can always argue that we could pass in a longer linked list. If we make the cutoff too large, it becomes impractical, and we are hard coding which is a terrible practice.

The better approach is to use a fast and slow pointer. Imagine a straight racetrack (like the one used in the 100m sprint). If two runners of significantly different speeds are racing, then the slower one will never catch up to the faster one. The faster runner finishing the race is like the fast pointer reaching the end of the linked list.

But what if the runners were instead running around a circular racetrack, and needed to complete many laps? In that case, the faster runner will eventually pass (lap) the slower runner.

We can apply this logic - move a fast pointer twice the speed of a slow pointer. If they ever meet (except at the start), then we know there must be a cycle. If the fast pointer reaches the end of the linked list, then there isn't a cycle.

> Why will the pointers always meet, and the fast pointer won't just "skip" over the slow pointer in the cycle? After looping around the cycle for the first time, if the fast pointer is one position behind, then the pointers will meet on the next iteration. If the fast pointer is two positions behind, then it will be one position behind on the next iteration. This pattern continues - after looping around once, the fast pointer moves exactly one step closer to the slow pointer at each iteration, so it's impossible for it to "skip" over.
>

```python
class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        slow = head
        fast = head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if slow == fast:
                return True

        return False
```

This approach gives us a time complexity of *O(n)* and a space complexity of *O(1)*, where `n` is the number of nodes in the linked list. Note that this problem can also be solved using hashing, although it would require *O(n)* space.

The hashing solution: if you continuously iterate using the `next` pointer, there are two possibilities:

1. If the linked list doesn't have a cycle, you will eventually reach `null` and finish.
2. If the linked list has a cycle, you will eventually visit a node twice. We can use a set to detect this.

```python
class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        seen = set()
        while head:
            if head in seen:
                return True
            seen.add(head)
            head = head.next
        return False
```

This solution is added for the sake of completeness - the first one is better as it uses less space.


> Example 3: Given the head of a linked list and an integer k, return the kth node from the end.
> For example, given the linked list that represents `1 -> 2 -> 3 -> 4 -> 5` and `k = 2`, return the node with value `4`, as it is the 2nd node from the end.

This problem is very similar to the first example. Again, we could just convert the list to an array, or we could iterate through once to find the length and then iterate again once we know the length, but there is a more elegant solution.

If we separate the two pointers by a gap of `k`, and then move them at the same speed, they will always be `k` apart. When the fast pointer (the one further ahead) reaches the end, then the slow pointer must be at the desired node, since it is `k` nodes behind.

```python
def find_node(head, k):
    slow = head
    fast = head
    for _ in range(k):
        fast = fast.next
    
    while fast:
        slow = slow.next
        fast = fast.next
    
    return slow
```

For the same reasons as in the first example, the time complexity of this algorithm is 
*O(n)* and the space complexity is *O(1)*, where `n` is the number of nodes in the linked list.

