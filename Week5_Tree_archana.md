**Tree Problems**
1. Write an algorithm to find the next node (i.e in-order successor) of a given node in a binary search tree. You may assume that each node has a link to its parent 
https://youtu.be/2hhEHZGoYlk

2. Design an algorithm and write code to find the first common ancestor of two nodes in a binary tree. Avoid storing additional nodes in a data structure. NOTE: This is not necessarily a BST 
https://youtu.be/DCV8A9aVOPw

3. You have 2 very large binary trees: T1 with millions of nodes, and T2 with hundreds of nodes. Create an algorithm to decide if T2 is a subtree of T1. A tree T2 is a subtree of T1 if there exists a node n in T1 such that the subtree of n is identical to T2. That is, if you cut off the tree at node n, the two trees would be identical. 
https://youtu.be/O4000kPAaLA

4. You are given a binary tree in which each node contains a value. Design an algorithm to print all paths which sum to a given value. Note that a path can start or end anywhere in the tree. 
https://youtu.be/iLfkTekQ1fw

5. Given a BST, create a linkedlist of all the nodes at each depth 
https://youtu.be/zmyC8VwRTw8

6. Convert a BST into a doubly linkedlist.
https://youtu.be/-S5NsYMGW3Q

Note: I made a mistake while writing the recursive call in whiteboard the method call in the second method is helper().
```java
void helper(Node root){
        if(root != null){
            helper(root.left); //convertBSTToLL(root.left)
            if(head == null){
                head = root;
                tail = root;
            }else{
                tail.right = root;
                root.left = tail;
                tail = root;
            }
            helper(root.right);
        }
 ```

7. Determine if a binary tree is balanced 
https://youtu.be/zuBnnbvydLU

8. Given a sorted array, create a binary search tree with minimal height 
https://youtu.be/G7UrLh9Ggzw

9. Implement a function to check if a binary tree is a BST.
https://youtu.be/XSQDJeI722U
