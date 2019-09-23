# (Easy) 38. Count and Say
* 初始值第一行是 1。

* 第二行读第一行，1 个 1，去掉个字，所以第二行就是 11。

* 第三行读第二行，2 个 1，去掉个字，所以第三行就是 21。

* 第四行读第三行，1 个 2，1 个 1，去掉所有个字，所以第四行就是 1211。

* 第五行读第四行，1 个 1，1 个 2，2 个 1，去掉所有个字，所以第五航就是 111221。

* 第六行读第五行，3 个 1，2 个 2，1 个 1，去掉所以个字，所以第六行就是 312211。

* 然后题目要求输入 1 - 30 的任意行数，输出该行是啥。
     
      
递归里套递归，which is interesting
* 第一个helper，计算重复数字的次数
```Java
private int getRepeatNum(String string) {
    int count = 0;
    char same = string.charAt(0);
    for(int i = 0; i<string.length();i++){
        if(string.charAt(i) == same){
            count ++;
        }else{
            break;
        }
    }
    return count;
}
```
第二个helper，迭代用来数个数格式化输出的
```Java
private String getNextString(String last) {
    if(last.length() == 0){
        return "";
    }
    int num = getRepeatNum(last);
    return num + "" + last.charAt(0) + getNextString(last.subString(num));
}
```
   
主要的也是迭代，n由n-1推出
```Java
public String countAndSay(int n) {
    if(n == 1) return "1";
    String last = countAndSay(n-1);
    return getNextString(last);     
}
```
# (Medium) 29. Divide two Integers
* https://leetcode.wang/leetCode-29-Divide-Two-Integers.html   
* 4位32 bits: -2147483648 到 2147483647   
* 补码: 各位取反（~x），末位+1
  * 我们的算法开始的部分是不管三七二十一，通通转换成正数，而出现 -2147483648 的时候，它无法转换成正数，我们怎么该解决呢？   
* dividend - divisor加速：每次翻倍减
  * 我们每次减 1 次除数，我们其实可以每次减多次。比如 10 / 1 ，之前是 10 - 1 = 9，计数器加 1 变成 1，然后 9 - 1 = 8，
  计数器加 1 变成 2，然后 8 - 1= 7，计数器加 1 变成 3，直至减到 0 < 1，我们结束了循环。我们其实可以翻倍减， 
  减完 1 ，减 2 ，再减 4 ，在减 8，当然计数器也不能只加 1 了，减数是翻倍减的，所以计数器也会一直翻倍的加。
  这里肯定会遇到一个问题，比如 10 - 1 = 9，9 - 2 = 7，7 - 4 = 3，3 - 8 = -5 < 1，它就走出了 while 循环。
  但是 3 本来还可以减 3 次 1，所以我们只要再递归就可以了。再看 3 / 1 的商，然后把之前的计数器的值加上 3 / 1 的商就够了。
```Java
public long opposite(long x) {
    return ~x + 1;
}
```
Solution: turn all the positive value to negative   
```Java
public int divide(int dividend, int divisor) {
    int ans = -1;
    int sign = 1;
    
    if(dividend > 0){
        sign = opposite(sign);
        dividend = opposite(dividend);
    }
    
    if(divisor > 0){
        sign = opposite(sign);
        divisor = opposite(divisor);
    }
    
    //store the current dividend and divisor as we are going to change them in the while loop
    int origin_dividend = dividend;
    int origin_divisor = divisor;
    if(divisor > dividend){
        return 0;
    }
    
    //do the operation via 减法
    dividend -= divisor;
    while(dividend >= divisor){
        and = ans + ans; //counter
        divisor += divisor;
        dividend -= divisor;
    }
    
    //此时我们传进的是两个负数，正常情况下，它就返回正数，但我们是在用负数累加，所以要取相反数
    int a = ans + opposite(divide(origin_dividend - divisor), origin_divisor);
    if(a == Integer.MIN_VALUE){
        if(sign == 1){
             return Integer.MAX_VALUE;
        }
        else{
            return Integer.MIN_VALUE;
        }
    }else{
        if(sign == 1){
            return opposite(a);
        }
        else{
            return a;
        }
    }
}
```

