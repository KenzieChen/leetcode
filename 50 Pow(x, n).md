2015/02/14
###题目

> Implement pow(x, n).

###题目大意：

即计算x的n次方。

###题目分析

主要是考虑x和n取特殊值的时候

- 任何数的0次方等于1；
- 0的正数次方为0，0的负数次方为无穷大；

采用递归方法，代码如下：

```java
public class Solution {
	public double pow(double x,int n){
		if(n==0){
			return 1;
		}
		if(x ==0){
			if(n<0){
				return Double.POSITIVE_INFINITY;
			}else{
				return 0;		
			}
		}
		if(n==1){
			return x;
		}
		if(n==2){
			return x*x;
		}
		int positiveN = n>0 ? n : -n;
		x = positiveN % 2 == 1 ? x*pow(pow(x,positiveN/2),2) : pow(pow(x,positiveN/2),2);
		x = n>0 ? x : 1/x;
		return x;
	}
}
```
