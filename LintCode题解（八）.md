## 删除元素

给定一个数组和一个值，在原地删除与值相同的数字，返回新数组的长度。

元素的顺序可以改变，并且对新的数组不会有影响。

### 样例

给出一个数组 [0,4,4,0,0,2,4,4]，和值 4

返回 4 并且4个元素的新数组为[0,0,0,2]

### 算法

	public class Solution {
	    /*
	     * @param A: A list of integers
	     * @param elem: An integer
	     * @return: The new length after remove
	     */
	    public int removeElement(int[] A, int elem) {
	        // write your code here
	        int index = 0;
	        for(int i=0;i<A.length;i++){
	            if(A[i]!=elem){
	                index = index + 1;
	            }
	            
	        }
	        //if(index!=0){  
	        int[] array = new int[index];
	     
	        int a = 0;
	        
	        for(int j=0;j<A.length;j++){
	            if(A[j]!=elem){
	                array[a] = A[j];
	                a = a+1;
	            }
	        }
	        for(int m =0;m<index;m++){
	            if(m<array.length)
	                A[m]=array[m];
	        }
	        return index;
	    }
	}

## 链表求和
你有两个用链表代表的整数，其中每个节点包含一个数字。数字存储按照在原来整数中相反的顺序，使得第一个数字位于链表的开头。写出一个函数将两个整数相加，用链表形式返回和。

### 样例
样例
给出两个链表 3->1->5->null 和 5->9->2->null，返回 8->0->8->null

### 算法
没有特别的亮点，纯碎是小学加减法

	/**
	 * Definition for singly-linked list.
	 * public class ListNode {
	 *     int val;
	 *     ListNode next;
	 *     ListNode(int x) {
	 *         val = x;
	 *         next = null;      
	 *     }
	 * }
	 */
	public class Solution {
	    /*
	     * @param l1: the first list
	     * @param l2: the second list
	     * @return: the sum list of l1 and l2 
	     */
	    public ListNode addLists(ListNode l1, ListNode l2) {
	        // write your code here
	        ListNode head = new ListNode(0);
	        ListNode _head = new ListNode(0);
	        head.next = null;
	        int num_1 = 0;
	        int num_2 = 0;
	        int val = 0;
	        int index = -1;
	        if(l1==null&&l2==null)
	            return head;
	        while(l1!=null&&l2!=null){
	            num_1 = l1.val;
	            num_2 = l2.val;
	            val = num_1+num_2;
	            if(index==0)
	                val = val+1;
	            if(val>9){
	                index = 0;
	                val = val-10;
	            }else
	                index = -1;
	            dealList(head, val);
	            l1 = l1.next;
	            l2 = l2.next;
	        }
	        while(l1!=null){
	            if(index==0)
	                val = l1.val+1;
	            else
	                val = l1.val;
	            if(val>9){
	                index = 0;
	                val = 0;
	            }else
	                index = -1;
	            dealList(head,val);
	            l1 = l1.next;
	        }
	        while(l2!=null){
	            if(index==0)
	                val = l2.val+1;
	            else 
	                val = l2.val;
	            if(val==10){
	                index = 0;
	                val = 0;
	            }else
	                index = -1;
	            dealList(head, val);
	            l2 = l2.next;
	        }
	        if(index==0){
	            dealList(head, 1);
	        }
	        while(head.next!=null){
	            val = head.next.val;
	            dealList(_head, val);
	            head.next = head.next.next;
	        }   
	        return _head.next;
	     }
	     public void dealList(ListNode head, int val){
	        ListNode node = new ListNode(val);
	        node.next = head.next;
	        head.next = node;
	     }
	}

