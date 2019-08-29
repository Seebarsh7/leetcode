Hard
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
