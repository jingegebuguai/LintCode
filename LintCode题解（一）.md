## 单例singleton（题号204）
### 问题描述
单例 是最为最常见的设计模式之一。对于任何时刻，如果某个类只存在且最多存在一个具体的实例，那么我们称这种设计模式为单例。例如，对于 class Mouse (不是动物的mouse哦)，我们应将其设计为 singleton 模式。

你的任务是设计一个 getInstance 方法，对于给定的类，每次调用 getInstance 时，都可得到同一个实例。

样例（Java中）：
	
	A a = A.getInstance();
	A b = A.getInstance();

a应等于b。

### 解答过程

若是看过设计模式的应该都清楚，单例是一种创建型模式。并且单例类只能有一个实例，必须自己创建自己的唯一实例，必须给其他对象提供这一实例。一般有如下几种实现方式：

懒汉式，线程不安全：
	
	public class Singleton{
		private static Singleton instance;
		private Singletion(){}
		public static Singleton getInstance(){
			if(instance==null){
				instance = new Singletan();
			}
		} 
	}

懒汉式，线程安全：

	public class Singleton{
		private static Singleton instance;
		private Singletion(){}
		public static synchronized Singleton getInstance(){
			if(instance==null){
				instance = new Singletan();
			}
		} 
	}	

饿汉式(最常用)：

	public class Singleton{
		private static Singleton instance = new Singleton();
		private Singletion(){}
		public static Singleton getInstance(){
			return instance;
		} 
	}	

双重锁（DCL）：

	public class Singleton{
		private volatile static Singleton singleton;
		private Singletion(){}
		public static Singleton getInstance(){
			if(singleton==null){
				synchronized(Singleton.class){
					if(singleton == null){
						singleton = new Singleton();
					}
				}
			}
			return singleton;
		} 
	}	

## 二叉树的路径和（题号376）
### 问题描述
给定一个二叉树，找出所有路径中各节点相加总和等于给定 目标值 的路径。

一个有效的路径，指的是从根节点到叶节点的路径。
    1
    / \
   2   4
  / \
 2   3				
返回：
 [
  [1, 2, 2],
  [1, 4]
]

### 算法解答

读取树的路径最先想到的应该是树的遍历，这里的遍历方式是先序遍历，其次，最简单的就是使用DFS（深度优先遍历）。
DFS的遍历顺序为1->2->2->3->4,逻辑自然明了。
下面给出邻接矩阵模型类AMWGraph.java，其中depthFirstSearch(),broadFirstSearch
()分别代表深度优先和广度优先遍历。
	
	import java.util.ArrayList;
	import java.util.LinkedList;
	/**
	 * @description 邻接矩阵模型类
	 * @author beanlam
	 * @time 2015.4.17 
	 */
	public class AMWGraph {
	    private ArrayList vertexList;//存储点的链表
	    private int[][] edges;//邻接矩阵，用来存储边
	    private int numOfEdges;//边的数目

	    public AMWGraph(int n) {
	        //初始化矩阵，一维数组，和边的数目
	        edges=new int[n][n];
	        vertexList=new ArrayList(n);
	        numOfEdges=0;
	    }

	    //得到结点的个数
	    public int getNumOfVertex() {
	        return vertexList.size();
	    }

	    //得到边的数目
	    public int getNumOfEdges() {
	        return numOfEdges;
	    }

	    //返回结点i的数据
	    public Object getValueByIndex(int i) {
	        return vertexList.get(i);
	    }

	    //返回v1,v2的权值
	    public int getWeight(int v1,int v2) {
	        return edges[v1][v2];
	    }

	    //插入结点
	    public void insertVertex(Object vertex) {
	        vertexList.add(vertexList.size(),vertex);
	    }

	    //插入结点
	    public void insertEdge(int v1,int v2,int weight) {
	        edges[v1][v2]=weight;
	        numOfEdges++;
	    }

	    //删除结点
	    public void deleteEdge(int v1,int v2) {
	        edges[v1][v2]=0;
	        numOfEdges--;
	    }

	    //得到第一个邻接结点的下标
	    public int getFirstNeighbor(int index) {
	        for(int j=0;j<vertexList.size();j++) {
	            if (edges[index][j]>0) {
	                return j;
	            }
	        }
	        return -1;
	    }

	    //根据前一个邻接结点的下标来取得下一个邻接结点
	    public int getNextNeighbor(int v1,int v2) {
	        for (int j=v2+1;j<vertexList.size();j++) {
	            if (edges[v1][j]>0) {
	                return j;
	            }
	        }
	        return -1;
	    }
	    
	    //私有函数，深度优先遍历
	    private void depthFirstSearch(boolean[] isVisited,int  i) {
	        //首先访问该结点，在控制台打印出来
	        System.out.print(getValueByIndex(i)+"  ");
	        //置该结点为已访问
	        isVisited[i]=true;
	        
	        int w=getFirstNeighbor(i);//
	        while (w!=-1) {
	            if (!isVisited[w]) {
	                depthFirstSearch(isVisited,w);
	            }
	            w=getNextNeighbor(i, w);
	        }
	    }
	    
	    //对外公开函数，深度优先遍历，与其同名私有函数属于方法重载
	    public void depthFirstSearch() {
	        for(int i=0;i<getNumOfVertex();i++) {
	            //因为对于非连通图来说，并不是通过一个结点就一定可以遍历所有结点的。
	            if (!isVisited[i]) {
	                depthFirstSearch(isVisited,i);
	            }
	        }
	    }
	    
	    //私有函数，广度优先遍历
	    private void broadFirstSearch(boolean[] isVisited,int i) {
	        int u,w;
	        LinkedList queue=new LinkedList();
	        
	        //访问结点i
	        System.out.print(getValueByIndex(i)+"  ");
	        isVisited[i]=true;
	        //结点入队列
	        queue.addlast(i);
	        while (!queue.isEmpty()) {
	            u=((Integer)queue.removeFirst()).intValue();
	            w=getFirstNeighbor(u);
	            while(w!=-1) {
	                if(!isVisited[w]) {
		                //访问该结点
		                System.out.print(getValueByIndex(w)+"  ");
	                    //标记已被访问
	                    isVisited[w]=true;
	                    //入队列
	                    queue.addLast(w);
	                }
	                //寻找下一个邻接结点
	                w=getNextNeighbor(u, w);
	            }
	        }
	    }
	    
	    //对外公开函数，广度优先遍历
	    public void broadFirstSearch() {
	        for(int i=0;i<getNumOfVertex();i++) {
	            if(!isVisited[i]) {
	                broadFirstSearch(isVisited, i);
	            }
	        }
	    }
	}

