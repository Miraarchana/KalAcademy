**Linked List:**
- No indexing
- Dynamically growing O(1) operation.

*1. Write an algorithm to determine if a linkedlist is a palindrome*

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
*2. Write an algorithm to determine if a linkedlist is circular. FOLLOW UP: Determine where the circle meets.*

```
   1-->2-->3-->4-->5-----
           |		|
 	   <-------6<----
```
Method1: Fast and slow pointer
1. Detect whether there is a loop in linked list
2. fix the fast pointer to the point where it meets slow pointer, set slow pointer back to the head.
3. move both pointer a node ahead at the same time, the node where they meet is the starting node of the loop.
```
df is the distance travelled by Fast pointer
ds is the distance travelled by slow pointer
m - distance from start to loop start(1->2->3 =3)
n-  distance from loop start to the point where fast n slow meets(4->5 =2)
p - loop count travelled by slow (0)
q - loop count travelled by fast (1)
l - number of nodes in loop.(3->4->5->6->3 = 5)
df = 2(ds)
m+n+q*l = 2(m+n+p.l); q>p
m+n =(q-2p)*l
m = (q-2p)*l-n -> (1-0)*5 -2 = 3
```
```java
int detectLoop(LinkNode nd){
		//check if a loop exist
		LinkNode slow = nd;
		LinkNode fast = nd;
		while(fast!=null) {
			slow = slow.next;
			if(fast != null)
				fast = fast.next.next;
			else
				break;
			if(slow == fast)
				break;
		}
		if (fast == null)
			return 0;
		else {
			//detect the meeting point
			slow = nd;
			while(slow != fast) {
				slow = slow.next;
				fast = fast.next;
			}
		}
		return slow.value;
	}
```
*Time complexity: O(N) where N is the number of nodes visited. --doubt*

*3. Clone a linked list with a random pointer.*

Method1: O(N) time and O(N) space
2- pass algorithm
- Constructing a map for current node and newly created clone node
- Constant access to map to get the curr node clone and its pointers.
```java
LinkNode cloneLL(LinkNode nd1) {
		LinkNode head = nd1;
		LinkNode cloneHead = null;//inorder to return constructed clone
		LinkNode curr = head;
		//clone mapping - O(n) time and O(n) space
		Hashtable<LinkNode,LinkNode> ht = new Hashtable<LinkNode, LinkNode>();
		while(curr!=null) {
			LinkNode newNode = new LinkNode(curr.value);
			if(curr == head)
				cloneHead = newNode;//inorder to return constructed clone
			ht.put(curr,newNode );
			curr = curr.next;
		}
		//construct clone -O(1) access time
		curr = head;
		while(curr!=null) {
			LinkNode cloneCurr = ht.get(curr);
			if(curr.next!=null)
				cloneCurr.next =ht.get(curr.next);
			if(curr.rand!=null)
				cloneCurr.rand = ht.get(curr.rand);
			curr = curr.next;
		}
		
		return cloneHead;
	}
```
Method 2: O(1) space
3 pass method

```java
LinkNode cloneWithConstantSpace(LinkNode head){
		//first pass - map clone to original list
		LinkNode curr = head;
		while(curr!= null) {
			LinkNode temp = curr.next;
			LinkNode clone = new LinkNode(curr.value);
			curr.next =clone ;
			clone.next = temp;
			curr = curr.next.next;
		}
		//second pass- map rand pointers.
		curr = head;
		while(curr!= null) {
			if(curr.rand!=null)
				curr.next.rand = curr.rand.next;
			curr = curr.next.next;
		}
		//third pass - restore the original list
		LinkNode dummyHead = new LinkNode(0);
		LinkNode cloneTail = dummyHead;
		curr = head;
		while(curr!= null) {
			LinkNode copy = curr.next; //this is to retain pointer to clone
			cloneTail.next = copy;//append clone to tail of clone list
			cloneTail = copy;//move clone tail to newly appended node
			if(curr.next!= null) {
				curr.next = curr.next.next;
			}	
			curr= curr.next;
		}
		return dummyHead.next;
	}
```


