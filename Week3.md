*1. Implement an algorithm to determine if a string has all unique characters. What if you
cannot use additional data structures?*


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
