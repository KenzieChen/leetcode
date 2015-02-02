2015/01/30
###题目

> Given a sorted array of integers, find the starting and ending position of a given target value.
> Your algorithm's runtime complexity must be in the order of O(log n).
> If the target is not found in the array, return [-1, -1].

> For example,
> Given [5, 7, 7, 8, 8, 10] and target value 8,
> return [3, 4].

###分析

####方法一

分三种情况：

1. 找到1个，返回数组中两个元素相同
1. 找到多余一个，返回数组中后一个元素比前一个元素大`count-1`个,`count`为所有匹配个数。
1. 找到0个，返回[-1,-1]

以下算法时间复杂度为O(n)，不符合题意

代码如下：

```java
public class Solution {
    public int[] searchRange(int[] A, int target) {
        int length = A.length;
		int[] position = {-1,-1};
		int count = 0;
		boolean flag = false;        //标识是否找到
		for(int i=0;i < length;i++){
			if(target == A[i] && !flag){
				position[0] = i;
				flag =true;			
			}else if (target == A[i] &&flag){
				count++;
			}
		}
		position[1] = position[0]+count;
		return position;
    }
}
```

运行时间：233ms

####方法二

将int数组转换为List，采用List已有的方法直接查找第一个出现target和最后一个出现target的位置

以下时间复杂度也为O(n)，不符合题意

代码如下：

```java
public class Solution {
    public int[] searchRange(int[] A, int target) {
       int[] position ={-1,-1};
		Integer[] objA = new Integer[A.length];
		for(int i = 0; i<A.length; i++){
			objA[i] = A[i]; 
		}
		List A2List = Arrays.asList(objA);
		position[0] = A2List.indexOf(target);
		position[1] = A2List.lastIndexOf(target);
		return position;
    }
}
```

运行时间：230ms

####方法三

采用二分法，如果某个中间值等于目标值，则从这个中间值向左右展开查找。

代码如下：

```java
public class Solution {
    public int[] searchRange(int[] A, int target) {
        int length = A.length;
		int mid = (length-1)/2;
		int leftBound = 0;
		int rightBound =length-1;
		int left = -1;
		int right = -1;
		while(true){
			if(A[mid] == target){
				left = right = mid;
				while(A[left] == target){
					left--;
					if(left < 0){
						break;
					}
				}
				while(A[right] == target){
					right++;
					if(right >= length){
						break;
					}
				}
				left++;
				right--;
				break;
			}else if(A[mid] < target){
				leftBound = mid+1;
				if(leftBound >= length){
					break;
				}
			}else{
				rightBound = mid-1;
				if(rightBound <0){
					break;
				}
			}
		    if(rightBound < leftBound){
		        break;
		    }
			mid = (leftBound+rightBound)/2;
		}
		return new int[]{left,right};
    }
}
```
运行时间：231ms


###参考

#### 参考一

递归二分法

```java
public class Solution {
    private int left = -1;;
    private int right = -1;
    public int[] searchRange(int[] A, int target) {
        int n = A.length;
        binaryRangeSearch(A, target, 0, n-1);
        int[] result = new int[2];
        result[0] = left;
        result[1] = right;
        return result;
    }
    private void binaryRangeSearch(int[] A, int target, int lo, int hi) {
        if (lo > hi)    return;
        int mid = lo + (hi - lo) / 2;
        if (target == A[mid]) {
            if (left == -1 || right == -1) {
                left = mid;
                right = mid;
            } else {
                if (mid < left) left = mid;
                if (mid > right) right = mid;
            }
            if (mid > lo && mid <= left && A[mid-1] == A[mid])
                binaryRangeSearch(A, target, lo, mid-1);
            if (mid < hi && mid >= right && A[mid] == A[mid+1])
                binaryRangeSearch(A, target, mid+1, hi);
        } else if (target > A[mid]) 
            binaryRangeSearch(A, target, mid+1, hi);
        else 
            binaryRangeSearch(A, target, lo, mid-1);
    }
}
```

####参考二

简洁的递归二分

```java
public int[] searchRange(int[] A, int l, int r, int target) {
    int[] result = new int[] { -1, -1 };
    while (l <= r) {
        int mid = (l + r) / 2;
        if (A[mid] < target) {
            l = mid + 1;
        } else if (A[mid] > target) {
            r = mid - 1;
        } else {
            int[] left = searchRange(A, l, mid - 1, target);
            result[0] = left[0] == -1 ? mid : left[0];
            int[] right = searchRange(A, mid + 1, r, target);
            result[1] = right[1] == -1 ? mid : right[1];
            break;
        }
    }
    return result;
}

public int[] searchRange(int[] A, int target) {
    return searchRange(A, 0, A.length - 1, target);
}
```