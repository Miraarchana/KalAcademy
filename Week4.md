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

*4. Write code to remove duplicates from an unsorted linked list. Follow up: How would you solve it if temporary buffer is not allowed?*
Method1: with additional memory 
- Create a HashTable and keep adding elements to it. Prev and current pointer is tracked
- If element is already present in the ht, current is removed
-O(N) time and O(N) space

Method2 : Without additional memory
- Use four pointers
```
11-->12-->11-->14-->10-->12
^     ^
hd  curr
runner
prev
```
- Till runner is equal to curr, check for duplicates.
- If dup is found remove the curr node from list by assigning curr.nxt to prev.next
- else move prev to prev.nxt and curr to curr.nxt
- O(N^2) time complexity at worst case.

```java
LinkNode removeDupNoBuffer(LinkNode head) {
		if(head == null)
			return null;
		LinkNode prev = head; //to hold the previous reference to which the next element after remoing dup to be added
		LinkNode curr = head.next;//element for which, we have to run from head to check if it is a dup.
		while(curr!= null) {
			LinkNode runner = head;		 // reference to run from start of list till current check if curr is a dup of already existing value
			while(!(runner == curr)) {
				if(runner.value == curr.value) {
					curr = curr.next;
					prev.next = curr;
					break;
				}
				runner = runner.next;
			}
			if(runner == curr) {
				prev = prev.next;
				curr = curr.next;
			}
		}
		return head;
	}
```
*5.Implement an algorithm to find the kth to the last element of a singly linked list*

```java
/*
	 * Use two pointers.
	 * Move fast pointer k steps away from head
	 * Move fast and slow one step at a time. when fast reaches null, exit loop
	 * return slow pointer value as the kth element to end.
	 * O(n) time complexity with constant space O(1)
	 */
	private static int findKthIterative(LinkNode nd1, int i) {
		LinkNode slow = nd1;
		LinkNode fast = nd1;
		while(i > 0) {
			fast = fast.next;
			i--;
		}
		while(fast !=null) {
			fast = fast.next;
			slow = slow.next;
		}
		return slow.value;
	}
	
	/*
	 * recursively call each node with a counter incremented from bottom up.
	 * if the element's order from end is equal to i, 
	 * the element is printed as kth element from end.
	 * O(n) time. O(N) space if call stack is considered.
	 */
	private static int findKthRecursive(LinkNode nd1, int i) {
		if(nd1 == null)
			return 0; //base condition to windup recursive call
		int k = findKthRecursive(nd1.next, i)+1;
		if(i == k ) {
			System.out.println("kth element" + nd1.value);	
		}
		return 0;
	}
```

*6.Implement an algorithm to delete a node a singly linked list, given only access to that node.*
-algorithm
- copy the next element value to the node to be deleted
```java
	void deleteNode(LinkNode nd) {
		LinkNode curr = nd.next;v
		if(curr!= null) {
			nd.value = curr.value;
			nd.next = curr.next;
		}else {
			//if the node is the end of the list
			//assign a dummy node to the list
			nd.value = 0;
			nd.next = null;
		}
	}
```

*7.Write code to partition a linkedlist around a value x, such that all nodes less than x come before all nodes great than or equal to x.*
```java
LinkNode partitionList(LinkNode head, int x) {
		if(head==null) return null;
		LinkNode curr = head; //to track the current element in list
		LinkNode smH = new LinkNode(0);//small head is a dummy node
		LinkNode smT = smH;//small tail is also set to dummy node
		LinkNode lgH = new LinkNode(0);
		LinkNode lgT = lgH;
		LinkNode eqH= new LinkNode(0);
		LinkNode eqT = eqH;
		
		while(curr!= null) {
			if(curr.value == x) {
				//append to eqT
				eqT.next = curr;
				curr=curr.next;
				eqT = eqT.next;
			}
			else if(curr.value >x) {
				//append to lgT
				lgT.next = curr;
				curr=curr.next;
				lgT = lgT.next;
			}else {
				//append to smT
				smT.next = curr;
				curr=curr.next;
				smT = smT.next;
			}
		}
		
		//append equal list to small and large list to equal tail
		smT.next= eqH.next;
		eqT.next = lgH.next;
		return smH.next;
	}
```
*8.*
```java

	/*
	 * 3 pointer approach
	 * curr, prev, next
	 * -Assign curr.next to nxt pointer and change curr.nxt to point prev node
	 * - curr node is made prev node and curr node is my next node
	 * p,n	 c
	 * null->10->11->3->4->5->null
	 *       p  c   n
	 * null<-10 11->3->4->5->null
	 *           p  c  n
	 * null<-10<-11 3->4->5->null
	 *               p c  n
	 * null<-10<-11<-3 4->5->null
	 * 		 p c	 n
	 * null<-10<-11<-3<-4 5->null
	 *                     p c
	 * null<-10<-11<-3<-4<-5
	 * return p
	 */
	static LinkNode reverseIterative(LinkNode hd) {
		LinkNode prev = null;
		LinkNode nxt = null;
		LinkNode curr = hd;
		while(curr!=null) {
			nxt =curr.next;
			curr.next = prev;
			prev = curr;
			curr = nxt;
		}
		return prev;
	}
	
	static LinkNode recurHead;
	static LinkNode reverseRecursive(LinkNode curr, LinkNode prev) {
		if(curr.next == null){
			//we have reached the end of the list
			recurHead = curr;//tail of the list is the head of recurse list
			curr.next = prev;//reverse the link between tail and prev element
			return recurHead;
		}
		LinkNode nxt = curr.next;//get hold of next element in list before removing the pointer between curr and curr.next
		curr.next = prev;//reverse the link between curr and prev element
		reverseRecursive(nxt, curr);//call reverse of new curr(nxt) and new prev(curr) pointers.
		return recurHead;
	}
```

*9. Given a singly linked list which has data sorted in ascending order, construct a balanced BST.*

Psuedocode:
1->2->3->4->5

find number of nodes
initialize a root node to null
initialize a currRoot to null
initialize an int value to 1
curr = head

while(curr!=null){
Node currNd= new Node(curr.val);
if(currRoot == null)
	currRoot = currNd;
else if(i == mid)
	currNd.left = currRoot;
	root = currNd;
else
     if(currNd.val>currRoot.val)
	currRoot.right = currNd;
	currRoot = currNd;
     else
	currNd.left = currRoot;
	currRoot=currNd;
curr = curr.next;
i++;
}
return root;
}


