# (Hard) 32. Longest Valid Parentheses  
This is a dynammic programming.
## Preview: 20 Valid Parentheses (using stack)
* 遍历整个字符串，遇到左括号就入栈，然后遇到和栈顶对应的右括号就出栈，遍历结束后，如果栈为空，就表示全部匹配。    
* 新建Stack:  Stack<类型> brackets = new Stack<>();
* peek(): 看一下接下来要出栈的元素；
* push() and pop()...no need to explain
```Java
public boolean isValid(String s){
    Stack<Character> brackets = new Stack<>();
    for(int i = 0; i < s.length; i++){
        Char c = s.charAt(i);
        switch(c):{
            case '(':
            case '[':
            case '{':
              brackets.push(c);
              break;
            case ')':
              if(!brackets.empty()){
                  if(brackets.peek() == '('){
                      brackets.pop();
                  }else{
                      return false;
                  }
                  break;
            case ']':
                if(!brackets.empty()){
                      if(brackets.peek() == '['){
                          brackets.pop();
                      }else{
                          return false;
                      }
                      break;
              }
           case '}':
              if(!brackets.empty()){
                      if(brackets.peek() == '['){
                          brackets.pop();
                      }else{
                          return false;
                      }
                      break;
              }
           
        }
    }
    
    return brackets.empty();
}
```
如果只有一种括号，我们完全可以用一个计数器 count ，遍历整个字符串，遇到左括号加 1 ，遇到右括号减 1，遍历结束后，如果 count 等于 0 ，则表示全部匹配。但如果有多种括号，像 ( [ ) ] 这种情况它依旧会得到 0，所以我们需要用其他的方法。

## 本题
* only one kind of parenthesis, can use a counter.
* brute force using counter is easy to understand O(n²)
```Java
public int longestValidParentheses(String s) {
    int count = 0;
    int max = 0;
    for(int i = 0; i < s.length(); i++){
        count = 0;
        for(int j = i; j < s.length(); j++){
            if(s.charAt(j) == '('){
                count++;
            }else{
                count--;
            }
            if(count < 0){
                break;
            }//this is when has more ')' than '('
        }//for
        
        if(count == 0){
            if(j - i + 1 > max){
                max = j-i+1;
            }
        }
    }
    return max;
}
```
* dynammic programming
    * OPT(i) = the longest substring ended with index i   
    * i ended with '(' must be invalid, OPT(i) = 0
    * i ended with ')' and its left is '(', OPT(i) = OPT(i-2) + 2
    * i ended with ')' and its left is ')', 
        * if i - OPT(i-1) - 1 == '(', then valid, dp[i] = dp[i-1] + dp[i - dp[i-1] - 2] + 2
        * else, invalid, OPT(i) = 0
```Java
public int longestValidParentheses(String s) {
    int maxans = 0;
    int dp[] = new int[s.length()]; 
    for(int i = 1; i < s.length(); i++){
        if(s.charAt(i) == ')'){
            if(s.charAt(i-1) == '('){
                dp[i] = (i >= 2 ? dp[i-2]: 0) + 2; //pay attention to initialization
            }
            else if(i-dp[i-1] >0 && s.charAt(i-dp[i-1] - 1) == '('){
                dp[i] = dp[i - 1] + ((i-dp[i-1]) >= 2 ? dp[i-dp[i-1]-2] : 0) + 2;
            }
            //update the max value while doing the for loop (comparing max with dp[i])
            maxans = Math.max(maxans, dp[i]);
        }
    }
    return maxans;
}
```
* if I still want to use Stack?
    * if '(', push.
    * if ')', pop
        * if the stack is empty, pop(index) //index = current position
        * if not empty, length = index - index of pop
