# 56. Merge Intervals
* 此题是谷歌经典题，前面几个算法我就按下不表，因为这个是可以用graph解决的。（不会，不想写了）
![Graph](https://windliang.oss-cn-beijing.aliyuncs.com/56_11.jpg)
我们用一个 HashMap，用邻接表的结构来实现图，类似于下边的样子。     
![Graph2](https://windliang.oss-cn-beijing.aliyuncs.com/56_12.jpg)   
* 如何自己写一个sort函数！
```Java
private class IntervalComparator implements Comparator<int[][]> {
@Override
    public int compare(int[] a, int[] b) {
        return a[0] < b[0] ? -1 : a[0] == b[0] ? 0 : 1;
    }
}

public List<Interval> merge(List<Interval> intervals) {
    Collections.sort(intervals, new IntervalComparator());
    int[][] res = new int[][];
    for(int i = 0; i < res[0].size; i++){
        if (res.length()==0 || res[-1][1] < res[i][0]) {
            res.add(int[i]);
        }
        else {
            res[-1][1] = Math.max(res[-1][1], res[i][0]);
        }
    }
}
```
# 116. 117. Populating Next Right Pointers in Each Node 
今天再来俩graph的题就可以告老还梦乡了。   
* 116 is a perfect binary tree
来吧让我们搞个BFS, 
### 101. Symmetric Tree
tree structure   
```Java
public class TreeNode {
     int val;
     TreeNode left;
     TreeNode right;
     TreeNode(int x) { val = x; }
 }
```
```Java
public boolean isSymmetric(TreeNode root) {
    if(root == NULL) return true;
    Queue<TreeNode> leftTree = new LinkedList<>();
    Queue<TreeNode> rightTree = new LinkedList<>();
    
    leftTree.offer(root.left);
    rightTree.offer(root.right);
    while(!leftTree.isEmpty() && !rightTree.isEmpty()){
        TreeNode cur_left = leftTree.poll();
        TreeNode cur_right = rightTree.poll();
        if (curLeft == null && curRight != null || curLeft != null && curRight == null) {
            return false;
        }
        if (curLeft != null && curRight != null) {
            if (curLeft.val != curRight.val) {
                return false;
            }
            leftTree.offer(cur_left.left);
            leftTree.offer(cur_right.left);
            rightTree.offer(cur_right.right);
            rightTree.offer(cur_left.right);
         }
    }
    if (!leftTree.isEmpty() || !rightTree.isEmpty()) {
        return false;
    }
    return true;
    
}
```
### 102. Binary Tree Level Order Traversal
```Java
public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> ans = new ArrayList<>();
    if(root == NULL) return ans;
    Queue<TreeNode> treeNode = new LinkedList<>();
    Queue<TreeNode> levelNode = new LinkedList<>();
    treeNode.offer(root);
    int level = 0;
    nodeLevel.offer(level);
    while(!treeNode.isEmpty()){
        int curLevel = nodeLevel.poll();
        TreeNode curNode = treeNode.poll();
        if (curNode != null) {
            if(ans.size() < curLevel){
                ans.add(new ArrayList<Integer>());
            }
            ans.get(curLevel).add(curNode.val);
            level = curLevel + 1;//很关键！
            treeNode.offer(curNode.left);
            nodeLevel.offer(level);
            treeNode.offer(curNode.right);
            nodeLevel.offer(level);
        }
        
    }
    return ans;
}
```

### 116. （Medium）Populating Next Right Pointers in Each Node
```Java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node left;
    public Node right;
    public Node next;

    public Node() {}

    public Node(int _val,Node _left,Node _right,Node _next) {
        val = _val;
        left = _left;
        right = _right;
        next = _next;
    }
};
*/
class Solution {
    public Node connect(Node root) {
        if(root = null) return root;
        Queue<Node> queue = new LinkedList<Node>();
        queue.offer(root);
        Node pre = null;
        while(!queue.isEmpty()){
            int size = queue.size();
            for(int i = 0; i < size; i++){
                Node cur = queue.poll();
                if(i > 0){
                    pre.next = cur;
                }
                pre = cur;
                if(cur.left != null) queue.offer(cur.left);
                if(cur.right != null) queue.offer(cur.right);
            }
            
            
        }
        return root;
        
    }
}
```
If want to use constant space:
```Java
public Node connect(Node root) {
    if(root = null) return null;
    Node start = root;
    Node prev = root;
    Node cur = null;
    while(start.left != null){
        if(cur == null){
            prev.left.next = prev.right;
            prev = start.left;
            cur = start.right;
            start = prev;
        }
        else{
            prev.left.next = prev.right;
            prev.right.next = cur.left;
            prev = prev.next;
            cur = cur.next;
        }
        
    }
    return root;
}
```

### 117可以用116的BFS思想解决
![](https://windliang.oss-cn-beijing.aliyuncs.com/117_3.jpg)
```Java
public Node connect(Node root) {
    Node cur = root;
    Node dummy = new Node();
    while(cur!=null){
        (Node tail = dummy).next = null;
        if(cur.left!=null){
            tail.next = cur.left;
            tail = tail.next;
        }
        if(cur.right!=null){
            tail.next = cur.right;
            tail = tail.next;
        }
        cur = cur.next;
    }
    return root;
}
```

# 124. Binary Tree Maximum Path
* B-Tree： Recursion
全局变量+Recursion
* 看到一个post说，不断更新max为val+left+right. 但每次返回的时候只返回 root.val + max(left,right). 这个实在是太巧妙了。我操他妈的。

```Java
int max = Integer.MIN_VALUE;
public int maxPathSum(TreeNode root) {
    helper(root);
    return max;
}
public int helper(TreeNode root){
    if(root == null) return 0;
    int left = Math.max(helper(root.left), 0);
    int right = Math.max(helper(root.right), 0);
    
    max = Math.max(max, root.val+left+right);
    return root.val+Math.max(left,right);
}
```
### 104. (Easy) Binary Tree Max Depth
* DFS in a recursive manner
```Java
public int maxDepth(TreeNode root) {
    if(root = null) return 0;
    return Math.max(maxDepth(root.left), maxDepth(root.right)) + 1;
}
```
* BFS
```Java
public int maxDepth(TreeNode root) {
   和103相似， while里套for 
}
```
#### 102. Binary Tree Level Traverse
```
DFS一般都是递归做的
```
```Java
public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> ans = new ArrayList<>();
    DFS(ans, root, 0);
    return ans;
}

public void DFS(List<List<Integer>> ans, TreeNode root, int level){
    if(root = null) return;
    if(ans.size() <= level){ //root has level 0 but has 1 arraylist
        ans.add(new ArrayList<Integer>());
    }
    ans.get(level).add(root.val);
    DFS(ans, root.left, level+1);
    DFS(ans, root.right, level+1);
}
```
```
BFS
```

```Java
public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> ans = new ArrayList<>();
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    while(!queue.isEmpty){
       
        List<Integer> sublist = new ArrayList<>();
        for(int i = 0; i < queue.size(); i++){
            TreeNode cur = queue.poll();
            if(cur!= null){
                sublist.add(cur.val);
                queue.offer(cur.left);
                queue.offer(cur.right);
            }
        }
        if(sublist.size() > 0) ans.add(sublist);
    }
    return ans;

}
```

#### 103. Zigzag
* DFS/BFS modify from 102 is ok, just need to remember when adding to end, when adding to head
```Java
if ((level % 2) == 0) {
        ans.get(level).add(root.val); //添加到末尾
    } else {
        ans.get(level).add(0, root.val); //添加到头部
    }
```
* Using Stack, interesting because of Stack's Last in First out.

### (Medium)105. Construct Binary Tree from Preorder and Inorder Traversal 
* preorder: root, left child，right child
* in order: left child，root，right child


### 106 和105差不多我觉得有点难

### (Easy)110. Balanced Binary Tree
```Java
public boolean isBalanced(TreeNode root) {
    return getTreeDepth!= -1;
}
private int getTreeDepth(TreeNode root) {
    if(root == null) return 0;
    int left = getTreeDepth(root.left);
    if(left == -1) return -1;
    int right = getTreeDepth(root.right);
    if(right == -1) return -1;
    
    if(Math.abs(left - right) > 1 ) return -1;
    return Math.max(left, right);
}
```
# 200. Number of Islands
* woc看懂之后拍案叫绝，why DFS？ 这就相当于一个四个方向的DFS，如果对应格子不是1的话就不走了，如果是1就把它改成0，然后在这个1的基础上继续向四周发散。
* 如果继续不能前进了（周围都是0）就return
* 回到主函数的for loop，这时候如果grid里还有1，那么就应该是island了。
* 整体一个大的也叫一个island
```Java
public int numIslands(char[][] grid) {
    int Y = grid.length;
    int X = grid[0].length;
    for(int i = 0; i < X; i++){
        for(int j = 0; j < Y; j++){
            if(grid[i][j] == 1){
                DFS(grid, i, j);
                count++;
            }
        }
    }
    public void DFS(char[][] grid, int i, int j){
        if(i < 0 || j < 0 || i > X || j > Y || grid[i][j] != 1) return;
        grid[i][j] = 0;
        DFS(grid, i+1, j);
        DFS(grid, i-1, j);
        DFS(grid, i, j + 1);
        DFS(grid, i, j - 1);
    }
}
```
# 222. Count Complete Tree Nodes
* 好的我花了半个小时然后发现自己看错题了，日
* 第一反应是可以用BFS或者recursion做，再想一想感觉BFS可能不太合适
* 今天感觉看答案之前先自己想想会比较好，除了一些从来没遇到过的，比如昨天的Trie 
* 自从学了CS每天都还是乐呵乐呵的，这里还学到了位运算，左移×2，右移÷2
* Given a complete binary tree, count the number of nodes.
     
* Explanation

The height of a tree can be found by just going left. Let a single node tree have height 0. Find the height h of the whole tree. If the whole tree is empty, i.e., has height -1, there are 0 nodes.

Otherwise check whether the height of the right subtree is just one less than that of the whole tree, meaning left and right subtree have the same height.
    * If yes, then the last node on the last tree row is in the right subtree and the left subtree is a full tree of height h-1. So we take the 2^h-1 nodes of the left subtree plus the 1 root node plus recursively the number of nodes in the right subtree.
    * If no, then the last node on the last tree row is in the left subtree and the right subtree is a full tree of height h-2. So we take the 2^(h-1)-1 nodes of the right subtree plus the 1 root node plus recursively the number of nodes in the left subtree.
```Java
public int height(TreeNode root){
    return root.left == null? -1: height(root.left) + 1; //root的高度是0
}
    
public int countNodes(TreeNode root) {
    int h = height(root);
    int nodes = 0;
    while(root!= null){
        if(height(root.right) == h-1){ //indicates that the left tree is complete
            nodes += (1 << h);
            root = root.right;
        } 
        else if(heigh(root.right) == h-2){ //indicates that right tree is complete
            nodes = (1 << h-1);
            root = root.left;
        }
        h--;
    return nodes;
}

```
# 218 The Skyline Problem (Hard) - Segment Tree
```Java
void ConstructTree(int input[], int segTree[], int low, int high, int pos){
    if(low == high){
        segTree[pos] = input[low];
        return;
    }
    int mid = (low+high)/2;
    constructTree(input[], segTree[], low, mid, 2*pos+1);
    constructTree(input[], segTree[], mid, high, 2*pos+2);
    segTree[pos] = Math.min(segTree[2*pos+1], segTree[2*pos+2]);
}
```
解法有priority queue和tree map, 都是以前没用过的数据结构
* priority queue
```Java
public List<int[]> getSkyline(int[][] buildings) {
    List<int[]> res = new ArrayList<>();
    List<int[]> height = new ArrayList<>();
    for(int[] b: buildings){
        height.add(new int[]{b[0], -b[1]}); // put the start point into the height array, indicating by negative height
        height.add(new int[]{b[1], b[2]}); //put the end point into the height array, indicating by positive height
    }
    Collections.sort(height, (a,b)->(a[0] == b[0]?a[1]-b[1]:a[0]-b[0])); //define a comparator that's gonna be used in PQ
    PriorityQueue<Inetegr, Integer> pq = New PriorityQueue<>((a, b) -> (b - a));//sort in descending order
    po.offer(0); //dummy root 0
    int prev = 0;
    
    for(int[] h: height){
        if(h[1] < 0){
            pq.offer(-h[1]); //if h[1]<0, indicating that this is the start point of the pq
        }
        else{
            pq.remove(h[1]); //else, this is the end point and would remove from the pq      
        }
        int cur = pq.peek();
        if(cur > prev){
            res.add(new int[]{h[0], curMax}); //why h[0]? it means adding the point changes the previous max
            //so this would be the max: a critical point
            prev = cur;
        }
    }
    return res;
}

```
* TreeMap
    * the difference is the removal, and tree cannot contain any duplicate keys.
```Java
public List<int[]> getSkyline(int[][] buildings) {
    List<int[]> res = new ArrayList<>();
    List<int[]> height = new ArrayList<>();
    for(int[] b: buildings){
        height.add(new int[]{b[0], -b[1]}); // put the start point into the height array, indicating by negative height
        height.add(new int[]{b[1], b[2]}); //put the end point into the height array, indicating by positive height
    }
    Collections.sort(height, (a,b)->(a[0] == b[0]?a[1]-b[1]:a[0]-b[0])); //define a comparator that's gonna be used in PQ
    TreeMap<Integer, Integer> map = new TreeMap<>(Collections.reverseOrder());
    int prev = 0;
    map.put(0,1); //dummy node
    for(int[] h: height){
        if(h[1] < 0){
            Integer count = map.get(-h[1]); //this is for duplicate keys
            count = count==null? 1: count+1;
            map.put(-h[1], count);
        }
        else{
            Integer count = map.get(h[1]); 
            if(count == 1){ //can safely remove from the Tree
                map.remove(h[1]);
            }else{
                map.put(h[1], count-1);
            } 
        }
        int cur = map.firstKey(); //the max
        if(cur > prev){
            res.add(new int[]{h[0], cur});
            prev = cur;
        }
    }
    return res;
}
```
# 315. Count of Smaller Numbers After Self
似乎是一道谷歌真题
* 复习一下merge sort
```Java
void merge(int arr[], int l, int m, int r) 
{ 
    int i, j, k; 
    int n1 = m - l + 1; 
    int n2 =  r - m; 
    
    //create new list to store left and right 
    int left[n1], right[n2];
    //copy array to temp list
    for(int i = 0; i < n1; i++){
        left[i] = arr[i];
    }
    for(int i = 0; i< n2; i++){
        right[i] = arr[i+m+1];
    }
    
   //compare and merge back to arr
    i = 0; j = 0; k = 0;
    while(i < n1 && j < n2){
        if(left[i] < right[j]){
            arr[k] = left[i];
            i++;
        }else{
            arr[k] = right[j];
            j++
        }
        k++
    }
    //the leftover of left/right, only one case would happen
    while(i < n1){
        arr[k] = left[i];
        i++;
        k++;
    }
    while(j < n2){
        arr[k] = right[j];
        j++;
        k++;
    }
}

void mergeSort(int arr[], int l, int r) { 
    if (l < r) {
        int m = (l+r)/2;
        mergeSort(arr, l, m);
        mergeSort(arr, m+1, r);
        merge(arr, l, m, r);
    }
    return arr;
}
```
* 本题其实一样，只不过要keep一个index数组一个count数组，解法特别机智！规则：
    * While doing the merge part, say that we are merging left[] and right[], left[] and right[] are already sorted.
    * We keep a rightcount to record how many numbers from right[] we have added and keep an array count[] to record the result.
    * When we move a number from right[] into the new sorted array, we increase rightcount by 1.
    * When we move a number from left[] into the new sorted array, we increase count\[ index of the number \] by rightcount.
  
