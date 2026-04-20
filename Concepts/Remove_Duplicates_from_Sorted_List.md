# Remove Duplicates from Sorted List

[Remove Duplicates from Sorted List](https://leetcode.com/problems/remove-duplicates-from-sorted-list/)



```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next

class Solution:
    def deleteDuplicates(self, head: Optional[ListNode]) -> Optional[ListNode]:
        current = head
        
        while current and current.next:
            if current.next.val == current.val:
                current.next = current.next.next
            else:
                current = current.next
                
        return head
```


prev = null
curr = head

nextNode = curr.next (temporary variable, to point to the next node before changing any of the other pointers)
curr.next = prev (switch the direction of the arrow)
prev = curr
curr = nextNode

nextNode = curr.next
curr.next = prev (switch the direction of the arrow)
prev = curr
curr = nextNode

nextNode = curr.next
curr.next = prev (switch the direction of the arrow)
prev = curr
curr = nextNode

nextNode = curr.next (null)
curr.next = prev (switch the direction of the arrow)
prev = curr
curr = nextNode (null)


Failed to Schedule Interview Sorry, something went wrong
