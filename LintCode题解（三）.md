title: LintCode题解（三）
author: 惠惠
tags:
  - LintCode
categories:
  - 计算机
date: 2017-10-17 10:14:00
---

## 移动零
给一个数组 nums 写一个函数将 0 移动到数组的最后面，非零元素保持原数组的顺序

### 样例
给出 nums = [0, 1, 0, 3, 12], 调用函数之后, nums = [1, 3, 12, 0, 0].

### 解题过程

稍懂排序算法，这题没难点，低级冒泡就可以完成，但是越是简单，就越要考虑优化时间复杂度。这里我们尽量让时间复杂度为O(n)。

	public class Solution {
    /**
     * @param nums an integer array
     * @return nothing, do this in-place
     */
	  	public static void moveZeroes(int[] nums) {
	        // Write your code here
	        int pre = 0;
	        int tail = 0;
	        int[] array = new int[nums.length];
	        for(int i = 0;i < nums.length;i++){
	            if(nums[i]!=0) {
	                array[pre] = nums[i];
	                pre++;
	            } else {
	                array[nums.length - tail - 1] = 0;
	                tail++;
	            }
	        }
	        for(int j = 0;j < nums.length;j++)
	            nums[j] = array[j];
	    }
	}


## 玩具工厂
工厂模式是一种常见的设计模式。请实现一个玩具工厂 ToyFactory 用来产生不同的玩具类。可以假设只有猫和狗两种玩具。

### 样例

	ToyFactory tf = ToyFactory();
	Toy toy = tf.getToy('Dog');
	toy.talk(); 
	>> Wow

	toy = tf.getToy('Cat');
	toy.talk();
	>> Meow

### 解答过程
何为工厂模式，这是要求再熟悉不过的一种设计模式了。主要是定义一个创建对象的接口，让其子类自己决定实例化哪一个工厂类，工厂模式使其创建过程延迟到子类进行。主要解决接口的选择问题。
	
	/**
	 * Your object will be instantiated and called as such:
	 * ToyFactory tf = new ToyFactory();
	 * Toy toy = tf.getToy(type);
	 * toy.talk();
	 */
	interface Toy {
	    void talk();
	}

	class Dog implements Toy {
	    // Write your code here
	    public void talk(){
	        System.out.println("Wow");
	    }
	}

	class Cat implements Toy {
	    // Write your code here
	    public void talk(){
	        System.out.println("Meow");
	    }
	}

	public class ToyFactory {
	    /**
	     * @param type a string
	     * @return Get object of the type
	     */
	    public Toy getToy(String type) {
	        // Write your code here
	        if(type==null)
	            return null;
	        if(type.equals("Dog"))
	            return new Dog();
	        else if(type.equals("Cat"))
	            return new Cat();
	        else
	            return null;
	    }
	}


## 左填充
实现一个leftpad库，如果不知道什么是leftpad可以看样例。

### 样例
	
	leftpad("foo", 5)
	>> "  foo"

	leftpad("foobar", 6)
	>> "foobar"

	leftpad("1", 2, "0")
	>> "01"

注意字符串可以用“+”相连接，字符转化为字符串，用String.valueOf(),或者Character.toString()即可，其他没有逻辑点：

	public class StringUtils {
    /**
     * @param originalStr the string we want to append to with spaces
     * @param size the target length of the string
     * @return a string
     */
	    static public String leftPad(String originalStr, int size) {
	        // Write your code here
	        if (originalStr.length() > size)
	            return originalStr;
	        String result = "";
	        for (int i = 0; i < size - originalStr.length(); i++)
	            result = result + " ";
	        result = result + originalStr;
	        return result;
	    }

	    /**
	     * @param originalStr the string we want to append to
	     * @param size the target length of the string
	     * @param padChar the character to pad to the left side of the string
	     * @return a string
	     */
	    static public String leftPad(String originalStr, int size, char padChar) {
	        // Write your code here
	        if(originalStr.length() > size)
	            return originalStr;
	        String result = "";
	        //String c = String.valueOf(padChar);
	        String c = Character.toString(padChar);
	        for (int i = 0; i < size - originalStr.length(); i++)
	            result = result + c;
	        result = result + originalStr;
	        return result;
	    }
	}

## 丑数
写一个程序来检测一个整数是不是丑数。

丑数的定义是，只包含质因子 2, 3, 5 的正整数。比如 6, 8 就是丑数，但是 14 不是丑数以为他包含了质因子 7。

### 样例
给出 num = 8，返回 true。
给出 num = 14，返回 false。

### 解体过程
使用递归

	public class Solution {
    /*
     * @param num: An integer
     * @return: true if num is an ugly number or false
     */
	    public boolean isUgly(int num) {
	        // write your code here
	        if (num == 0) return false;
	        else if (num == 1) return true;
	        else if (num % 2 == 0)
	            return isUgly(num / 2);
	        else if (num % 3 == 0)
	            return isUgly(num / 3);
	        else if (num % 5 == 0)
	            return isUgly(num / 5);
	        else return false;    
	    }
	} 	