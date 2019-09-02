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
