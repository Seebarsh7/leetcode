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
