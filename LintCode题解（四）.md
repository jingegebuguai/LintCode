title: LintCode题解（二）
author: 惠惠
tags:
  - 算法
categories:
  - 计算机
date: 2017-09-06 10:14:00
---

## 栅栏染色
我们有一个栅栏，它有n个柱子，现在要给柱子染色，有k种颜色可以染。
必须保证不存在超过2个相邻的柱子颜色相同，求有多少种染色方案。

## 样例
n = 3, k = 2, return 6

      	post 1,   post 2, post 3
	way1    0         0       1 
	way2    0         1       0
	way3    0         1       1
	way4    1         0       0
	way5    1         0       1
	way6    1         1       0

## 算法题解

这种数学类的题目绝大多数，使用递归解题。
递归的要求有两个：
	
- 临界条件，这里的临界条件是柱子n个数为0时，返回0；柱子n个数为1时，则有k种染色方法；柱子数为2时，则有k^2种染色方法。
- 递归体，这里的递归体要自下而上去考虑，可以分为两种情况，当最后两根柱子颜色相同时，倒数第三根柱子的染色方法则有k-1种，可表示为numWays(n-2,k)*(k-1);若是要求最后两根柱子颜色不同，则在前n-
根柱子已经部署好的情况下，最后一根柱子的染色方法为k-1种，则可表示为numWanys(n-1,k)*(k-1)。

	public class Solution {
    /*
     * @param n: non-negative integer, n posts
     * @param k: non-negative integer, k colors
     * @return: an integer, the total number of ways
     */
	   	public static int numWays(int n, int k) {
	        // write your code here
            if(n>2&&k==1){
                return 0;
            }
            if(n==0)
                return 0;
            if(n==1)
                return k;
            if(n==2)
                return k*k;
            else
                return numWays(n-1,k)*(k-1)+numWays(n-2,k)*(k-1);
        }
	}

## 快乐数
写一个算法来判断一个数是不是"快乐数"。

一个数是不是快乐是这么定义的：对于一个正整数，每一次将该数替换为他每个位置上的数字的平方和，然后重复这个过程直到这个数变为1，或是无限循环但始终变不到1。如果可以变为1，那么这个数就是快乐数。

### 样例
19为一个快乐数。

	1^2 + 9^2 = 82
	8^2 + 2^2 = 68
	6^2 + 8^2 = 100
	1^2 + 0^2 + 0^2 = 1

### 算法题解
这题我的算法写的并不是很严谨，不论时间复杂度还是对死循环的处理，可能都不好，但是很暴力有木有。

我的思想是将每个数转化为字符数组，再从数组中提取每个数字字符，转化为整形，平方相加得到新值，然后依次循环下去，直到数值为1为止。

	public class Solution {
    /*
     * @param n: An integer
     * @return: true if this is a happy number or false
     */
	    public static boolean isHappy(int n) {
	        // write your code
	        boolean index = false;
	        int _index = 0;
	        while(n!=1){
	            String s = String.valueOf(n);
	            String _s[] = s.split("");
	            n = 0;
	            for(int i = 0;i<_s.length;i++){
	                int num = Integer.valueOf(_s[i]);
	                n = n + num*num;
	            }
	            if(_index==32767)
	                break;
	            _index = _index+1;     
	        }
	        if(n==1)
	            index = true;
	        return index;
	    }
	}


## 二叉树的所有路径
给一棵二叉树，找出从根节点到叶子节点的所有路径。

### 样例

给出下面这棵二叉树：
	
		   1
		 /   \
		2     3
		 \
		  5	

所有根到叶子的路径为：

		[
		  "1->2->5",
		  "1->3"
		]

### 算法题解
看输出内容可知，这是前序遍历，所以按照根左右的方式，使用递归即可。

	/**
	 * Definition of TreeNode:
	 * public class TreeNode {
	 *     public int val;
	 *     public TreeNode left, right;
	 *     public TreeNode(int val) {
	 *         this.val = val;
	 *         this.left = this.right = null;
	 *     }
	 * }
	 */
	public class Solution {
	    /*
	     * @param root: the root of the binary tree
	     * @return: all root-to-leaf paths
	     */
	    public List<String> binaryTreePaths(TreeNode root) {
	        // write your code here
	        List<String> list = new LinkedList<String>();
	        if(root!=null)
	            addPath(root, String.valueOf(root.val),list);
	        return list;
	    }    
	    public void addPath(TreeNode root, String path, List<String> list){    
	        if(root==null)
	            list.addAll(Arrays.asList());
	        if(root.right==null&&root.left==null)
	            list.add(path);
	        if(root.left!=null)
	            addPath(root.left, path+"->"+String.valueOf(root.left.val), list);
	        if(root.right!=null)
	            addPath(root.right, path+"->"+String.valueOf(root.right.val),list);
	    }
	}


## Rotate Words(新题)
The words are same rotate words if rotate the word to the right by loop, and get another. Count how many different rotate word sets in dictionary.

E.g. picture and turepic are same rotate words.

### 样例
Given dict = ["picture", "turepic", "icturep", "word", "ordw", "lint"]
return 3.

"picture", "turepic", "icturep" are same ratote words.
"word", "ordw" are same too.
"lint" is the third word that different from the previous two words.

### 算法题解

我只会笨方法，哈哈。
第一步，将List转化为Set
第二步，判断Set是否为空，不为空，则以此读取里面的字符串。需要判断，Set里是否contains每个字符串的不同旋转组合

	public static int countRotateWords(List<String> list){
       int index =0;
       Set<String> set = new LinkedHashSet<String>();
       set.addAll(list);
       if(list.isEmpty())
           return 0;
       else {
           while (!set.isEmpty()) {
               String str = set.iterator().next();
               for (int i = 0; i < str.length(); i++) {
                   if (set.contains(str.substring(i, str.length()) + str.substring(0, i)))
                       set.remove(str.substring(i, str.length()) + str.substring(0, i));
               }
               index = index + 1;
           }
       }
       return index;
    }

## 等价二叉树
检查两棵二叉树是否等价。等价的意思是说，首先两棵二叉树必须拥有相同的结构，并且每个对应位置上的节点上的数都相等。

### 样例

	    1             1
	   / \           / \
	  2   2   and   2   2
	 /             /
	4             4

就是两棵等价的二叉树。


	    1             1
	   / \           / \
	  2   3   and   2   3
	 /               \
	4                 4

就不是等价的。

### 算法题解

	/**
	 * Definition of TreeNode:
	 * public class TreeNode {
	 *     public int val;
	 *     public TreeNode left, right;
	 *     public TreeNode(int val) {
	 *         this.val = val;
	 *         this.left = this.right = null;
	 *     }
	 * }
	 */


	public class Solution {
	    
	    /*
	     * @param a: the root of binary tree a.
	     * @param b: the root of binary tree b.
	     * @return: true if they are identical, or false.
	     */
	    public boolean isIdentical(TreeNode a, TreeNode b) {
	        // write your code here
	        if(a==null&&b==null)
	            return true;
	        if(a==null&&b!=null||a!=null&&b==null||a.val!=b.val)  
	            return false;
	        return isIdentical(a.left,b.left)&&isIdentical(a.right,b.right);
	    }
	}