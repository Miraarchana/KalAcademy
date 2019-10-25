**String**
________________________________________________________________________________________________________________________________________
**1. Implement an algorithm to determine if a string has all unique characters. What if you
cannot use additional data structures?**

**Method Bitwise Shift:**
*Assumptions:*
1. String uses only lower case letters 'a' - 'z'.
*Algorithm:*
1. Convert the String to char array or directly access each character using charAt() method.
2. intialize a checker(32-bit) to 0. ( this is the reason for assuming the characters in string are 'a' to 'z' range only. )
3. Iterate through the char array.
  * calculate the bitAtindex ( the integer value of that character at i by  ch[i]-'a').
  * If the current character's bit value is already set in the checker, its value must be greater than 0 and that shows the string has duplicate character. 
    - This can be done using AND operator, the binary value of the integer at given index can be calculated using (1<<bitAtIndex) left shift 1 by that integer value.
  * else set that character's bit to checker by OR(|) operator with the binary value of the integer at bitAtIndex. Continue the loop.

*Code:*
```java
   boolean uniqueCharacters(String str) 
	  {
    int checker = 0; 
    for (int i = 0; i < str.length(); i++) { 
     int bitAtIndex = str.charAt(i) - 'a'; 
     if ((checker & (1 << bitAtIndex)) > 0) 
      return false; 

     checker = checker | (1 << bitAtIndex); 
    } 
    return true; 
   } 
```

**2. Given two strings, write a method to decide if one is a permutation of the other?**
*Clarifications & assumptions:*
1. If the strings are case-sensitive. YES
2. White-space counts as a character "she    " not same as "she"

*Method 1: Sort and check*
Permutation means, two strings has same set of character but in different order.
Strings must be of equal length.
So sorting both strings and checking whether their sorted strings are equal. O(nlogn)+O(nlogn).

*Method 2: Count the occurence of character in one string and check with other:*
Time complexity : O(n) 
Assumption:
	- size of the character set is assumed to be ASCII (128 characters).
```java
boolean checkPermutation(String str1, String str2) {
	if(str1.length() != str2.length())
		return false;
	char[] ch1 = str1.toCharArray();
	int[] indexes = new int[128];//ASCII charset 
	for(char c:ch1)
		indexes[c]++;//increment the value at index of that char
	char[] ch2 = str2.toCharArray();
	for(char c: ch2) {
		indexes[c]--;//decrement the value at index of that char
		if(indexes[c]<0)
			return false;
	}
	return true;
}
```
**3. Write a method to replace all spaces in a string with ‘%20’**
Hint:
```reference
A common approach in string manipulation problems is to edit the string starting from the end and working
backwards. This is useful because we have an extra buffer at the end, which allows us to change characters
without worrying about what we're overwriting.
```
Clarification:
The input string has enough space for characters to be added. 

Method1:
1. Find the no.of space in the string. ignore the spaces at the end.
2. calculate the new length string with extra-space for ('20' as '%' can fit in ' ' that already exist in the string)
3. Start filling the char array from end.
   * If char at index == ' ' , start replacing next 3 indexes with '0','2','%' and decrement index by 3
   * else replace the ```char[index-1]``` to ```char[i]``` and decrement index by 1
```java
	static void replaceSpace(String str, int truelen) {
		int spaceCnt = 0;
		int index = 0;
		char[] ch = str.toCharArray();
		for (int i = 0; i < truelen; i++) {//O(N) N is truelen
			if (ch[i] == ' ')
				spaceCnt++; // count the space
		}
		index = truelen + (spaceCnt * 2);// calculate the index for o/p
		// length is tripled by space count
		if (truelen < ch.length)
			ch[truelen] = '\0';
		for (int i = truelen - 1; i >= 0; i--) {//O(M) M is index
			if (ch[i] == ' ') {
				ch[index - 1] = '0';
				ch[index - 2] = '2';
				ch[index - 3] = '%';
				index = index - 3;
			} else {
				ch[index - 1] = ch[i];
				index--;
			}
		}
	}
```

**4.Implement a method to perform a basic string compression using the counts of
repeated characters. For example, the string aabccccaaa would become a2b1c4a3. If the
compressed string would not become smaller than the original string, your method
should return the original string**

```java
	static String compress(String str) {
		int currentConsecutive =0; //keeps track of consecutive same characters
		StringBuilder compress = new StringBuilder();//result 
			currentConsecutive++;
			if((i+1 >=str.length())|| str.charAt(i+1)!= str.charAt(i)) { //if next char is different
				compress.append(str.charAt(i));
				compress.append(currentConsecutive);
				currentConsecutive =0;
			}
		}
		return compress.length() < str.length() ? compress.toString():str;	
	}
```
Worst case: if compressed string is greater than given string, the compressed string is of no use.
Checking the length prior is another example, but the downside is additional loop to check the length and duplicate code.

**5. Write an algorithm such that if an element in an MxN matrix is 0, its entire row and
column are set to 0**

Algorithm:
1. Iterate through rows and column. 
2. Check whether the nums[i][j] ==0,
	* If true, set the corresponding row's first index to 0 and corres. column's first index to 0.(marks that this row and column has a 0, used for nullifying).
	* else continue.
3. Nullify rows that has first index 0.
4. Nullify columns that has first index 0.
5. return the modified matrix.

