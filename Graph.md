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
