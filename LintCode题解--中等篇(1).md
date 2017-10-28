title: LintCode题解——中等篇（一）
author: 惠惠
tags:
  - LintCode
categories:
  - 计算机
date: 2017-10-27 14:14:00
---

容易题刷烂了，略感无趣
## 三角形计数

给定一个整数数组，在该数组中，寻找三个数，分别代表三角形三条边的长度，问，可以寻找到多少组这样的三个数来组成三角形？


### 样例
例如，给定数组 S = {3,4,6,7}，返回 3

其中我们可以找到的三个三角形为：

	{3,4,6}
	{3,6,7}
	{4,6,7}

给定数组 S = {4,4,4,4}, 返回 4

其中我们可以找到的三个三角形为：

	{4(1),4(2),4(3)}
	{4(1),4(2),4(4)}
	{4(1),4(3),4(4)}
	{4(2),4(3),4(4)}

### 算法详解
这题按道理算不上是中等题，其实最暴力的方法就是来个三重for循环，每次取三个数，把所有数的组合都尝试一遍，判断每个组合是否符合三角形的定义。

	public class Solution {
	    /*
	     * @param S: A list of integers
	     * @return: An integer
	     */
	    public int triangleCount(int[] S) {
	        // write your code here
	        int index = 0;
	        for(int i=0; i<S.length-2; i++){
	            for(int j=i+1; j<S.length-1;j++){
	                for(int k=j+1;k<S.length;k++){
	                    if(isTriangle(S[i],S[j],S[k]))
	                        index++;
	                }
	            }
	        }
	        return index;
	    }
	    
	    public static boolean isTriangle(int a, int b, int c){
	        if(a+b>c&&a+c>b&&b+c>a)
	            return true;
	        else
	            return false;
	    }
	}

解法二：
三角形两边之和大于第三边，先将数组排序，然后定位最长边与最短边，然后寻找第三条边的数量，第三条边满足：

最长边 >= 第三条边 > 最长边 - 最短边
可以利用二分查找寻找最短的第三条边的下标，然后和最长边下标之差就是第三条边的数量
最后继续定位定位最长边与最短边


	class Solution {
	public:
	    /*
	     * @param : A list of integers
	     * @return: An integer
	     */
	    int triangleCount(vector<int> S) {
	        // write your code here
	        int size = S.size();
	        if (size <= 0) {
	            return 0;
	        }

	        int result = 0;
	        sort(S.begin(), S.end());
	        for (int i = 0; i < size - 1; i++) {
	            for (int j = i + 1; j < size; j++) {
	                int third = S[j] - S[i];
	                int low = i + 1, high = j;
	                while (low < high) {
	                    int mid = low + (high - low) / 2;
	                    if (S[mid] > third) {
	                        high = mid;
	                    }
	                    else {
	                        low = mid + 1;
	                    }
	                }
	                result += (j - low);
	            }
	        }
	        return result;
	    }
	};

## 计算最大值
给一个字符串类型的数字, 写一个方法去找到最大值, 你可以在任意两个数字间加 + 或 *

您在真实的面试中是否遇到过这个题？ Yes
### 样例
给出 str = 01231, 返回 10 ((((0 + 1) + 2) * 3) + 1) = 10 我们得到了最大值 10
给出 str = 891, 返回 73 因为 8 * 9 * 1 = 72 和 8 * 9 + 1 = 73, 所以73是最大值

	public class Solution {
	    /*
	     * @param : the given string
	     * @return: the maximum value
	     */
	    public int calcMaxValue(String str) {
	        // write your code here
	        if(str.equals(""))
	            return 0;
	        String[] array = str.split("");
	        int max_result = Integer.parseInt(array[0]);
	        for(int i=0;i<array.length-1;i++){
	            int add = max_result+Integer.parseInt(array[i+1]);
	            int multify = max_result*Integer.parseInt(array[i+1]);
	            if(add>=max_result&&add>=multify)
	                max_result = add;
	            if(multify>=max_result&&multify>add)
	                max_result = multify;
	        }
	        return max_result;
	    }
	};

## 	重复子串
写一个方法, 给一个由 N 个字符构成的字符串 A和一个由 M 个字符构成的字符串 B, 返回 A 必须重复的次数，使得 B 是重复字符串的子串.如果 B 不可能为重复字符串的子串, 则返回 -1.

 注意事项

Assume that 0 <= N <= 1000, 1 <= M <= 1000

