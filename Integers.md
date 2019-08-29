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

