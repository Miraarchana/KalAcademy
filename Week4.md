**Linked List:**
- No indexing
- Dynamically growing O(1) operation.

*1. Write an algorithm to determine if a linkedlist is a palindrome *
Approach1: Two pointer (slow and fast) 
- Find the mid node
- reverse the second half
- compare the reversed list with first half.


Approach2: Using call Stack O(N) time, O(N) if call stack is considered.
- Store left and right as head.
- recurse till right is null.
- unwind by returning head of list
- start comparing right with left returned by recent call.
- if left returned reaches null, all characters match in order
- else the list is not palindromic
```java
  ListNode left;
	boolean isPalindromeRecursive(ListNode head) {
		left = head;
		ListNode right = head;
		if(head== null)
			return true;
		if(isPalinUtil(right)!=null)
			return false;
		return true;
		
	}
  
	ListNode isPalinUtil(ListNode right) {
		if(right == null)
			return left;
		ListNode left2 = isPalinUtil(right.next);
		if(left2!=null) {
			if(right.value == left2.value) {
				left2 = left2.next;
				return left2;
			}
		}
		return left2;
	}
  ```
*2. Write an algorithm to determine if a linkedlist is circular. FOLLOW UP: Determine where the circle meets. *


