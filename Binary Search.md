# 35. Search Insert Position
Binary Search Model
* 这是目前我比较接受的一种模板，就是end指针初始化为length
* start每次移动到mid+1
```Java
public int searchInsert(int[] nums, int target) {
        if(nums.length == 0) return 0;
        int start = 0; 
        int end = nums.length;
        int mid = 0;
        while(start < end){
            mid = (start + end)/2;
            if(nums[mid] == target) return mid;
            else if(nums[mid] < target) 
                start = mid + 1;
            else 
                end = mid;
        }
        return start;
    }
```
# 74. 2D Array
```Java
public boolean searchMatrix(int[][] matrix, int target) {
        if(matrix == null || matrix.length == 0) return false;
        int start = 0;
        int n = matrix.length;
        int m = matrix[0].length;
        int end = n*m;
        int mid = 0;
        while(start < end){
            mid = (start+end)/2;
            if(matrix[mid/m][mid%m] == target) return true;
            if(matrix[mid/m][mid%m] < target) start = mid+1;
            else end = mid;
        }
        return false;
    }
```
* Still can search first column first and then search that particular column
```Java
public boolean searchMatrix(int[][] matrix, int target) {
        if(matrix == null || matrix.length == 0 || matrix[0].length == 0) 
            return false;
        if(target < matrix[0][0] || target > matrix[matrix.length-1][matrix[0].length-1]) return false;
        int start = 0, end = matrix.length, mid = 0;
        while(start < end){
            mid = (start + end)/2;
            if(matrix[mid][0] == target) return true;
            if(matrix[mid][0] < target) start = mid + 1;
            else end = mid;
        }
        int row = start - 1;
        start = 0; end = matrix[0].length; mid = 0;
        while(start < end){
            mid = (start + end)/2;
            
            if(matrix[row][mid] == target) return true;
            if(matrix[row][mid] < target) start = mid + 1;
            else end = mid;
        }
        return false;
    }
```

# 33. Search in Rotated Sorted Array
* 这题570见过
```Java
public int search(int[] nums, int target) {
        if(nums.length == 0) return -1;
        int start = 0, end = nums.length, mid = 0;
        while(start < end){
            mid = (start+end)/2;
            if(target == nums[0]) return 0;
            //if target belongs to right half
            if(target < nums[0]){
                //mid falls into right half
                if(nums[mid] < nums[0]){
                    if(target == nums[mid]) return mid;
                    if(target < nums[mid]) end = mid;
                    else start = mid+1;
                }
                //
                else start = mid + 1;
            }
            //target belongs to left half
            else{
                //mid falls into left half
                if(nums[mid] > nums[0]){
                    if(target == nums[mid]) return mid;
                    if(target < nums[mid]) end = mid;
                    else start = mid+1;
                }
                else end = mid;
            }
        }
        return -1;
    }
```

# Quick Sort
* choose pivot is arbitrary
```Java
int partition(int arr[], int low, int high) {
        int pivot = arr[high];
        int i = low;
        //swap smaller numbers to the header of array
        for(int j = low; j < high; j++){
                if(arr[j] < pivot){
                        int temp = arr[i];
                        arr[i] = arr[j];
                        arr[j] = temp;
                        i++;
                }
        }
        //no swap indicating remaining are larger than pivots, move pivots to the current position pointed by i
        arr[high] = arr[i];
        arr[i] = pivot;
        
        return i;
} 

void sort(int arr[], int low, int high) {
        int pos = partition(arr, low, high);
        sort(arr, low, pos-1); //lower part, pos does not change
        sort(arr, pos+1, high); //higher part
} 
```
