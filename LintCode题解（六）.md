## 奇偶分割数组
分割一个整数数组，使得奇数在前偶数在后。

### 样例
给定 [1, 2, 3, 4]，返回 [1, 3, 2, 4]。

### 算法
这题还是比较简单的，方法很多很多，我们将数组循环遍历，将遍历中的偶数和奇数相互交换就ok了。

	public class Solution {
    /*
     * @param nums: an array of integers
     * @return: nothing
     */
	    public void partitionArray(int[] nums) {
	        // write your code here
	        int length = nums.length;
	        int temp = 0;
	        int index = 0;
	        if(length==0)
	            return;
	        for(int i=0;i<length;i++){
	            if(nums[i]%2==0){
	                index = i;
	                while(nums[index]%2==0){
	                    if(index!=length-1)
	                        index=index+1;
	                    else
	                        break;
	                }
	                temp = nums[i];
	                nums[i] = nums[index];
	                nums[index] = temp;
	            }
	        }
	    }
	}

## 二进制中1个数
计算在一个 32 位的整数的二进制表示中有多少个 1.
### 样例

给定 32 (100000)，返回 1

给定 5 (101)，返回 2

给定 1023 (111111111)，返回 9	

### 算法
按与计算
1&1==1，1&0==0，0&&0==0；下面是很巧妙的一个方法。
	
	public class Solution {
    /*
     * @param num: An integer
     * @return: An integer
     */
	    public int countOnes(int num) {
	        // write your code here
	        int count = 0;
	        while(num!=0){
	            num = num & (num-1);
	            count++;
	        }
	        return count;       
	    }
	}


## 反转整数
将一个整数中的数字进行颠倒，当颠倒后的整数溢出时，返回 0 (标记为 32 位整数)。


### 样例
给定 x = 123，返回 321

给定 x = -123，返回 -321

### 算法
反转很简单，难点在于颠倒后证书溢出判断，int32位范围是-2^31~2^31-1,这里使用temp/10与reversed_n进行判断，当temp溢出时，/10不等于reversed_n，所以可判断溢出成立。

	public class Solution {
	    /*
	     * @param n: the integer to be reversed
	     * @return: the reversed integer
	     */
	    public int reverseInteger(int n) {
	        // Write your code here
	        int reversed_n = 0;

	        while (n != 0) {
	            int temp = reversed_n * 10 + n % 10;
	            n = n / 10;
	            if (temp / 10 != reversed_n) {
	                reversed_n = 0;
	                break;
	            }
	            reversed_n = temp;
	        }
	        return reversed_n;
	    }
	}


## 加一

给定一个非负数，表示一个数字数组，在该数的基础上+1，返回一个新的数组。

该数字按照大小进行排列，最大的数在列表的最前面。

### 样例

给定 [1,2,3] 表示 123, 返回 [1,2,4].

给定 [9,9,9] 表示 999, 返回 [1,0,0,0].


### 算法

google的题，哪道没有陷阱，第一次被坑了。
注意不要把数组转为int型整数，因为不确定数组大小会导致溢出。
本题思想是，若数组值都为9，则增加数组length，首位设为1，其他位为0；否则，末位加1，结束返回。

	public class Solution {
	    /*
	     * @param digits: a number represented as an array of digits
	     * @return: the result
	     */
	    public static int[] plusOne(int[] digits) {
	        // write your code here
	        boolean index = true;
	        for(int n=0; n<digits.length;n++){
	            if(digits[n]!=9)
	                index = false;
	        }
	        if(index==true){
	            int[] num = new int[digits.length+1];
	            num[0] = 1;
	            for(int m=1;m<digits.length+1;m++)
	                num[m] = 0;
	            return num;
	        }
	        for(int i=digits.length-1;i>=0;i--){
	            if(digits[i]!=9){
	                digits[i] = digits[i]+1;
	                break;
	            }else{
	                digits[i] = 0;
	            }
	        }
	        return digits;
	    }
	}


## 把排序数组转换为高度最小的二叉搜索树

给一个排序数组（从小到大），将其转换为一棵高度最小的排序二叉树。

### 样例

给出数组 [1,2,3,4,5,6,7], 返回

		     4
		   /   \
		  2     6
		 / \    / \
		1   3  5   7

## 算法

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
	public class Solution(){
	    /**
	     * @param nums: an integer array
	     * @return: a tree node
	     */
		public TreeNode sortArrayToBST(int[] A){
			if(A==null)
				return null;
			return bulidTree(A, 0, A.length-1);	
		}

		public void buildTree(int[] A, int start, int end){
			if(start>end)
				return null;
			int middle = (start+end)/2;
			TreeNode root = new TreeNode(A[middle]);
			A.left = buildTree(A, start, middle-1);
			A.right = buildTree(A, middle+1, end); 	
		}
	}


## 二进制求和
给定两个二进制字符串，返回他们的和（用二进制表示）。

## 样例
a = 11

b = 1

返回 100

## 算法

这种题千万不要将二进制转化为整数，这样转化复杂度将提高几何倍数。
注意如果相应位相加结果为2（1+1），则结果设为1，这时要设一个哨兵，相应位溢出时，哨兵设为1；结果为3（1+1+哨兵）时，相同道理。

	public class Solution {
	    /*
	     * @param a: a number
	     * @param b: a number
	     * @return: the result
	     */
	    public String addBinary(String a, String b) {
	        // write your code here
	        if(b.length()==0&&a.length()==0)
	            return null;
	        String str = "";
	        int index = 0;
	        int i = 0;
	        int middle = 0;
	        String[] A = a.split("");
	        String[] B = b.split("");
	        while(i<A.length||i<B.length) {
	            if (i >= A.length && i < B.length)
	                middle = Integer.valueOf(B[B.length-1-i]) + index;
	            else if (i >= B.length && i < A.length)
	                middle = Integer.valueOf(A[A.length-1-i]) + index;
	            else if (i < A.length && i < B.length)
	                middle = Integer.valueOf(A[A.length-1-i]) + Integer.valueOf(B[B.length-1-i]) + index;
	            if (middle == 3) {
	                str = 1 + str;
	                index = 1;
	                //i=i+1;
	            } else if (middle == 2) {
	                str = 0 + str;
	                index = 1;
	            } else if (middle == 0 || middle == 1) {
	                str = middle + str;
	                index = 0;
	            }
	            i = i + 1;
	        }
	        if(index==1)
	            str = index+str;
	        return str;
	    }
	}	