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
# (Medium) 36. Valid Sudoku
Determine if a 9x9 Sudoku board is valid. Only the filled cells need to be validated according to the following rules:

* Each row must contain the digits 1-9 without repetition.
* Each column must contain the digits 1-9 without repetition.
* Each of the 9 3x3 sub-boxes of the grid must contain the digits 1-9 without repetition.
遍历也只需要O(n), 3遍是3n，空间只要O(1)    
Use HashSet 规定格式:
 * 如果第 4 列有一个数字 8，我们就 (8)4，把 "(8)4"放进去。
 * 如果第 5 行有一个数字 6，我们就 5(6)，把 "5(6)"放进去。
 * 小棋盘看成一个整体，总共是 9 个，3 行 3 列，如果第 2 行第 1 列的小棋盘里有个数字 3，我们就把 "2(3)1" 放进去。
The Java.util.HashSet.add() method in Java HashSet is used to add a specific element into a HashSet. This method will add the element only if the specified element is not present in the HashSet else the function will return False if the element is already present in the HashSet.   

* Syntax: Hash_Set.add(Object element)
* Parameters: The parameter element is of the type HashSet and refers to the element to be added to the Set.
* Return Value: The function returns True if the element is not present in the HashSet otherwise False if the element is already present in the HashSet.
```Java
public boolean isValidSudoku(char[][] board) {
        Set seen = new HashSet();
        for(int i = 0; i < 9; i++){
            for(int j = 0; j < 9; j++){
                char number = board[i][j];
                if(number != '.'){
                    if(!seen.add(number + 'in row ' + i) || 
                    !seen.add(number + ' in col ' + j) ||
                    !seen.add(number + ' in block' + i/3 + '-' + j/3)){
                        return false;
                    }
                }
            }
        }
        return true;
}
```

# (Hard) 37. Sudoku Solver
经典回溯法：从上到下，从左到右遍历每个空位置。在第一个位置，随便填一个可以填的数字，再在第二个位置填一个可以填的数字，一直执行下去直到最后一个位置。期间如果出现没有数字可以填的话，就回退到上一个位置，换一下数字，再向后进行下去。
* helper function: isValid, 这个位置上填这个数行不行？
```Java
private boolean isValid(int row, int col, char[][] board, char c) {
    for(int i = 0; i < 9; i++){
        if(board[row][i] == c){
            return false;
        }
    }
    for(int i = 0; i < 9; i++){
        if(board[col][i] == c) return false;
    }
    
    int start_row = row/3 * 3;
    int start_col = col/3 * 3;
    for(int i = 0; i < 3; i++){
        for(int j = 0; j < 3; j++){
            return !(board[start_row+i][start_col+j] == c);
        }
    }
    return true;
}
```
主要：
```Java
public void solveSudoku(char[][] board) {
    solver(board);        
}

private boolean solver(char[][] board) {
    for(int i = 0; i < 9; i++){
        for(int j = 0; j <9){
            if(board[i][j] == '.'){
                char count == '1';
                while(count < 9){
                 if(isValid(i,j, board, count)){
                     board[i][j] = count;
                     if(solver(board)){
                         return true;
                     }else{
                         board[i][j] = '.';
                     }
                 }
                 count ++;
                }
                return false;
             }
        }
    }
    return true;
}
```
