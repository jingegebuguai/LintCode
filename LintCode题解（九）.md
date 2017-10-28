### Missing String

Given two strings, you have to find the missing string.

#### Example

Given a string str1 = This is an example
Given another string str2 = is example

Return ["This", "an"]

#### Programming

	public class Solution {
    /*
     * @param : a given string
     * @param : another given string
     * @return: An array of missing string
     */
	    public List<String> missingString(String str1, String str2) {
	        // Write your code here
	        if(str1.length()==0&&str2.length()==0)
	            return null;
	        List<String> list = new LinkedList<String>();
	        String[] _str1 = str1.split(" ");
	        String[] _str2 = str2.split(" ");
	        if(str2.length()!=0&&_str2.length==0)
	            return Arrays.asList(_str1);
	        for(int i=0;i<_str1.length;i++){
	            for(int j=0;j<_str2.length;j++){
	                if(_str1[i].equals(_str2[j]))
	                    break;
	                if(j==_str2.length-1&&!_str1[i].equals(_str2[j]))
	                    list.add(_str1[i]);
	            }
	        }
	        return list;
	    }
	};

### First Unique Number In Stream

Given a continuous stream of numbers, write a function that returns the first unique number whenever terminating number is reached(include terminating number). If there no unique number before terminating number or you can't find this terminating number, return -1.

#### Example
Given a stream [1, 2, 2, 1, 3, 4, 4, 5, 6] and a number 5
return 3

Given a stream [1, 2, 2, 1, 3, 4, 4, 5, 6] and a number 7
return -1

#### Programming

	public class Solution {
	    /*
	     * @param : a continuous stream of numbers
	     * @param : a number
	     * @return: returns the first unique number
	     */
	    public int firstUniqueNumber(int[] nums, int number) {
	        // Write your code here
	        if(nums.length==0)
	            return -1;
	        Map<Integer, Integer> map = new HashMap<Integer, Integer>();
	        int i = 0;
	        Integer index = 0;
	        while(nums[i] != number&&i < nums.length){
	            Integer _num = map.get(nums[i]);
	            map.put(nums[i], _num==null?1:_num+1);
	            i = i+1;
	            if(i==nums.length)
	                break;
	        }
	        if(i==nums.length)
	            return -1;
	        else{
	            Integer _num = map.get(nums[i]);
	            map.put(nums[i], _num==null?1:_num+1);
	        }
	        for(int j=0;nums[j]!=number;j++){
	            //index = map.get(nums[j]);
	            if(map.get(nums[j])==1){
	                index = nums[j];
	                
	                break;
	            }
	        } 
	        if(index==null&&map.get(number)!=1)
	            return -1;
	        else if(index==null&&map.get(number)==1)
	            return number;
	        return index;
	    }
	};


