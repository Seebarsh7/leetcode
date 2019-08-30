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
