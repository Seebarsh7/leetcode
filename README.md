# 1. Easy: Two Sum
Given an array of integers, return indices of the two numbers such that they add up to a specific target.   
You may assume that each input would have exactly one solution, and you may not use the same element twice.   
* Input: integer array nums; int target;   
* Output: the index of two numbers
* This is an example of subset sum. 
While we only have two numbers. Therefore we could use a hash map to store the target we are trying to find in the map and traverse the list only once.   
* 学习： HashMap的语法
  * 新建： Map<key, value> = new HashMap<>()；
  * 查找: map.containsKey(key)
  * 返回key: map.get(key)
```Java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> res = new HashMap<>();
        
        for(int i = 0; i < nums.length; i++){
          int tar = target - nums[i];
          
          if(res.containsKey(tar) && res.get(tar) != i){
            return new int[]{i, res.get(tar)}
          }//if in the list
          
          else{
            map.put(nums[i], i) //nums[i]是key，这样才可以chazhoa
          }//not in the list
          
        }//loop the list
        
        throw new IllegalArgumentException("no sum"); //没有return值怎么办，报exception
    }
}
```

# 15. Medium: 3Sum
* a+b+c = 0
* Input: interger list nums
* output: List<List<Integer>> 
* Example:
 ```
  Given array nums = [-1, 0, 1, 2, -1, -4],

  A solution set is:
  [
    [-1, 0, 1],
    [-1, -1, 2]
  ]
 ```
```Java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        
    }
}
```
