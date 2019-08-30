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

# (Hard) 30. Substring with concatenation of all words
```Java
public List<Integer> findSubstring(String s, String[] words) {
    List<Integer> res = new ArrayList<>();
    int wordNum = words.length;
    if(wordNum == 0){
        return res;
    }
     
    int wordLen = words[0].length;
    //create the first HashTable
    HashMap<String, Integer> allWords = new HashMap<>();
    for(string w: words){
        int value = allWords.getOrDefault(w, 0); //if no, then default = 0; else = get(w)
        allWords.put(w, value + 1);
    }
    //for all the position in s
    for(int i = 0; i < s.length() - wordLen*wordNum + 1; i++){
        HashMap<String, Integer> hasWords = new HashMap<>();
        int num = 0; //the number of words that are compatible with the 'words'
        
        //if has not find all the numbers
        while(num < wordNum){
            String word = s.substring(i + wordLen*num, i + wordLen*(num + 1));
            if(allWords.containsKey(word)){
                int value = hasWords.getOrDefault(word, 0);
                hasWords.put(word, value + 1);
                if(hasWords.get(word) >= allWords.get(word)){
                    break;
                }
            }else{
                break;
            }     
        }//while
        num++;
    }//for
    if(num == wordNum){
        res.add(i);
    }
}
return res;
```

* find a way to move the window
这个我不会    
https://leetcode.wang/leetCode-30-Substring-with-Concatenation-of-All-Words.html

# (Medium, Array)31. Next Permutation
Implement next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers.     
If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).    
The replacement must be in-place and use only constant extra memory.    
Analysis:    
* Just look from right to left, if there is one number to the right that is just the next smallest number other than the number we are looking into, permutate it. Then permutate the right part.  
* The following GIF may help:   
![GIF](https://windliang.oss-cn-beijing.aliyuncs.com/31_Next_Permutation.gif)
* need to helper function:
    * swap
```Java
public void swap(int[] nums, int i, int j){
    int temp = nums[j];
    nums[j] = nums[i];
    nums[i] = nums[j];
}
```
* reverse
```Java
public void reverse(int[] nums, int start){
    int i = start, j = nums.length -1;
    while(i<j){
        swap(nums, i, j);
        i++;
        j--;
    }
}
```
* solution (return void)
```Java
public void nextPermutation(int[] nums) {
    int i = nums.length - 2; //start from the second last position.
    //find the position where nums[i] < nums[i+1]
    while(i >= 0 && nums[i+1] > nums[i]){
        i--;
    }
    //if i = 0
    if(i<0){
        reverse(nums, 0);
        return;
    }
    
    //find the position that just bigger than nums[i]
    int j = nums.length - 1;
    while(j >= i && nums[j] <= nums[i]){
        j--;
    }
    //swap
    swap(nums, i, j);
    //reverse
    reverse(nums, i+1);
}
```
