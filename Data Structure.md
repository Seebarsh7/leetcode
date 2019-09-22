# (Hard) 23. Merge k Sorted Lists
## Solution 1:    
Staright Forward, loop through all the elements storing in an array. Then turn the array into a linked list.   
O(nlgn)   
```Java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        List<Integer> l = new ArrayList<Integer>();
        //loop over all the lists
        for(ListNode ln: lists){
            while(ln != null){
                l.add(ln.val);
                ln = ln.next;
             } //while
        }//for
        
        //sort the array
        collections.sort(l); //quick sort
        
        //turn array into list
        ListNode head = new ListNode(0);
        ListNode h = head;
        for(int i: l){
            h.next = new ListNode(i);
            h = h.next;
        }
        h.next = null;
        return head.next;
    }
}
```

## Solution 2: PQ
* Compare every first element for every list. Compare takes O(n) but using a priority queue takes O(lgn).   
* Java PriorityQueue and method comparator. If want to compare ListNode, need to override compare() method.   
* O(nlgk)
```Java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        Comparator<ListNode> cmp;
        cmp = new Comparator<ListNode>() {  
            @Override
            public int compare(ListNode o1, ListNode o2) {
                return o1.val-o2.val;
            }//compare
        }; // new comparator
        
        Queue<ListNode> q = new PriorityQueue<ListNode>(cmp); //size of the PQ is k;
        for(ListNode l: lists){
            if(l!= null){
                q.add(l);
            }
        }//add the first node to the PQ
        ListNode head = new ListNode(0);
        ListNode h = head;
        while(!q.isEmpty()){
            h.next = q.poll(); 
            h = h.next;
            
            ListNode next = h.next; 
            if(!next = null){ //until it reaches the end of the list (the last node)
                q.add(next);
            }
        }//loop through the queue until it it empty
        return head.next;
    }
}
```

## (New)Solution 3: 
* using the method of merging two sorted list -> 21. Merge two sorted list
* Preview of 21   
  * Iteration
```Java
public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    ListNode h = new ListNode(0);
    ListNode ans=h;
    while (l1 != null && l2 != null) {
        if (l1.val < l2.val) {
            h.next = l1;
            h = h.next;
            l1 = l1.next;
        } else {
            h.next = l2;
            h = h.next;
            l2 = l2.next;
        }
    }
    if(l1==null){
        h.next=l2;
    }
    if(l2==null){
        h.next=l1;
    } 
    return ans.next;
}
```
  * Recursion
```Java
ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    if(l1 == null) return l2;
    if(l2 == null) return l1;

    if(l1.val < l2.val) {
        l1.next = mergeTwoLists(l1.next, l2);
        return l1;
    } else {
        l2.next = mergeTwoLists(l2.next, l1);
        return l2;
    }
}
```
* Merge the first and second, and then merge the result with the third...
```Java
public ListNode mergeKLists(ListNode[] lists) {
    if(lists.length==1){
        return lists[0];
    }
    if(lists.length==0){
        return null;
    }
    ListNode head = mergeTwoLists(lists[0],lists[1]);
    for (int i = 2; i < lists.length; i++) {
        head = mergeTwoLists(head,lists[i]);
    }
    return head;
}
```
* Optimization
```Java
public ListNode mergeKLists(ListNode[] lists) {
    if(lists.length==0){
        return null;
    }
    int interval = 1;
    while(interval<lists.length){
        System.out.println(lists.length);
        for (int i = 0; i + interval< lists.length; i=i+interval*2) {
            lists[i]=mergeTwoLists(lists[i],lists[i+interval]);            
        }
        interval*=2;
    }

    return lists[0];
}
```
# (Medium) 24. Swap Nodes in Pairs
* Given a linked list, swap every two adjacent nodes and return its head.
* You may not modify the values in the list's nodes, only nodes itself may be changed.
* Swapping the pointers of two ListNode
    
