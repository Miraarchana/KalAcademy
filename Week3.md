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