```Java
public int longestValidParentheses(String s) {
    int maxans = 0;
    Stack<Integer> stack = new Stack<>();
    stack.push(-1);
    for(int i = 0; i < s.length(); i++){
        if(s.charAt(i) == '('){
            stack.push(i);
        }else{
            stack.pop(); //能够先pop再判断就是因为那个-1！！！！
            if(stack.empty()){ //nothing matched, push the current position to stack
                stack.push(i);
            }else{
                maxans = Math.max(maxans, i - stack.peek());
            }
        }
    }
    return maxans;
}
```
* 神奇解法: space complexity to O(1)
    * 从左到右扫描，用两个变量 left 和 right 保存的当前的左括号和右括号的个数，都初始化为 0 。
        * 如果左括号个数等于右括号个数了，那么就更新合法序列的最长长度。
        * 如果左括号个数大于右括号个数了，那么就接着向右边扫描。
        * 如果左括号数目小于右括号个数了，那么后边无论是什么，此时都不可能是合法序列了，此时 left 和 right 归 0，然后接着扫描。
    * 从左到右扫描完毕后，同样的方法从右到左再来一次，因为类似这样的情况 ( ( ( ) ) ，如果从左到右扫描到最后，left = 3，right = 2，期间不会出现 left == right。但是如果从右向左扫描，扫描到倒数第二个位置的时候，就会出现 left = 2，right = 2 ，就会得到一种合法序列。
this is too smart...
```Java
public int longestValidParenthese(String s){
    int left = 0, right = 0, maxans = 0;
    for(int i = 0; i < s.length(); i++){
        if(s.charAt(i) == '(') left ++;
        else right++;
        if(left == right){
            maxans = Math.max(maxans, 2*right);
        }
        else if(right > left){
            left = right = 0;
        }
    }
    left = right = 0;
        for(int i = s.length()-1; i >= 0; i--){
        if(s.charAt(i) == '(') left ++;
        else right++;
        if(left == right){
            maxans = Math.max(maxans, 2*left); //注意这里以left为准
        }
        else if(left > right){
            left = right = 0;
        }
    }
    return maxans;
}
```
# (Medium) 33. 丢失
# (Medium) 34. Find First and Last Position of Element in Sorted Array
* 单边二分
* 左右开弓
```Java
public int[] searchRange(int[] nums, int target) {
    int start = 0;
    int end = nums.length()-1;
    int[] ans = {-1, -1};
    if(nums.length() == 0) return ans;
    
    while(start <= end){
        mid = (start + end)/2;
        if(nums[mid] == target){
            int n = mid > 0? nums[mid-1]:Integer.MIN_VALUE;
            if(n < target || mid == 0){
                ans[0] = mid;
                break;
            }
            end = mid - 1;
        }
        if(nums[mid] > target) end = mid - 1;
        if(nums[mid] < target) start = mid + 1;
        
    }//while
    
    //左边有，右边肯定也有
    start = 0;
    end = nums.length() - 1;
    
    while(start <= end){
        mid = (start + end)/2;
        if(nums[mid] == target){
            int n = mid > 0? nums[mid+1]:Integer.MAX_VALUE;
            if(n > target || mid == nums.length()-1){
                ans[1] = mid;
                break;
            }
            start = mid + 1;
        }
        if(nums[mid] > target) end = mid - 1;
        if(nums[mid] < target) start = mid + 1;
    }
    return ans;
}
```

# (Easy) 35. Search Insert Position
这道题比较简单，在二分查找的基础上，只要想清楚返回啥就够了。想的话，就考虑最简单的情况如果数组只剩下 2 5，target 是 1, 3, 6 的时候，此时我们应该返回什么就行。
* 二分法start<= end的话start更新mid+1, end更新mid -1.
* 二分法start< end的话，start更新mid+1， end更新mid
```Java
public int searchInsert(int[] nums, int target) {
        int start = 0;
        int end = nums.length()-1;
        if(nums.length() == 0) return 0;
        
        while(start < end){
            mid = (start + end)/2;
            if(target == nums[mid]) return mid;
            else if(target < nums[mid]) end = mid;
            else(target > nums[mid]) start = mid + 1;
        }
        if(target > nums[start]) return start + 1;
        else if(target < nums[end]) return start;
}
```
* 如何省略末尾的if判断
    * 用start <= end; 这样如果start = end的时候能多做一次判断，如果target < nums\[mid\]的话，end更新而start不变，return的值是start，else，start更新为mid+1
    * end = num.length(), 而非nums.length()-1，这样取mid的时候取到右端点。这样就可以放心return start了。
```Java
public int searchInsert(int[] nums, int target) {
    int start = 0;
    int end = nums.length - 1;
    if (nums.length == 0) {
        return 0;
    }
    while (start <= end) {
        int mid = (start + end) / 2;
        if (target == nums[mid]) {
            return mid;
        } else if (target < nums[mid]) {
            end = mid - 1;
        } else {
            start = mid + 1;
        }
    }

    return start;

}
```
```Java
public int searchInsert(int[] nums, int target) {
    int start = 0;
    int end = nums.length;
    if (nums.length == 0) {
        return 0;
    }
    while (start < end) {
        int mid = (start + end) / 2;
        if (target == nums[mid]) {
            return mid;
        } else if (target < nums[mid]) {
            end = mid;
        } else {
            start = mid + 1;
        }
    }

    return start;

}
```
# (Medium) 39. Combination Sum
* backtracking(回溯): 一般的架构就是一个大的 for 循环，然后先 add，接着利用递归进行向前遍历，然后再 remove ，继续循环。
```Java
public List<List<Integer>> combinationSum(int[] candidates, int target) {
    List<List<Integer>> res = new ArrayList<>();
    backtrack(res, new ArrayList<>, candidates, target, 0);
}

public void backtrack(List<List<Integer>> res, List<Integer> tempList, int[] candidates, int remain, int start){
    if(remain < 0) return;
    else if(remain == 0) res.add(new ArrayList<>(tempList));
    else{
        for(int i = start; i < candidates.length;i++){
            tempList.add(candidates[i]);
            backtrack(res, tempList, nums, remain - nums[i], i);
            //if the remain <0 or find a solution;
            tempList.remove(tempList.size()-1);
        }
    }
}
```
* dynammic programming (like knapsack problem with repetition) 
![dp](https://windliang.oss-cn-beijing.aliyuncs.com/39_1.jpg)
```Java
public List<List<Integer>> combinationSum(int[] candidates, int target) {
    List<List<List<Integer>>> res = new ArrayList<>();
    Array.sort(candidates);
    if(target < candidates[0]) return new ArrayList<Integer<Integer>>();
    
    for(int i = 0; i <= target; i++){
        List<List<Integer>> ans_i = new ArrayList<>();
        ans.add(i, ans_i);
    }
    
    for(int i = 0; i < candidates.length; i++){
        for(int sum = candidates[i]; sum <= target; sum++){
            List<List<Integer>> ans_sum = ans.get(sum);
            List<List<Integer>> ans_sub = ans.get(sum-candidates[i]);
        }
        //初始情况
        if(sum == candiates[i]){
            ArrayList<Integer> temp = new ArrayList<Integer>(); //这边的新建
            temp.add(nums[i]);
            ans_sum.add(temp);
        }
        if(ans_sub.size()>0){
            for(int j = 0; j < ans_sub.size(); j++){
                ArrayList<Integer> temp = new ArrayList<Integer>(ans_sub.get(j));
                temp.add(nums[i]);
                ans_sum.add(temp);
            }
        }
    }
    return ans.get(target);
}
```

# (Medium) 40. Combination Sum II 
* backtracking
* this is the knapsack without repetition.
* 也有人用DFS...

# (Hard) 42. Trapping Rain Water
* Use dynammic programming to find maxLeft and maxRight. O(n) and space is O(n)
* Use two pointers to save space, O(1)
* Use stack   
![illus](https://windliang.oss-cn-beijing.aliyuncs.com/42_10.jpg)   
```Java
//DP
public int trap(int[] height) {
    int sum = 0;
    int[] maxLeft = new int[height.length];
    int[] maxRight = new int[height.length];
    for(int i = 0; i < height.length; i++){
        maxLeft[i] = Math.max(maxLeft[i-1], height[i]);
    }
    
    for(int i = height.length-2; i >= 0; i--){
        maxRight[i] = Math.max(maxRight[i+1], height[i]);
    }
    
    for(int i = 0; i < height.length; i++){
        int min = Math.min(maxLeft[i], maxRight[i]);
        if(min < height[i]){
            sum = sum + (min - height[i]);
        }
    }
    return sum;
}
```
     
then how to use two pointers? constant space.
```Java
public int trap(int[] height) {
    int left = 1;
    int right = height.length -2;
    int maxLeft = 0;
    int maxRight = 0;
    int sum = 0;
    for(int i = 0; i < height.length; i++){
        if(heigh[left-1] < height[right+1]){
        //then left must be smaller than right
            maxLeft = Math.min(height[i], maxLeft);
            if(height[i] < maxLeft){
                sum = sum + (maxLeft - height[i]);
            }
            left++;
        }else{
            maxRight = Math.min(height[i], maxRight);
            if(height[i] < maxRight){
                sum = sum + (maxRight - height[i]);
            }
            right--;
        }
    }
    return sum;
}
```
      
Use stack, this is tricky!
```Java
public int trap6(int[] height) {
    int sum = 0;
    Stack<Integer> stack = new Stack<>();
    int current = 0;
    while(current < height.length){
        while (!stack.empty() && height[current] > height[stack.peek()]) {
            int h = height[stack.peek()];
            stack.pop();
            if(stack.empty()) break;
            int distance = current - stack.peek() - 1;
            int min = Math.min(height[stack.peek()], height[current]); //pop之后
            sum = distance * (min - h);
        }
        stack.push(current);
        current++;
    }
    return sum;
}
        
    }
}
```

# (Hard) 44. Wildcard
### 类似第十题，下面回忆一下第十题。    
* Recursion:   
    *  假定我们已经解决了问题，并且有了解决这个问题的函数。
    * 如何将问题从 n 的规模降到 n - 1 的规模。
    * 递归的出口是什么，也就是当问题规模已经降到最小的时候，我们如何解决它。
```Java
public boolean isMatch(String text, String pattern) {
    if (pattern.isEmpty()) return text.isEmpty();
    
    boolean first_match = (!text.isEmpty() && (text.charAt[0] == pattern.charAt[0] || text.charAt[0] == '.'));
    if(pattern.length >= 2 && pattern.charAt[1] == '*'){
        return ((isMatch(text, pattern.subString(2))) || (first_match && isMatch(text.subString(1), pattern)));
    }else{
        return(first_match && isMatch(test.subString(1), pattern.subString(1)));
    }
}

* DP
    * OPT[i][j] indicate the matching between text[i...] with pattern[j...]
```Java
public boolean isMatch(String text, String pattern) {
    // 多一维的空间，因为求 dp[len - 1][j] 的时候需要知道 dp[len][j] 的情况，
    // 多一维的话，就可以把 对 dp[len - 1][j] 也写进循环了
    boolean[][] dp = new boolean[text.length() + 1][pattern.length() + 1];
    // dp[len][len] 代表两个空串是否匹配了，"" 和 "" ，当然是 true 了。
    dp[text.length()][pattern.length()] = true;

    // 从 len 开始减少
    for (int i = text.length(); i >= 0; i--) {
        for (int j = pattern.length(); j >= 0; j--) {
            // dp[text.length()][pattern.length()] 已经进行了初始化
            if(i==text.length()&&j==pattern.length()) continue;

            boolean first_match = (i < text.length() && j < pattern.length()
                                   && (pattern.charAt(j) == text.charAt(i) || pattern.charAt(j) == '.'));
            if (j + 1 < pattern.length() && pattern.charAt(j + 1) == '*') {
                dp[i][j] = dp[i][j + 2] || first_match && dp[i + 1][j];
            } else {
                dp[i][j] = first_match && dp[i + 1][j + 1];
            }
        }
    }
    return dp[0][0];
}

```
* 优化空间，只需要知道i和i+1, 因此只需要申请OPT[2][pattern.length()+1]。两组数组轮换存储。
```Java
 public boolean isMatch(String text, String pattern) {
        // 多一维的空间，因为求 dp[len - 1][j] 的时候需要知道 dp[len][j] 的情况，
        // 多一维的话，就可以把 对 dp[len - 1][j] 也写进循环了
        boolean[][] dp = new boolean[2][pattern.length() + 1]; 
        dp[text.length()%2][pattern.length()] = true;

        // 从 len 开始减少
        for (int i = text.length(); i >= 0; i--) {
            for (int j = pattern.length(); j >= 0; j--) {
                if(i==text.length()&&j==pattern.length()) continue;
                boolean first_match = (i < text.length() && j < pattern.length()
                        && (pattern.charAt(j) == text.charAt(i) || pattern.charAt(j) == '.'));
                if (j + 1 < pattern.length() && pattern.charAt(j + 1) == '*') {
                    dp[i%2][j] = dp[i%2][j + 2] || first_match && dp[(i + 1)%2][j];
                } else {
                    dp[i%2][j] = first_match && dp[(i + 1)%2][j + 1];
                }
            }
        }
        return dp[0][0];
    }
```
### 本题是wildcard，和第十题\*的含义有所不同。
字符串匹配，? 匹配单个任意字符，* 匹配任意长度字符串，包括空串。     
* DP
```Java
不想填坑
```
* 迭代, 我觉得有点不好理解，看这里： https://leetcode.wang/leetCode-44-Wildcard-Matching.html
```Java
boolean isMatch(String str, String pattern) {
    int s = 0, p =0, starIdx = -1, match = 0;
    while(s < str.length()){
        if(p < pattern.length() && str.charAt(s) == pattern.charAt(p) || pattern.charAt(p) == '?'){
            p++;
            s++;
        }
        else if(p < pattern.length() && pattern.charAt(p) == "*"){
            starIdx = p;
            match = s;
            s++;
        }
        //用*匹配了一个字符
        else if(starIdx != -1){
            p = starIdx + 1;
            s = match + 1;
            match += 1;
        }
        else return false;
    }
    
    while(p<pattern.length() && pattern.charAt(p) == '*'){
        p++;
        
    }
    return p==pattern.length();
}
```

# （Hard）45. Jump Game 2

# (Medium) 46. Permutation
A general approach to backtracking questions in Java (Subsets, Permutations, Combination Sum, Palindrome Partioning)
![picture](https://windliang.oss-cn-beijing.aliyuncs.com/46_2.jpg)  
* 每调用一层就进入一个 for 循环，相当于列出了所有解，然后挑选了我们需要的。其实本质上就是深度优先遍历 DFS。     
        
https://leetcode.com/problems/permutations/discuss/18239/A-general-approach-to-backtracking-questions-in-Java-(Subsets-Permutations-Combination-Sum-Palindrome-Partioning
### 78. Subsets
* Given a set of distinct integers, nums, return all possible subsets (the power set).     
* Note: The solution set must not contain duplicate subsets.   
```
example:
Input: nums = [1,2,3]
Output:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```
```Java
public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> list = new ArrayList<>();
        Array.sort();
        backtrack(list, new ArrayList<>(), nums, 0);
        return list;
}

private void backtrack(List<List<Integer>> list , List<Integer> tempList, int[] nums, int start){
    list.add(new ArrayList<>(tempList)); //这样就可以保证每种长度的都会加进去
    for(int i = start; i < nums.length; i++){
        tempList.add(nums[i]);
        backtrack(list, tempList, nums, i + 1);
        tempList.remove(tempList.size() - 1);
     }
}
```

### 90. Subset 2 
Given a collection of integers that might contain duplicates, nums, return all possible subsets (the power set).   
Note: The solution set must not contain duplicate subsets.    
```
example:
Input: [1,2,2]
Output:
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]
```
```Java
public List<List<Integer>> subsetsWithDup(int[] nums) {
    List<List<Integer>> list = new ArrayList<>();
    Array.sort(nums);
    backtrack(list, new ArrayList<>(), nums, 0);
    return list;
}

public void backtrack(List<List<Integer>> list, List<Integer> tempList, int[] nums, int start){
    list.add(new ArrayList<>(tempList));
    for(i = start; i < nums.length(); i++){
        if(i > start && nums[i] == nums[i-1]) continue; //能做这种对比是因为sort过了
        tempList.add(nums[i]);
        backtrack(list, tempList, nums, i+1);
        temp.remove(tempList.size() - 1);
    }
}
```

### 46. Permutations
Given a collection of distinct integers, return all possible permutations.
```
Example:


Input: [1,2,3]
Output:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```
```Java
public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> list = new ArratList<>();
        backtrack(list, new ArrayList<>(), nums); //start is unnecessary
        return list;
}