```java
	int[][] nullifyRowsColumns(int[][] nums){
		for(int i=0; i<nums.length;i++) { //scan through all elements O(M*N)time
			for(int j=0; j<nums[0].length;j++) {
				if(nums[i][j]==0) {
					nums[i][0]= 0;
					nums[0][j]= 0;
				}else if(nums[i][0]==0 || nums[0][j] == 0){
					nums[i][j] = 0;
				}
			}
		}
		//nullifying rows
		int z=0;
		for(int i=0;i<nums.length;i++) { 
			if(nums[i][0]==0){
				while(z<=nums[0].length-1)
					nums[i][z++] = 0;
			}
		}
		//nullifying columns
		int y=0;
		for(int j=0;j<nums[0].length;j++) {
			if(nums[0][j]==0){
				while(y<=nums.length-1)
					nums[y++][j] = 0;
			}
		}
		return nums;
	}
```


**7.Given a string S, find the longest palindromic substring in S.
Input: "babad"
Output: "bab"
Note: "aba" is also a valid answer.**

*Method 1: Brute Force*
To check each substring, outer two loops to fix the edges of substring and inner loop to check whether the string within that range is palindromic. O(n^3) time complexity approach.

*Method 2: Expand from center*
Using two loops, with outer loop determining the center points for a palindromic sequence and inner loop to expand and check from that center selected.
	- for selecting center for odd length string, expand from the character at i.
	- for checking even length string, expand between i and i+1 index.
```java
	String longestpalindromicSS(String str){//o(n)
		if(str == null|| str.length()<=0)
			return "";
		String longest = str.substring(0,1);
		for(int i=0; i<str.length();i++) {
			String temp1 = expand(str,i,i);//this is for odd length "abdba"
			String temp2 = expand(str, i, i+1);//this is to find mirror between even length palindromic sequence "abba"
			if(temp1.length() >longest.length())
				longest = temp1;
			if(temp2.length() > longest.length())
				longest = temp2;
		}
		return longest;
	}
	
	String expand(String str, int begin, int end) {//o(n)
		while(begin >=0 && end <str.length() && str.charAt(begin)== str.charAt(end)) {
			begin--;
			end++;
		}
		if(begin ==-1 && end >str.length()) {//if entire string is a palindromic.to avoid index out of bound exception.
			return str.substring(begin+1);
		}
		return str.substring(begin+1,end);
	}
```
This approach takes O(n^2) time complexity and O(1) space.

*Method3: Mancher's algorithm*
This is a linear time algorithm.

----------------------------------------------------------------------------------------------------------------------------------------*8.Given a string, recursively remove adjacent duplicate characters from string. The output
string should not have any adjacent duplicates. See following examples.
Input: azxxzy
Output: ay
First "azxxzy" is reduced to "azzy". The string "azzy" contains duplicates, so it is further
reduced to "ay".
Input: caaabbbaacdddd
Output: Empty String
Input: acaaabbbacdddd
Output: acac*

*Method1:using Stack*
- Keep pushing character till the top of the stack is different
- pop the top element if the current character is equal to stack top element and discard
- Pop all the elements and keep appending the result to the result string.

*Method2:Traverse from leftmost element with window size 2*
Algorithm:
1. If the string is empty returns ""
2. If the next index of the character to be checked for duplicate occurrence an index before has exceeded the length string, return thr string processed so far
3. If the next index is the lower bound of length, increment x
4. IF the string length is more than 1, check for the adjacent index for duplicate
	- If char at adjacent indices are equal and length of string is 2 return empty string.
	- else if adjacent indices are equal and  length of th string is greater than 2, partition the string by removing the adjacent indices and recur checking duplicate characters for remaining string
5. Else if string length is greater than 2 and adjacent indices elements are different recur checking duplicate characters for next index
6. Else return the string (as length equal to 1).

----------------------------------------------------------------------------------------------------------------------------------------

*11.Given two strings ‘X’ and ‘Y’, find the length of the longest common substring.*

```java
	//If the characters for i n j in current str1 n str2 is same, refer previous character's lcs length stored in i-1 j-1 indices and increment by 1.
	//else set lcs[i][j] to 0.
	//we store the length of all common substrings for all substrings of str1 and str2 and store its length in m+1*n+1 matrix.
	//this will be execute in time complexity O(m*n) m is len of str1, n is len str2
	
	static int findLCS(char[] s1, char[] s2, int m, int n) {
		int[][] lcs = new int[m+1][n+1];//cells hold the substring length, created with buffer row n column
		int result = 0;//track the max length
		StringBuffer substring = new StringBuffer();
		for(int i =0 ; i<=m ;i++) {
			for(int j=0; j<=n; j++) {
				if(i ==0 || j==0)
					lcs[i][j]=0;
				else if(s1[i-1] == s2[j-1])
				{
					lcs[i][j] = lcs[i-1][j-1]+1;
					if(result < lcs[i][j]) {
						substring.append(s1[i-1]);//to print the string
						
					}
					result=  Math.max(result, lcs[i][j]);
					
				}else
					lcs[i][j]=0;
			}
		}
		System.out.println(substring.toString());
		
		return result;
	}
```
----------------------------------------------------------------------------------------------------------------------------------------