您在真实的面试中是否遇到过这个题？ Yes
### 样例
给出 A = abcd, B = cdabcdab
你的方法需要返回 3, 因为在重复字符串 A 3次之后我们得到了字串 abcdabcdabcd. 字符串B是这个字符串的一个子串.

### 算法详解
先把B中间包含A的部分全部去除，看两端剩下几个字符串，如果大于两个则return -1，若是两个或一个，则再判断这两个或一个字串是否能被包含在A中。

	import java.util.LinkedList;
	import java.util.List;

	public class Solution {
	    /*
	     * @param : string A to be repeated
	     * @param : string B
	     * @return: the minimum number of times A has to be repeated
	     */
	    public static int repeatedString(String A, String B) {
	        // write your code here
	        if(A.length()==0)
	            return -1;
	        if(A.length()>=B.length())
	            if(A.contains(B))
	                return 1;
	        String[] b = B.split(A);
	        int index = -1;
	        List<String> list = new LinkedList<>();
	        for(int i=0;i<b.length;i++) {
	            if (!b[i].equals(""))
	                list.add(b[i]);
	        }
	        if(A.length()<B.length()){
	            if(list.size()>2)
	                return -1;
	            else {
	                if (list.size() == 1)
	                    if (B.substring(0, 4).equals(A) || B.substring(B.length() - 4, B.length()).equals(A)) {
	                        if(!B.substring(0,4).equals(A)){
	                            if(A.substring(A.length()-list.get(0).length(),A.length()).equals(list.get(0)))
	                                index = 1 + (B.length() - list.get(0).length()) / A.length();
	                        }
	                        if(!B.substring(B.length() - 4, B.length()).equals(A)){
	                            if(A.substring(0, list.get(0).length()).equals(list.get(0)))
	                                index = 1 + (B.length() - list.get(0).length()) / A.length();
	                        }
	                    }
	                if (list.size() == 2)
	                    if (!B.substring(0, 4).equals(A) && !B.substring(B.length() - 4, B.length()).equals(A)) {
	                        if(A.substring(A.length()-list.get(0).length(),A.length()).equals(list.get(0))&&A.substring(0, list.get(1).length()).equals(list.get(1)))
	                            index = 2 + (B.length() - list.get(0).length() - list.get(1).length()) / A.length();
	                    }
	                if(list.size() == 0)
	                    index = B.length()/A.length();
	            }
	        }
	        return index;
	    }
	}

## 	二进制时间
给了一个二进制显示时间的手表和一个非负整数 n, n 代表在给定时间表上 1 的数量, 返回所有可能的时间

 注意事项

输出的顺序没有要求.
小时不能包含前导零, 比如 "01:00" 是不允许的, 应该为 "1:00".
分钟必须由两位数组成, 可能包含前导零, 比如 "10:2" 是无效的, 应该为 "10:02".

您在真实的面试中是否遇到过这个题？ Yes
### 样例
给出 n = 1
返回 ["1:00", "2:00", "4:00", "8:00", "0:01", "0:02", "0:04", "0:08", "0:16", "0:32"]

### 算法详解
这是一道非常有意思的题目，当然给出一个数，分别把这个数分到时和分里再细分，形成组合。这里给出两个附带的静态函数，具体代码实现很容易懂：

	import java.util.LinkedList;
	import java.util.List;

	public class Solution {
	    /*
	     * @param : the number of "1"s on a given timetable
	     * @return: all possible time
	     */
	    public static List<String> binaryTime(int num) {
	        // Write your code here
	        List<String> list = new LinkedList<>();
	        for(int i=0;i<=num;i++){
	            List<String> list1 = binarylist(i, true);
	            List<String> list2 = binarylist(num-i, false);
	            for(String _list1 : list1){
	                for(String _list2 : list2){
	                    list.add(_list1+":"+_list2);
	                }
	            }
	        }
	        return list;
	    }

	    public static List<String> binarylist(int _num, boolean index) {
	        List<String> list = new LinkedList<>();
	        if(index == false)
	            for(int k=0;k<60;k++){
	                if(_num==binaryOneNum(k)&&k<10)
	                    list.add("0"+k);
	                if(_num==binaryOneNum(k)&&k>=10)
	                    list.add(String.valueOf(k));
	            }
	        else
	            for(int j=0;j<12;j++){
	                if(_num==binaryOneNum(j))
	                    list.add(String.valueOf(j));
	            }
	        return list;
	    }

	    public static int binaryOneNum(int timeNum) {
	        int num = 0;
	        while (timeNum != 0) {
	            if (timeNum % 2 == 1)
	                num = num + 1;
	             timeNum = timeNum/2;
	        }
	        return num;
	    }
	};