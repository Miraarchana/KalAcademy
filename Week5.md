**Tree**

*1. Inorder successor with Parent access*

![Image](https://github.com/Miraarchana/KalAcademy/blob/master/BST-InorderSuccessorParentAccess.png)

*Time complexity: O(H) where H is the height of the tree. Average case O(log N) , worst case  O(N) - N is number of nodes in tree*

*2. First Common Ancestor or Lowest Common Ancestor*
![Image](https://github.com/Miraarchana/KalAcademy/blob/master/LowestCommonAncestorBT.png)

*3. Subtree of Another Tree*

*Approach 1:*
Uses Recursive approach.
1. Check if the root node of T1 is identical to root node of T2 [or] else traverse to left subtree in T1 and check for identical to T2
[or] else traverse to right subtree in T1 and check for identical to T2.
2. Checking if identical runs through the entire T2 and T1 from the node that matches T2 root.

![Image](https://github.com/Miraarchana/KalAcademy/blob/master/SubtreeOfAnotherTree.jpg)


Time complexity : O(m*n) worst case. m - number of nodes in T2. n - number of nodes in T1.
Space complexity : o(n) for n recursive calls.

*4.Print all Path with Sum.*

Hint: Find contiguous subsequence in array that sums up to a given number.

```
<-----------RunningSumY------>
<RunningSumX-><--targetsum--->
|------------|---------------|
s            x               y
```
![Image](https://github.com/Miraarchana/KalAcademy/blob/master/PathWithSumsPrint_1.png)
  