这里给出另一种解法：
	
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
	     * @param root the root of binary tree
	     * @param target an integer
	     * @return all valid paths
	     */
	    public List<List<Integer>> binaryTreePathSum(TreeNode root, int target) {
	        // Write your code here
	        List<List<Integer>> list = new ArrayList<List<Integer>>();
	        if(root==null)
	            return list;
	        if(root.left==null&&root.right==null){
	            if(root.val==target){
	                 List<Integer> _list = new ArrayList<Integer>();
	                 _list.add(root.val);
	                 list.add(_list);
	            }
	            return list;
	       }
	       list.addAll(binaryTreePathSum(root.left,target-root.val));
	       list.addAll(binaryTreePathSum(root.right,target-root.val));
	        for(List<Integer> i :list)
	            i.add(0,root.val);
	           return list;    
	    }
	 }


## 字符串置换（题号211）
给定两个字符串，请设计一个方法来判定其中一个字符串是否为另一个字符串的置换。

置换的意思是，通过改变顺序可以使得两个字符串相等。
"abc" 为 "cba" 的置换。

"aabc" 不是 "abcc" 的置换。

	public class Solution {
    /*
     * @param A: a string
     * @param B: a string
     * @return: a boolean
     */
	    public boolean Permutation(String A, String B) {
	        // write your code here
	        int index =0;
	        if(A.length()==0&&B.length()==0)
	            return true;
	        if(A.length() == B.length()){
	        String _A[] = A.split("");
	        String _B[] = B.split("");
	        Map<String,Integer> mapA = new TreeMap<String,Integer>();
	        Map<String,Integer> mapB = new TreeMap<String,Integer>();
	        for(int i = 0;i<_A.length;i++){
	            Integer num_A = mapA.get(_A[i]);
	            Integer num_B = mapB.get(_B[i]);
	            mapA.put(_A[i],num_A==null?1:num_A+1);
	            mapB.put(_B[i],num_B==null?1:num_B+1);      
	        }
	        Iterator<String> iter = mapA.keySet().iterator();
	        while(iter.hasNext()){
	            String key = (String)iter.next();
	            if(!mapA.get(key).equals(mapB.get(key)))
	                break;
	            else
	                index++;
	        }
	        if(index==mapA.keySet().size())
	            return true;
	        else
	            return false;
	    }else
	        return false;
	    }
	}


## 回文数（题号491）
判断一个正整数是不是回文数。

回文数的定义是，将这个数反转之后，得到的数仍然是同一个数。
11, 121, 1, 12321 这些是回文数。

23, 32, 1232 这些不是回文数。

很简单，思想也很多，这里贴出其中一种算法：

	public class Solution {
    /*
     * @param num: a positive number
     * @return: true if it's a palindrome or false
     */
	    public boolean isPalindrome(int num) {
	        // write your code here
	        LinkedList<String> list = new LinkedList<String>();
	        String s = Integer.toString(num);
	        String _s[] = s.split("");
	        list.addAll(Arrays.asList(_s));
	        while(list.size()>1){
	            if(list.getFirst().equals(list.getLast())){
	                    list.removeFirst();
	                    list.removeLast();
	                }else
	                    break;;
	        }
	        if(list.size()>1)
	            return false;
	        else
	            return true;
	}
	}
