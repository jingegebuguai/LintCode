title: LintCode题解（七）
author: 惠惠
tags:
  - LintCode
categories:
  - 计算机
date: 2017-10-17 10:14:00
---

## 判断数独是否合法
请判定一个数独是否有效。

该数独可能只填充了部分数字，其中缺少的数字用 . 表示。

### 注意事项

一个合法的数独（仅部分填充）并不一定是可解的。我们仅需使填充的空格有效即可。

### 样例

### 算法

	public class Solution {
	    /*
	     * @param board: the board
	     * @return: whether the Sudoku is valid
	     */
	    public boolean isValidSudoku(char[][] board) {
	        // write your code here
	        boolean[] index = new boolean[9];

	        for(int j = 0; j<9; j++){
	            Arrays.fill(index, false);
	            for(int i = 0; i<9; i++){
	                if(!checkSudoku(index, board[j][i]))
	                    return false;
	            }
	        }
	        
	        for(int i = 0; i<9; i++){
	            Arrays.fill(index, false);
	            for(int j = 0; j<9; j++){
	                if(!checkSudoku(index, board[i][j]))
	                    return false;
	            }
	        }
	        
	        for(int i=0; i<9; i=i+3){
	            for(int j = 0; j<9; j=j+3){
	                Arrays.fill(index, false);
	                for(int k = 0; k<9; k++){
	                    if(!checkSudoku(index, board[i+k/3][j+k%3]))
	                        return false;
	                }
	            }
	        }
	        return true;
	    }
	    
	    public boolean checkSudoku(boolean[] index, char num){
	        if(num == '.')
	            return true;
	        int _num = num-'0';    
	        if(num>9||num<0||index[_num-1])
	            return false;
	        
	        index[_num-1] = true;
	        return true;    
	    }
	}

## 平面列表
给定一个列表，该列表中的每个要素要么是个列表，要么是整数。将其变成一个只包含整数的简单列表。

### 样例
给定 [1,2,[1,2]]，返回 [1,2,1,2]。

给定 [4,[3,[2,[1]]]]，返回 [4,3,2,1]。

### 算法

使用递归
	
	 /**
	 * // This is the interface that allows for creating nested lists.
	 * // You should not implement it, or speculate about its implementation
	 * public interface NestedInteger {
	 *
	 *     // @return true if this NestedInteger holds a single integer,
	 *     // rather than a nested list.
	 *     public boolean isInteger();
	 *
	 *     // @return the single integer that this NestedInteger holds,
	 *     // if it holds a single integer
	 *     // Return null if this NestedInteger holds a nested list
	 *     public Integer getInteger();
	 *
	 *     // @return the nested list that this NestedInteger holds,
	 *     // if it holds a nested list
	 *     // Return null if this NestedInteger holds a single integer
	 *     public List<NestedInteger> getList();
	 * }
	 */
	public class Solution {

	    // @param nestedList a list of NestedInteger
	    // @return a list of integer
	    public List<Integer> flatten(List<NestedInteger> nestedList) {
	        // Write your code here
	        
	        List<Integer> list = new ArrayList<Integer>();
	        for(NestedInteger ele:nestedList){
	            if(ele.isInteger())
	                list.add(ele.getInteger());
	            else
	                list.addAll(flatten(ele.getList()));
	                
	        }
	        return list;
	    }
	    
	}


## 克隆二叉树

深度复制一个二叉树。

给定一个二叉树，返回一个他的 克隆品 

### 样例
给定一颗二叉树
	
		1				
	   /  \
	  2    3     
	 / \
	4   5
		=>
		1
	   /  \
	  2    3
	 / \
	4   5

### 算法

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
	    /**
	     * @param root: The root of binary tree
	     * @return root of new tree
	     */
	    public TreeNode cloneTree(TreeNode root) {
	        // Write your code here
	        if(root==null)
	            return null;
	        TreeNode _root = new TreeNode(root.val);
	        if(root.left!=null)
	            _root.left = cloneTree(root.left);
	        if(root.right!=null)
	            _root.right = cloneTree(root.right);
	        return _root;    
	    }
	}


## 序号排列
排列序号

给出一个不含重复数字的排列，求这些数字的所有排列按字典序排序后该排列的编号。其中，编号从1开始。

### 样例
例如，排列[1,2,4]是第1个排列。

### 算法
原谅我一开始题目意思都没有理解。
对于一个数321，三个数的排列有3！种，知道第一个数，排列有2！种，知道第二个数，排列有1！知道第三个数，排列有0！；
第一位后小于第一位的数为x，第二位后小于第二位的数有y，第三位后为0，所以最后排列序号的结果为index = x*2!+y*1!。

	public class Solution {
    /*
     * @param A: An array of integers
     * @return: A long integer
     */
	    public long permutationIndex(int[] A) {
	        // write your code here
	        long index = 0;
	        //long _middle = 0;
	        long middle = 1;
	        long num = 0;
	        for(int i=0;i<A.length;i++){
	            for(int m=A.length-i-1;m>0;m--)
	                middle = middle*m;
	            for(int j=i+1;j<A.length;j++){
	                if(A[i]>A[j])
	                    num=num+1;
	            }
	            index = index + num*middle;
	            num = 0;
	            middle = 1;
	            //_middle = 0;
	        }
	        return index+1;
	    }
	}

## 链表插入排序
用插入排序对链表排序

### 样例
Given 1->3->2->0->null, return 0->1->2->3->null	

### 算法
思想：先将链表的数值存到数组中，再对数组排序，将数组的值倒序插入到head后面。主要核心为下面三句代码：

	ListNode node = new ListNode(_list[j]);
    node.next = _head.next;
    _head.next = node;

具体算法如下，相对简单

	/**
	 * Definition for ListNode.
	 * public class ListNode {
	 *     int val;
	 *     ListNode next;
	 *     ListNode(int val) {
	 *         this.val = val;
	 *         this.next = null;
	 *     }
	 * }
	 */


	public class Solution {
	    /*
	     * @param head: The first node of linked list.
	     * @return: The head of linked list.
	     */
	    public ListNode insertionSortList(ListNode head) {
	        // write your code here
	        if(head==null)
	            return head;
	            
	        ListNode _head = new ListNode(0);
	        ListNode cur = new ListNode(0);
	        cur.next = head;
	        cur = cur.next;
	        List<Integer> list = new LinkedList<Integer>();
	        while(cur!=null){
	            list.add(cur.val);
	            cur = cur.next;
	        }
	        Iterator<Integer> iter = list.iterator();
	        int length = list.size();
	        
	        int i = 0;
	        int[] _list = new int[length];
	        while(iter.hasNext()){
	            _list[i] = iter.next();
	            i = i+1;
	        }
	        Arrays.sort(_list);
	        if(_list.length==1){
	            ListNode _node = new ListNode(_list[0]);
	            return _node;
	        }
	        for(int j = _list.length-1;j>=0;j--){
	            ListNode node = new ListNode(_list[j]);
	            node.next = _head.next;
	            _head.next = node;
	        }
	        return _head.next;
	    }	    
	}    