# (Medium) 43. Multiply Strings
大数相加：
```
首先每一位相加肯定会产生进位，我们用 carry 表示。进位最大会是 1 ，因为最大的情况是无非是 9 + 9 + 1 = 19 ，也就是两个最大的数相加，再加进位，这样最大是 19 ，不会产生进位 2 。下边是伪代码。

初始化一个节点的头，dummy head ，但是这个头不存储数字。并且将 curr 指向它。
初始化进位 carry 为 0 。
初始化 p 和 q 分别为给定的两个链表 l1 和 l2 的头，也就是个位。
循环，直到 l1 和 l2 全部到达 null 。
设置 x 为 p 节点的值，如果 p 已经到达了 null，设置 x 为 0 。
设置 y 为 q 节点的值，如果 q 已经到达了 null，设置 y 为 0 。
设置 sum = x + y + carry 。
更新 carry = sum / 10 。
创建一个值为 sum mod 10 的节点，并将 curr 的 next 指向它，同时 curr 指向变为当前的新节点。
向前移动 p 和 q 。
判断 carry 是否等于 1 ，如果等于 1 ，在链表末尾增加一个为 1 的节点。
返回 dummy head 的 next ，也就是个位数开始的地方。
初始化的节点 dummy head 没有存储值，最后返回 dummy head 的 next 。这样的好处是不用单独对 head 进行判断改变值。也就是如果一开始的 head 就是代表个位数，那么开始初始化的时候并不知道它的值是多少，所以还需要在进入循环前单独对它进行值的更正，不能像现在一样只用一个循环简洁。
```

```Java
public String multiply(String num1, String num2) {
    if (num1.equals("0") || num2.equals("0")) {
        return "0";
    }
    int n1 = num1.length();
    int n2 = num2.length();
    int[] pos = new int[n1 + n2]; //保存最后的结果
    for (int i = n1 - 1; i >= 0; i--) {
        for (int j = n2 - 1; j >= 0; j--) {
            //相乘的结果
            int mul = (num1.charAt(i) - '0') * (num2.charAt(j) - '0');
            //加上 pos[i+j+1] 之前已经累加的结果
            int sum = mul + pos[i + j + 1];
            //更新 pos[i + j]
            pos[i + j] += sum / 10;
            //更新 pos[i + j + 1]
            pos[i + j + 1] = sum % 10;
        }
    }
    StringBuilder sb = new StringBuilder();
    for (int i = 0; i < pos.length; i++) {
        //判断最高位是不是 0 
        if (i == 0 && pos[i] == 0) {
            continue;
        }
        sb.append(pos[i]);
    }
    return sb.toString();
}
```

# (Easy)157. Read N Characters Given Read4
* 这道题真的太难懂了。。。

how read4 works???
```
The API read4 reads 4 consecutive characters from the file, then writes those characters into the buffer array buf.   
The return value is the number of actual characters read.    
   
File file("abcdefghijk"); // File is "abcdefghijk", initially file pointer (fp) points to 'a'
char[] buf = new char[4]; // Create buffer with enough space to store characters
read4(buf); // read4 returns 4. Now buf = "abcd", fp points to 'e'
read4(buf); // read4 returns 4. Now buf = "efgh", fp points to 'i'
read4(buf); // read4 returns 3. Now buf = "ijk", fp points to end of file
```
大概就是n是我要读的数字，比如leetcode我要利用read4读五个数，那真实读了的就是leetc，而abc若是我要用read4读4个数，我实际只能读到abc，所以实际读了3个。   

```Java
/**
 * The read4 API is defined in the parent class Reader4.
 *     int read4(char[] buf); 
 */
public class Solution extends Reader4 {
    /**
     * @param buf Destination buffer
     * @param n   Number of characters to read
     * @return    The number of actual characters read
     */
    public int read(char[] buf, int n) {
        int total = 0;
        boolean eof = false;
        char[] tmp = new char[4];
        //total < n 的意思是现在read4读的数还没有超过要求
        while(!eof && total < n){
             int count = read4(tmp); //count是目前read4的actual_read
             eof = (count < 4);
             count = Math.min(count, n-total);
             for(int i = 0; i < count; i++){
                    //first use and then ++
                    buf[total] = tmp[i];
                    total++;
             }
        }
        return total;
    }
}
```

# (Hard)  158. Read N Characters Given Read4 II - Call multiple times
* 这道题的难点在于题目。。。很难理解。但有大佬提出来说这个其实在各种hardware里是非常常见的数据读取
* 这里我可以read很多次，看一个例子
```
File file("abc");
Solution sol;
// Assume buf is allocated and guaranteed to have enough space for storing all characters from the file.
sol.read(buf, 1); // After calling your read method, buf should contain "a". We read a total of 1 character from the file, so return 1.
sol.read(buf, 2); // Now buf should contain "bc". We read a total of 2 characters from the file, so return 2.
sol.read(buf, 1); // We have reached the end of file, no more characters can be read. So return 0.
```
* 我认为我需要三个指针和一个temperory buffer
     * ptr, 新数组我写到哪里了？是不是达到了题目中n的要求
     * buffer pointer: 目前这个read4的结果我写到哪里了？如果写到了尽头，那么我就可以reset buffer pointer =0，然后读下一个read4了
     * buffer counter： 目前这个buffer我的有效读取的字母有多少个？如果buffer pointer = buffer counter（也就是尽头）的话，我就可以把buffer pointer = 0了。
