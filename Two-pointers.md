
# (Easy)26. Remove duplicates from Sorted Array
```
Given nums = [1,1,2],

Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively.
```
* Solution
```Java
class Solution {
    public int removeDuplicates(int[] nums) {
        int i = 0;
        for(int j = 1; j < nums.length(); j++){
            if(!nums[i] == nums[j]){
                i++;
                nums[i] = nums[j];
            }//只需要前后比
        }//loop over the array
        
        return i+1;
    }
}
```

# (Easy) 27. Remove Element
* Similar to 26. Use fast and slow pointer.
* Solution 1
```Java
public int removeElement(int[] nums, int val) {
    int slow = 0;
    for(int fast = 1; fast < nums.length(); fast++){
        if(nums[fast] != val){
            nums[slow++] = nums[fast];
        }//not equal then slow ++ 
        fast++;
    }
    return slow;
}
```
* Solution 2
  * the order of those five elements can be arbitrary.
  * 1 2 2 6 4, val = 2. The first time we met a 2, we change it to 1 6 2 4 (move the last element 6 to the position). 
  The next 2 we do the same, move the last 4 to the position. We got 1 6 4 as the result.  
```Java
public int removeElement(int[] nums, int val) {
    int i = 0;
    int n = nums.length();
    while(i<n){
        if(nums[i] == val){
            nums[i] = nums[n-1];
            n--;
        }
        else{
            i++;
        }
    }
}
```

# (Easy)28. Implement strStr()
* different place to start: need one pointer i for haystack 
* compare each letter in haystack with the letter in neddle: need another pointer for neddle.
```Java
public int strStr(String haystack, String needle) {
    for(int i = 0; ;i++){
        for(int j = 0; ;j++){
            if(j == needle.length()) return i;
            if( i + j == haystack.length()) return -1;
            if(needle.charAt[j]) != haystack.charAt[i+j]) break; 
        }
    }        
}
```

# (Hard) 76. Minimum Window Substring
Sliding window的应用
```Java
public String minWindow(String s, String t) {
     int[] map = new int[128]; //why 128呢。。ASCII表一共128个
     for(int i =0; i < t.length; i++){
        map[t.charAt(i)] += 1;
     }
     int left = 0, right = 0;
     int ans_left = 0, ans_right = -1; //最后返回是用ans_right+1为了找不到时返回“”，这里先设为-1
     int ans_len = Integer.MAX_VALUE();
     int count = t.length();
     while(right < s.length()){
        map[s.charAt(right)] --；
        if(map[s.charAt(right)] >= 0) count --;
        while(count == 0){
            int temp_len = right - left + 1;
            if(temp_len < ans_len){
                ans_left = left;
                ans_right = right;
                ans_len = temp_len;
            }
            map[s.charAt(left)] ++;
            if(map[s.charAt(left)] > 0) count ++;
            left++;
        }
        right++;
     }
     return s.subString(ans_left, ans_right+1);
}
```
还挺好理解的   
