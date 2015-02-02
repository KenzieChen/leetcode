2015/02/02
###题目

>Given two sorted integer arrays A and B, merge B into A as one sorted array.

>Note:
>You may assume that A has enough space (size that is greater or equal to m + n) to hold additional elements from B. The number of 
>elements initialized in A and B are m and n respectively.

###分析

####方法一

题中已经明确说明，A数组的长度足够大，所以我们可以从A数组和B数组的末尾（A[m-1]和B[n-1]）比起，将较大的数依次从A数组的末尾A[m+n-1]开始填充。

代码如下：

```java
public class Solution {
    public void merge(int A[], int m, int B[], int n) {
        int i = m-1;
		int j = n-1;
		int k = m+n-1;
		while(i>=-1 && j>=-1){
			if(i== -1){
				for(int i1 =0; i1 <= j;i1++ ){
					A[i1] = B[i1];
				}
				break;
			}
			if(j== -1){
				break;
			}
			if(A[i] > B[j]){
				A[k--] = A[i--];
			}else{
				A[k--] = B[j--];
			}
		}
    }
}
```