public void backtrack(List<List<Integer>> list, List<Integer> tempList, int[] nums){
    if(tempList,length() == nums.length()){
        list.add(new ArrayList<>(tempList));
    }else{
        for(int i = 0; i < nums.length(); i++){
            if(tempList.contains(nums[i])) continue;
            tempList.add(nums[i]);
            backtrack(list, tempList, nums);
            tempList.remove(tempList.size()-1);
        }
    }
}
```

### 47. Permutations II (contains duplicates)
Given a collection of numbers that might contain duplicates, return all possible unique permutations.
```
Example:

Input: [1,1,2]
Output:
[
  [1,1,2],
  [1,2,1],
  [2,1,1]
]
```
```Java
public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> list = new ArratList<>();
        Arrays.sort(nums);
        backtrack(list, new ArrayList<>(), nums, new boolean[nums.length]); //start is unnecessary
        return list;
}


```

### 39. Combination Sum
* Given a set of candidate numbers (candidates) (without duplicates) and a target number (target), find all unique combinations in candidates where the candidate numbers sums to target.

* The same repeated number may be chosen from candidates unlimited number of times.
```
Input: candidates = [2,3,6,7], target = 7,
A solution set is:
[
  [7],
  [2,2,3]
]
Example 2:

Input: candidates = [2,3,5], target = 8,
A solution set is:
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]
```

### 40. Combination Sum 2

Given a collection of candidate numbers (candidates) and a target number (target), find all unique combinations in candidates where the candidate numbers sums to target.

Each number in candidates may only be used once in the combination.

Note:

All numbers (including target) will be positive integers.
The solution set must not contain duplicate combinations.
Example 1:
```
Input: candidates = [10,1,2,7,6,1,5], target = 8,
A solution set is:
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]
Example 2:

Input: candidates = [2,5,2,1,2], target = 5,
A solution set is:
[
  [1,2,2],
  [5]
]
```

# 53. (Easy) Maximum Subarray
* dynammic programming的话OPT[i] = 包括nums[i]的数组中的最大。OPT = max{nums[i], OPT[i-1]+nums[i]
```Java
public int maxSubArray(int[] nums) {
    int dp = nums[0];
    int max = nums[0];
    for(int i = 0; i < nums.length();i++){
        max = Math.max(nums[i], nums[i] + dp);
    }
    return max;
}
```

# (Hard) 128. Longest Consecutive Sequence
* 其实personally觉得不难，想到要用hash做了
* 第一种是用hash Set，是否contains，然后从最小的序列开始（也就是说这个数组里没有n-1了，只有n）, 竟然觉得有点难想
    * 趁机学一下hash set的语法
    * contains, add, remove, isEmpty, int size
* 第二种有点像DP(其实是Union-find)，所以放在DP里，但有些不同的是，OPT是更新在边界的，像是从中间向两边的结构。
```Java
public int longestConsecutive(int[] nums) {
    HashSet<Integer> set = new HashSet<>();
    for(int i = 0; i < nums.length; i++){
        set.add(nums[i]);
    }
    
    int max = 0;
    for(int i = 0; i < nums.length; i++){
        int num = nums[i];
        if(!set.contains(num-1)) {
            int count = 0;
            while(set.contains(num)){            
                count++;
                num++;
            }
            max = Math.max(max, count);
        }
    }
}
```
```Java
public int longestConsecutive(int[] nums) {
    HashMap<Integer, Integer> map = new HashMap<>();
    int max = 0;
    for(int i = 0; i < nums.length; i++){
        int num = nums[i];
        if(map.containsKey(num)){
            continue;
        }
        int left = map.getOrDefault(num-1, 0);
        int right = map.getOrDefault(num+1, 0);
        int sum = left + right + 1;
        
        map.put(num-left, sum);
        map.put(num+right, sum);
        map.put(num, -1);
    }
    return max;
}
```
# 221. Max Square
* 今天来找我玩耍的人好多，一整天都不知道在干什么。。。
* 这道题是一个一眼我能看出来的二维DP，无论从哪个角开始都可以，但我一开始的想法是OPT(i,j)直接存的是面积，高票答案们存的都是边长！！这非常巧妙了。
* 还有一个就是，string是length(), 数组是string，非callable。
* OPT(i,j)以i，j为右上顶点的
```Java
public int maximalSquare(char[][] matrix) {
     if(matrix.length == 0 || matrix[0].length == 0 || a == null){
        return 0;
     } 
     int row = matrix.length;
     int col = matrix[0].length;
     int max = 0;
     
     int[][] OPT = new int[row+1][col+1]; //多一行initial value, default都是0
     for(int i = 0; i < row; i++){
        for(int j = 0; j < col; j++){
            if(matrix[i][j] == '1'){
                OPT[i][j] = Math.min(OPT[i][j-1], OPT[i-1][j], OPT[i-1][j-1]) + 1;
                max = Math.math(max, OPT[i][j]);
            }
        }
     }
     return max*max;
}
```