* Iteration
```Java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode point = dummy; //相当于一个pointer
        while(point.next != null && point.next.next != null){
            swap1 = point.next;
            swap2 = point.next.next;
            point.next = point.next.next;
            swap2.next = swap1;
            swap1.next = swap2.next;
        }
        return dummy.next;
    }
}
```
* Recursion(永远不会)
head 和head.next最后交换位置
```Java
public ListNode swapPairs(ListNode head) {
    if ((head == null)||(head.next == null))
        return head;
    ListNode n = head.next;
    head.next = swapPairs(head.next.next);
    n.next = head;
    return n;
}
```

# (Hard)25. Reverse Nodes in k-Group

# (Easy)155. Min-Stack 
```
push(x) -- Push element x onto stack.
pop() -- Removes the element on top of the stack.
top() -- Get the top element.
getMin() -- Retrieve the minimum element in the stack.
```
* 这居然是个easy题，难在题目意思，没有想到要用Java自带的数据结构来做。
* 其实就是keep track of the min, and keep other operations the same as Stack.
* I think there are two problems:
    * how to keep the old min value when pop the current min value
    * how to keep track of the min value
* There is a very smart idea to store the gap between current min value and the upcoming x.
* Some trivial thing: using long as the data type, need cast
```
The problem for my solution is the cast. I have no idea to avoid the cast. Since the possible gap between the current value and the min value could be Integer.MAX_VALUE-Integer.MIN_VALUE;
```
```Java
class MinStack {
    long min;
    Stack<Long> stack;
    /** initialize your data structure here. */
    public MinStack() {
       stack = new Stack
    }
    
    public void push(int x) {
        if(stack.isEmpty){
            stack.push(0L);
        }else{
            stack.push(x-min);
            if(x<min) min = x;
        }
    }
    //what is amazing is that pop keeps the same behaviour as stack, 
    //but we need to keep track of the old min and current min
    public void pop() {
        if(stack.isEmpty) return;
        long pop = stack.pop();
        if(num < 0) min = min - pop; //(pop = x - min, min = x-pop, x is the current min)
    }
   
    public int top() {
        if(stack.peek()>0) return (int)(stack.peek() + min);
        else return (int)min; //this is the second place that I feel amazed, this means that min is updated to x, to the current min.
    }
    
    public int getMin() {
        return (int)min;    
    }
}
```
# (Hard) 208. Implement Trie (Prefix Tree)
https://leetcode.com/articles/implement-trie-prefix-tree/
* Insert: Inserts a word into the trie.
* Search: Returns if the word is in the trie.
* Start with: Returns if there is any word in the trie that starts with the given prefix.
从建一颗树入手
```Java
class TrieNode {
    public boolean isWord;
    public TrieNode[] children = new TrieNode[26];
    public TrieNode(){}
}
class Trie {

    /** Initialize your data structure here. */
    public Trie() {
        public TrieNode root;
        public Trie(){
            root = new TrieNode();
        }
    }
    
    /** Inserts a word into the trie. */
    public void insert(String word) {
        TrieNode ws = root;
        for(int i = 0; i < word.length(); i++){
            char c= word.charAt(i);
            if(ws.children[c - 'a'] == null){
                ws.children[c - 'a'] = new TrieNode();
            }
            ws = ws.children[c - 'a'];
   
        }
        ws.isWord = true;
    }
    
    /** Returns if the word is in the trie. */
    public boolean search(String word) {
        TrieNode ws = root;
        for(int i = 0; i < word.length(); i++){
            char c= word.charAt(i);
            if(ws.children[c-'a'] == null) return false;
            ws = ws.children[c - 'a'];
        }
        return ws.isWord; //the last node
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    public boolean startsWith(String prefix) {
        rieNode ws = root;
        for(int i = 0; i < prefix.length(); i++){
            char c= word.charAt(i);
            if(ws.children[c-'a'] == null) return false;
            ws = ws.children[c - 'a'];
        }
        return true;
    }
}

```

# 211. Add and Search Word - Data structure design


```Java
class WordDictionary {

    /** Initialize your data structure here. */
    public WordDictionary() {
        
    }
    
    /** Adds a word into the data structure. */
    public void addWord(String word) {
        
    }
    
    /** Returns if the word is in the data structure. A word could contain the dot character '.' to represent any one letter. */
    public boolean search(String word) {
        
    }
}
```
