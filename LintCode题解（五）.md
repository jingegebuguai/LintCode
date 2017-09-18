title: LintCode题解（五）
author: 惠惠
tags:
  - LintCode
categories:
  - 计算机
date: 2017-10-17 10:14:00
---

## 岛屿个数
给一个01矩阵，求不同的岛屿的个数。

0代表海，1代表岛，如果两个1相邻，那么这两个1属于同一个岛。我们只考虑上下左右为相邻。

### 样例

	[
	  [1, 1, 0, 0, 0],
	  [0, 1, 0, 0, 1],
	  [0, 0, 0, 1, 1],
	  [0, 0, 0, 0, 0],
	  [0, 0, 0, 0, 1]
	]

如上矩阵有3个岛屿。

### 算法解答
这题比较复杂，需要使用到深度遍历，使用递归将已确定的岛屿由true转化为false。并且递归他的四周其他岛屿。

	public class Solution {
    /**
     * @param grid a boolean 2D matrix
     * @return an integer
     */
	    public int numIslands(boolean[][] grid) {
	        // Write your code here
	        if(grid.length==0)
	            return 0;
	        int row = grid.length;
	        int column = grid[0].length;
	        int count = 0;
	        
	        for(int i = 0;i < row;i++){
	            for(int j  = 0;j < column;j++){
	                if(grid[i][j]==true){
	                    dfs_Islands(grid,i,j);
	                    count++;
	                }
	            }
	        }
	        return count;
	    }
	    private void dfs_Islands(boolean[][] grid, int i, int j){
	        if(i<0||j<0||i>=grid.length||j>=grid[0].length)
	            return; 
	        if(grid[i][j]==true){
	            grid[i][j]=false;
	            dfs_Islands(grid, i-1, j);
	            dfs_Islands(grid, i+1, j);
	            dfs_Islands(grid, i, j-1);
	            dfs_Islands(grid, i, j+1);
	        }    
	    }
	}

## 有效回文串
给定一个字符串，判断其是否为一个回文串。只包含字母和数字，忽略大小写。

 注意事项

你是否考虑过，字符串有可能是空字符串？这是面试过程中，面试官常常会问的问题。

在这个题目中，我们将空字符串判定为有效回文。

### 样例
"A man, a plan, a canal: Panama" 是一个回文。

"race a car" 不是一个回文。

### 算法

	public class Solution {
    /*
     * @param s: A string
     * @return: Whether the string is a valid palindrome
     */
	    public static boolean isPalindrome(String s) {
	        // write your code here
	        boolean index = true;
	        if(s.length()==0)
	            return index;
	        String[] str = s。toLowerCase.split("");
	        int low = 0;
	        int high = s.length()-1;
	        while(low<high){
	            if(str[low].matches("\\w")&&str[high].matches("\\w")){
	                if(str[low].equals(str[high])&&low==high-1)
	                    low = low+1;
	                else if(str[low].equals(str[high])&&low!=high+1){
	                    low = low+1;    
	                    high = high-1;
	                }
	                else if(!str[low].equals(str[high]))
	                    break;
	            }else{
	                if(!str[low].matches("\\w"))
	                    low = low+1;
	                else if(!str[high].matches("\\w"))
	                    high = high-1;
	            }
	        }
	        if(low==high)
	            index=true;
	        else
	            index=false;
	        return index;    
	    }
	}

上面的算法完全是我凑出来的，汗颜啊。
下面是别人写的，给予参考：

	public class Solution {
    /**
     * @param s A string
     * @return Whether the string is a valid palindrome
     */
	    public boolean isPalindrome(String s) {
	        // Write your code here
	        if(s.equals("")) return true;
	        int len = s.length();
	        int left = 0;
	        int right = len - 1;
	        s = s.toLowerCase();
	        while(left < right){
	            char leftchar = s.charAt( left );
	            char rightchar = s.charAt( right );
	            while(!isValid(leftchar)){
	                left ++;
	                leftchar = s.charAt( left );
	                if(left>=right) return true;
	            }
	            while(!isValid(rightchar)){
	                right --;
	                rightchar = s.charAt( right );
	                if(right<=left) return true;
	            }
	            if(leftchar != rightchar)
	                return false;
	            left ++;
	            right --;
	        }
	        
	        return true;

	    }
	    public boolean isValid(char ch){
	        if(ch>='a' && ch <= 'z')
	            return true;
	        if(ch >='0' && ch <= '9')
	            return true;
	        return false;
	    }
	}	

## 最长上升子序列
给定一个整数数组（下标从 0 到 n-1， n 表示整个数组的规模），请找出该数组中的最长上升连续子序列。（最长上升连续子序列可以定义为从右到左或从左到右的序列。）

 注意事项

time


### 样例

给定 [5, 4, 2, 1, 3], 其最长上升连续子序列（LICS）为 [5, 4, 2, 1], 返回 4.

给定 [5, 1, 2, 3, 4], 其最长上升连续子序列（LICS）为 [1, 2, 3, 4], 返回 4.


### 算法

	public class Solution {
	    /*
	     * @param A: An array of Integer
	     * @return: an integer
	     */
	    public int longestIncreasingContinuousSubsequence(int[] A) {
	        // write your code here
	        if(A.length==0)
	            return 0;
	        int up = 1;
	        int _up =1;
	        int _down=1;
	        int down=1;
	        for(int i=1;i<A.length;i++){
	            if(A[i]>A[i-1]){
	                _up = _up+1;
	                if(_down>=down){
	                    down = _down;
	                    _down = 1;
	                }else
	                    _down =1;
	            }
	            if(A[i]<A[i-1]){
	                _down = _down+1;
	                if(_up>=up){
	                    up = _up;
	                    _up = 1;
	                }else
	                    _up=1;
	            }
	        }
	        if(_up>=up)
	            up = _up;
	        if(_down>=down)
	            down = _down;
	        if(down>=up)
	            return down;
	        else
	            return up;
	    }
	}	

	