```Java
private int buffPointer = 0;
private int buffCounter = 0;
private char[] tmp = new char[4];
public int read(char[] buf, int n) {
     int ptr = 0;
     while(ptr < n){
          if(buffPointer == 0) buffCounter = read4(tmp);
          while(buffPointer < buffCounter && ptr < n){
               buf[ptr] = tmp[buffPointer];
               ptr++;
               buffPointer++;
          }
          if(buffPointer == buffCounter) buffPointer = 0;
          if(buffCounter < 4) break;
     }
     return ptr;
}
```
* Google follow up: how to save space without temp
```
Because using inner buffer(buf4) can introduce more memory copy operation, which is very time-consuming.

We need to copy characters from the file directly when there is enough space in the buffer.

When the buffer size is not enough, we first copy the 4B characters to inner buf4. Then we copy from buf4.

buffer: |-- 4B --| -- 4B --| - tail -|. Tail segment may be smaller than 4B.
```
* Scene 1
```
  buf4: || empty
buffer: |--- 4B ---|--- 4B ---|- tail -|
  file: |--- 4B ---|--- 4B ---|--- 4B ---|--- 4B ---|---|
```
* 2
```
buf4: || empty
buffer: |--- 4B ---|--- 4B ---|- tail -|
  file: |--- 4B ---|--- 4B ---|--|
  ```
* Scenario 3:
```
            i4     n4
  buf4: |--- 4B ---|
buffer: |--- 4B ---|--- 4B ---|- tail -|
  file: |--- 4B ---|--- 4B ---|--|
```
* Scenario 4:
```
            i4     n4
  buf4: |--- 4B ---|
buffer: |--- 4B ---|--- 4B ---|- tail -|
  file: |--- 4B ---|--- 4B ---|--- 4B ---|--- 4B ---|---|
```
* The rule is

     * whenever there is content in the buf4, copy from buf4
     * whenever there is enough space in buffer, copy from file
# 163. Missing Ranges
* My solution is wrong with case
[] lower = 1, upper = 1    
should return "1" but my program returns []   
* Therefore it is worth thinking how the problem is defined: the lower and upper could appear in the middle of the nums array
* String could be printed as formatted
```Java
String getRange(int n1, int n2){
     return (n1 == n2)?String.ValueOf(n1) : String.format("%d->%d", n1, n2);
}
```
* only need a pointer next, kind of like what is the current lower bound that I need to print out?
```Java
     int next = lower;
     for(int i = 0; i < nums.length; i++){
          if(nums[i] < next) continue;
          if(nums[i] == next){
               next++;
               continue;
          }
    
          res.add(getRange(next, nums[i] - 1));
          next = a[i] + 1; //the next lower bound
               
          }
          //a final check
           if(next <= upper) res.add(getRange(next, upper));
           
           return res;
```
* the idea is basically exchanging 2 pointers
```Java
int prev = lower - 1;
for(int i = 0; i <= nums.length; i++){
     int after = i ==nums.length? upper+1:nums[i];
     if(prev+2 == after)
          ans.add(String.ValueOf(prev+1));
     else if(prev+2<after){
          ans.add...
     }
     prev = after;
}
```
# 205. Isomorphic Strings
* 这是一道easy题。。。我的第一想法就是hashmap，使两个string一一对应
* 万万没想到第一次test case倒在ab和aa上，因为a map a的时候，第二个b还是会默认是一个新的key，map a没有任何毛病
* 于是我改了一下，让key不在map里的时候，寻找一下对应另一个string的character是否已经是key了
* 造成了一个新错误就是baa和cfa，因为这个时候我的代码没办法可以f map a，但此时a并不是key
* 所以重点就在于建立一个双向的map！！但又不能是一个一对一的关系，因为可能会产生多对一。。。
* 所以我submit了四次。。。全部fail，看看别人怎么做的吧： 用一个256的int数组，instead of map
     * if use HashMap, use containsValue: 
```
     The java.util.HashMap.containsValue() method is used to check whether a particular value is being mapped by a single or more than one key in the HashMap.
```

# 246. Strobogrammatic Number
* 5 condition (0,0) (1,1) (6,9), (9,6), (8,8), using hashmap or condition case
* using two pointers!!!!
    
下面是Stfan大佬的解法，我怀疑我的智商
```Java
for(int i = 0; j = num.length() -1; i <= j; i++, j--){
     if(!"00 88 11 696".contains(num.charAt(i) + "" + num.charAt(j))) return false;
}
return true;
```
真是打扰了。。。     

# 247. Strobogrammatic Number II    
* 找出某个长度的所有这种数，返回一个字符串array
* 来吧让我们，recursion
```Java

```
