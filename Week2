Week 2 - Stacks and Queues
Queue : FIFO
Stack : LIFO
 - Both can be implemented using array and linked list
 - No indexing, we can access only on top and bottom elements for queue and only top for stack

Check for implementing Queue using array in referencesource.microsoft.com for C#

Week 2 Assessments:
1. Find the missing paranthesis in the given expression 2*(3+5(sabcd)

Expression string can be converted to a character array and then look for the missing paranthesis
Approach1: Counter approach o(n) time
 - check every character in the string for left paranthesis or right paranthesis
   - if the char is '(' - increment openCounter
   - else if the  char is ')' - increment closeCounter
   - else if it is a number or variable continue checking the next char
 - if value of openCounter == closeCounter, there is no missing paranthesis
 - if value of openCounter > closeCounter, there is (openCounter-closeCounter) closeCounters missing
 - if value of openCounter < closeCounter, there is (closeCOunter-openCounter) openCounters missing
limitation: if the expression contains more than one set of paranthesis like [] or{} we cannot use this approach

Approach2: Using Stack - O(n) time , o(n) space
-declare a Stack<Charachter>
-iterate through the character in the expression
  - if char is '(' or '[' or '{' - push it to the stack
  -Else if the char is ')' or ']' or'}' pop the element from stack and check if the paranthesis matches(corresponding open paranthesis) the char.
   -if matches, continue
   -else print the missing paranthesis.
- If there are still paranthesis left in stack, pop and print its close paranthesis as missing

    void findMissing(String expression){
         Stack<Character> st = new Stack<>();
         Char[] ch  = expression.toCharArray();
         for(char c: ch){
           if(c=='(' || c=='[' || c=='{')
             st.push(c);
           else if(c==')' || c==']' || c=='}'){
             if(!st.isEmpty() && isMatching(st.peek(),c))
                st.pop();
             else
                print("missing paranthesis" + getMissing(c));
           }
           while(!st.isEmpty()){
               print("missing paranthesis"+getMissing(st.pop());
           }
         }
     }
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
2. Evaluate an expression given only single digits and only 2 operators * and +
Assumption: Expression is Infix notation as Reverse Polish Notation will be specified.
Approach: ShuntingYard algorithm
https://en.wikipedia.org/wiki/Shunting-yard_algorithm
- The processing time of infix notation is slower compared to Postfix and Prefix(because of additional processing for paranthesis).
- Shunting Yard algorithm converts a infix notation to postfix and maintains a stack for operator.
- This approach 2 stacks are used one for operator and anothe one for operand as we need to evaluate the expression on run.
Algorithm:
 - Tokenize the expression
 - Iterate through the tokens
   - if the token is
     -number
       -push it to operand stack
     -left paranthesis
       -push it to operator stack
     -right paranthesis
       -while top element of the stack is not '(' evaluate all expression inside the parantheses
         -pop operator from opStack
         -pop value from operand stack twice
         -apply the operation and push result to operand stack
       - pop '(' from opStack and discard
     -operator
        -while the opStack is not empty & top operator in stack has greater precedence thant the current token operator
          -pop operator from opStack
         -pop value from operand stack twice
         -apply the operation and push result to operand stack
        - push the current operator to opStack
  - while opStack is not empty
    -pop operator from opStack
    -pop value from operand stack twice
    -apply the operation and push result to operand stack
  - If opStack is empty
    - print the top element in operand stack as result
    
evaluate(String expression) -o(n)
    void evaluate(String expression){
      char[] token = expression.toCharArray();
      Stack<Character> opStack = new Stack<>();
      Stack<Integer> operands = new Stack<>();
      for(int i=0; i<token.length;i++){
        if(token[i]<='0' && token[i] <='9)
          operands.push(Integer.parseInt(token[i]);
        else if (token[i] =='(')
          opStack.push(token[i]);
        else if(token[i] ==')')
        {
          while(opStack.peek()!='(')
            operands.push(applyOp(operands.pop(),operands.pop(),opStack.pop()));
          opStack.pop();
        }
        else if(token[i]=='*' || token[i]=='+')
        {
          while(!opStack.isEmpty() && hasPrecedence(opStack.peek(),token[i])
            operands.push(applyOp(operands.pop(),operands.pop(),opStack.pop()));
          opStack.push(token[i]);
        }
      }
      while(!opStacj.isEmpty()){
          operands.push(applyOp(operands.pop(),operands.pop(),opStack.pop()));
      }
      return operands.pop();
    }
boolean hasPrecedence(op1,op2) -o(1)
    boolean hasPrecedence(op1,op2){
      if(op1 == '*' && op2=='+')
       return true;
      return false;
    }
int applyOp(opr1,opr2,op) -o(1)
    int applyOp(int x, int y, char op){
       if(op =='*')
         return x*y;
       else if(op =='+')
         return x+y;
     return -1;
    }
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
3. Reverse a stack and put the reversed value back in the same stack. You can use only one
other stack and a temp variable.
Approach1: o(n^2) time with o(n) space
   traverse to bottom of stack and start reversing from end.
   - have count as variable assigned to size of the input stack1.
   - create stack2
   - while count is not equal to 0 (count is used to track number of pop to be done on st1 for next cycle)
     -pop the top element in stack1 to a temp variable
     -while stack1 is not empty and count is greater than 0
      -push the next top element in stack1 to stack2 and decrement the count
     - push the temp back to the stack that is empty now and set count to st2.size()-1
     -push back elements from st2 to st1.
    -return st1.
 Approach2: reverse the stack using another stack and swap values - O(n) time
     static Stack<Integer> reverseStack(Stack<Integer> st1){
      Stack<Integer> st2 = new Stack<Integer>();
      while(!st1.isEmpty()) {
       st2.push(st1.pop());
      }
      System.out.println(st2.peek());
      return st2;
     }
     
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


4. Implement Queue using Stacks

- for implementing Queue, enqueue must add element to the top of Stack and dequeue must remove element from bottom of stack.

Approach1: Using 2 Stacks (Enqueue does the work of maintaing the LIFO pointers - expensive enqueue)
enqueue:
  - If stack1 is empty- push element to stack1
  - If stack1 is not empty
    - copy all elements from stack1 to stack2
    - push new element to stack1
    - push all elements back from stack2 to stack1
dequeue:
  - If stack1 is empty - throw underflow error
  - else pop element on top from stack1 and return
Time complexity : Enqueue operation takes O(n) + O (n) as it moves all the element twice. dequeue is o(1)
     public class UserQueue{
         Stack<Integer> stack1 = new Stack<>();
         Stack<Integer> stack2 = new Stack<>();
         public void enqueue(int x){
             if(stack1.isEmpty())
                stack1.push(x);
             else{
                while(!stack1.isEmpty()){
                  stack2.push(stack1.pop());
                }
                stack1.push(x);
                while(!stack2.isEmpty())
                  stack1.push(stack2.pop());
             }
         }
         
         public int dequeue(){
            if(stack1.isEmpty()) 
              print("Stack underflow");
              return -1;
            else
              return stack1.pop();
         }
     }
    
Approach2: Using 2 Stacks (Dequeue does the work of maitaining the LIFO pointers)
enqueue: 
 -push element to stack1
dequeue:
- If stack1 is empty && stack2 is empty - throw under flow error
- If stack1 is empty && stack2 is not empty - pop top element in stack2 and return
- else if stack1 is not empty && stack2 is empty 
  - push all elements from stack1 to stack2
  - pop the top element in stack2 and return it
Time complexity : Enqueue operation takes O(1). dequeue is o(1) and on worst cases O(n) where all elements are copied to stack2 from stack1.
    
     public class UserQueue{
         Stack<Integer> stack1 = new Stack<>();
         Stack<Integer> stack2 = new Stack<>();
         public void enqueue(int x){
             stack1.push(x);
         }
         
         public int dequeue(){ //stack2 acts as reverse stack for stack1, stack2 always holds top element for pop operation
            if(stack1.isEmpty() && stack2.isEmpty()) 
              print("Stack underflow");
              return -1;
            else if(stack1.isEmpty()){
              return stack2.pop();
            }
            else if(stack2.isEmpty()){
              while(stack1.isEmpty())
              {
                 stack2.push(stack1.pop());
              }
              return stack2.pop();
            }
         }
     }
Approach3: Using a user Stack and function call stack
enqueue:
push the element to stack1 -O(1)
Dequeue
Using function call stack we can store the current top element of the function call and recursively call the dequeue operation till we hit element at the bottom of stack and pop that element and return. Push the current call's top element back into stack1. - O(n) ( as we pop and push n-1 elements)
    class UserQueue<Integer>{
       private Stack<Integer> stack = new Stack<>();
       public void enqueue(int x){
          stack.push(x);
       }
       public int dequeue(UserQueue q){
          int top, y= 0;
          if(q.stack.isEmpty()) {
              print("underflow"); 
              return -1;
          }
          else if(q.stack.size() ==1){
              return q.stack.pop();
          }
          else{
              y=q.stack.pop();
              top = dequeue(q);
              q.stack.push(y);
              return top;
          }
          return 0; 
        }
      }
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
5. Implement Stack using Queues:
Ask question : what are the elements in stack? Integer
               whether we need to implement Queue(we can use a LinkedList) or use Java datastructure queue (default is array)
approach : we need to reverse queue inorder implement LIFO. (but this makes push element difficult)
           we can have two queue.
           
        app1: push is expensive
             - push(x) - O(n) time
               - add to q2 - this is the new top
               - if q1 is not empty - pop all elements from q1 to q2 and swap q1 with q2 (q1 has all elements and q2 is empty)
              
             - pop() - O(1) time
               - check if q1 is empty - throw underflow error
               - remove top element from q1
               
               public Stack<Integer>{
                  Queue<Integer> q1 = new Queue<>();
                  Queue<Integer> q2 = new Queue<>();
                  
                  push(Integer i){
                    q2.push(i);
                    while(!q1.isEmpty()){
                      q2.push(q1.pop());
                    }
                    Queue<Integer> temp = q1;
                    q1 = q2;
                    q2 = temp;
                  }
                  
                  int pop(){
                    if(q1.isEmpty()) throw "Underflow";
                    return q1.pop();
                  }
                 }
          
       app2: pop is expensive
           enqueue -O(1)
            - push the element to Q1
           dequeue - o(n) time
            - if q1 is empty = throw an error
            - if not empty - move all elements except the last element from q1 to q2
               - pop the element from q1
               - swap q1 and q2
               - return the element popped from q1.
               
                public Stack<Integer>{
                  Queue<Integer> q1 = new Queue<>();
                  Queue<Integer> q2 = new Queue<>();
                  
                  push(Integer i){
                    q1.push(x);
                  }
                  
                  int pop(){
                    if(q1.isEmpty()) throw "Underflow";
                    else{
                      while(q1.size()!=1){
                        q2.push(q1.pop());
                      }
                      int temp = q1.pop(); //pop the last element in q1 after moving rest of the element to q2
                      Queue<Integer> q = q1;
                      q1=q2;
                      q2=q;
                      return temp;
                     }
                  }
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
6. Next Greater Element(NGE) for every element in a given array.
   Hints:
    - NGE of rightmost element in array is -1
    - NGE of all the element in an array sorted in descending order is -1.
    
   Approach1: 2 loop - o(n^2) time
    - Iterate the array from i = 0 to n-1 elements
      - Iterate the array from j= i+1 to n-2 elements
          - if a[i]<a[j] - print NGE of A[i] is a[j] and break inner loop
          - else continue to increment the j
      - print NGE of a[i] is -1 and increment the i.
    print NGE of a[n-1] is -1.
    
    Approach2: Using Stack (addind index to stack) - o(n) time, o(n) space
    if the next value is not greater than current value we still search for next value that is greater than both previous and current value, using a stack to track the last and last before elements will be easy.
    - iterate through the array for i =0 to n-1
       - check until the value in top element index in stack is lesser than the current element in the array 
         - pop the top element index and print its NGE as current element and break the current
       - push the current elemen tindex to stack( that becomes the new top) and i is incremented
    - if there are any element's index left in stack (means that those elements does not have NGE) are printed as -1.
   
      int[]  findNGE(int[] nums){
        int[] result = new int[nums.length];
        Stack<Integer> stack = new Stack<>();
        for(int i=0; i<nums.length;i++){
           while(!stack.isEmpty() &&nums[stack.peek()] < nums[i])
            {
              result[stack.pop()] = nums[i];
              break;
            }
           stack.push(i);
        }
        while(!stack.isEmpty()){
           result[stack.pop()] =-1;
        }
      return result;
     }

