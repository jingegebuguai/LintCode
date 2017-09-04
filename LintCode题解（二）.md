title: LintCode题解（二）
author: 惠惠
tags:
  - 算法
categories:
  - 计算机
date: 2017-09-04 10:14:00
---
## 最长回文串
给出一个包含大小写字母的字符串。求出由这些字母构成的最长的回文串的长度是多少。

数据是大小写敏感的，也就是说，"Aa" 并不会被认为是一个回文串。

### 样例
给出 s = "abccccdd" 返回 7

一种可以构建出来的最长回文串方案是 "dccaccd"。

### 算法解答
这道算法考的是键值对关系，一个字符无法构成回文，但是当每个字符大于2时，就会产生回文对。这时也要同时考虑到相同字符个数的奇偶性是否影响回文串的长度。这里使用java的Map来解答，以字符作为键名，相应字符的数量作为键值。
	
	public class Solution {
    /*
     * @param s: a string which consists of lowercase or uppercase letters
     * @return: the length of the longest palindromes that can be built
     */
	    public int longestPalindrome(String s) {
	        // write your code here
	        int index = 0;
	        if(s.length()==0)
	            return 0;
	        String[] _s = s.split("");
	        HashMap<String,Integer>map = new HashMap<String,Integer>();
	        for(String str:_s){
	            Integer num = map.get(str);
	            map.put(str,num==null?1:num+1);
	        }
	        Iterator<String> iter = map.keySet().iterator();
	        while(iter.hasNext()){
	            String key = (String)iter.next();
	            if(map.get(key)>=2&&map.get(key)%2==0)
	                index = index+map.get(key);
	            else if(map.get(key)>=2&&map.get(key)%2!=0)
	                index = index+map.get(key)-1;
	        }
	        if((s.length()-index)>0)
	            return index+1;
	        else
	            return index;
	    }
	}

## Strings Homomorphism
Given two strings s and t, determine if they are isomorphic.

Two strings are isomorphic if the characters in s can be replaced to get t.

All occurrences of a character must be replaced with another character while preserving the order of characters. No two characters may map to the same character but a character may map to itself.

### 样例
Given s = "egg", t = "add", return true.

Given s = "foo", t = "bar", return false.

Given s = "paper", t = "title", return true.

### 算法题解
这题的目的是为了判断两个字符串是否结构相同，显然要用到键值对关系去判断

	public class Solution {  
    /** 
     * @param s a string 
     * @param t a string 
     * @return true if the characters in s can be replaced to get t or false 
     */  
	    public boolean isIsomorphic(String s, String t) {  
	        // Write your code here  
	        HashMap<Character , Character> record = new HashMap<>();  
	        HashSet<Character> repeat = new HashSet<>();  
	        for(int i = 0;i<s.length();i++){  
	            if(!record.containsKey(s.charAt(i))){  
	                if(repeat.contains(t.charAt(i))){  
	                    return false;  
	                }  
	                record.put(s.charAt(i) , t.charAt(i));  
	                repeat.add(t.charAt(i));  
	                
	            }else{  
	                if(record.get(s.charAt(i)) != t.charAt(i)){  
	                    return false;  
	                }  
	            }  
	        }  
	        return true;  
	    }  
	}  

## First Position Unique Character
Given a string, find the first non-repeating character in it and return it's index. If it doesn't exist, return -1.

### 样例
Given s = "lintcode", return 0.

Given s = "lovelintcode", return 2.

### 算法题解
找没有重复字符的索引，太简单，代码如下：

	public class Solution {
    /*
     * @param s: a string
     * @return: it's index
     */
	    public int firstUniqChar(String s) {
	        // write your code here
	        int index = -1;
	        Map<Character,Integer> map = new LinkedHashMap<Character,Integer>();
	        for(int i=0;i<s.length();i++){
	            Integer num = map.get(s.charAt(i));
	            map.put(s.charAt(i),num==null?1:num+1);
	        }
	        Iterator<Character> iter = map.keySet().iterator();
	        while(iter.hasNext()){
	            Character cha = iter.next();
	            if(map.get(cha)==1){
	                index = s.indexOf(cha);
	                break;
	            }
	        }
	        return index;
	    }
	}

## 两数组交集（2）
nums1 = [1, 2, 2, 1], nums2 = [2, 2], 返回 [2, 2].

	public class Solution {
    /**
     * @param nums1 an integer array
     * @param nums2 an integer array
     * @return an integer array
     */
	 public int[] intersection(int[] nums1, int[] nums2) {
	      List<Integer> list = new ArrayList<Integer>();
	        Arrays.sort(nums1);
	        Arrays.sort(nums2);
	        int i = 0, j = 0;
	        while (i < nums1.length && j < nums2.length) {
	            if(nums1[i] < nums2[j]){
	                i++;
	            }else if(nums1[i] == nums2[j]){
	                list.add(nums1[i]);
	                i++;
	                j++;
	            }else{
	                j++;
	            }
	        }
	        int[] inter = new int[list.size()];
	        for (i = 0; i < inter.length; i++) {
	            inter[i] = list.get(i);
	        }
	        return inter;
	    }
	}
	
## 两数组交集（1）
样例
nums1 = [1, 2, 2, 1], nums2 = [2, 2], 返回 [2].

	public class Solution {
    /**
     * @param nums1 an integer array
     * @param nums2 an integer array
     * @return an integer array
     */
	   public static int[] intersection(int[] nums1, int[] nums2) {
	 
	        // Write your code here
	        Set<Integer> set = new HashSet<Integer>();
	        for (int m = 0; m < nums1.length; m++)
	            set.add(nums1[m]);
	        List<Integer> array = new LinkedList<Integer>();
	        for (int i = 0; i < nums2.length; i++) {
	            if (set.contains(nums2[i])) {
	                array.add(nums2[i]);
	            }
	        }
	        int _index = 0;
	        Set<Integer> _set = new HashSet<Integer>(array);
	        int[] _array = new int[_set.size()];
	        Iterator<Integer> iter = _set.iterator();
	        while (iter.hasNext()){
	            _array[_index]=iter.next().intValue();
	            _index = _index+1;
	        }
	        return _array;
	    }
	 }
