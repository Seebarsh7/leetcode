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
