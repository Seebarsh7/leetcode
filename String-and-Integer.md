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

