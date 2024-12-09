
```java
package com.example.kmp;

import java.util.Arrays;
/** 
* @author ZhuJW 
* @classname KMPAlgorithm 
* @description KMP算法 
* @date 2022/6/22 21:52 
*/
public class KMPAlgorithm {  
 public static void main(String[] args) {    
  String str1 = "BBC ABCDA ABCDABCDABDE";    
  String str2 = "ABCDABD";    
  int[] kmpNext = kmpNext(str2);
  System.out.println(Arrays.toString(kmpNext));
  System.out.println(kmpSearch(str1, str2, kmpNext));  
 }  
 
 /**   
 *   
 * @param str1 源字符串   
 * @param str2 子串   
 * @param next 子串的部分匹配表   
 * @return 第一个出现的位置或者为-1   
 */  
 public static int kmpSearch(String str1, String str2, int[] next) {    
  for (int i = 0, j = 0; i < str1.length(); i++) {      
   // 如果不相等，就需要将str2往后移动，即str2的索引j往前移动，直到相等或者j为0    
   while (j > 0 && str1.charAt(i) != str2.charAt(j)) {        
    // 此时移动到next数组的前一个位置        
    j = next[j - 1];      
   }      
   // 判断是否相等，如果相等，就将str2的索引j++      
   if (str1.charAt(i) == str2.charAt(j)) {        
    j++;      
   }      
   // 如果j==str2.length()，就表示str2字符串已经匹配完成，可以直接返回数据     
   if (j == str2.length()) {        
    // 因为前面j++过，而此时的i还为当前循环的索引，没有增加，所以应该返回i - j + 1  
    return i - j + 1;      
   }    
  }    
  return -1;  
 }  
 /**   
 * 获取传入字符串的部分匹配值表   
 * @param text 字符串   
 * @return 部分匹配值表   
 */  
 public static int[] kmpNext(String text) {    
  int[] next = new int[text.length()];    
  // 一个数值是没有前后缀匹配值的    
  next[0] = 0;    
  for (int i = 1, j = 0; i < text.length(); i++) {      
   // 走前后索引，将当前循环的字符串与前索引的字符串对比，      
   while (j > 0 && text.charAt(i) != text.charAt(j)) {        
    // 如果不相等的时候，将前索引j赋值为next数组的j-1的值相当于前索引j - 1一样，直到当前字符与前索引的字符相等或者前索引的值为0即字符为第一个字符时退出        
    j--;      
   }      
   // 走前后索引，将当前循环的字符串与前索引的字符串对比，如果相同，前索引就++，然后进行下一个索引的对比      
   if (text.charAt(i) == text.charAt(j)) {        
    j++;      
   }      
   // 并将当前循环的索引作为下标，前索引作为值存入到部分匹配值表的数值中      
   next[i] = j;    
  }    
  return next;  
 }
}
```
