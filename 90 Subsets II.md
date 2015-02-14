2015/02/14
###题目：

> Given a collection of integers that might contain duplicates, S, return all possible subsets.

> Note:
> Elements in a subset must be in non-descending order.
> The solution set must not contain duplicate subsets.
> For example,
> If S = [1,2,2], a solution is:

> [
>   [2],
>   [1],
>   [1,2,2],
>   [2,2],
>   [1,2],
>   []
> ]

###题目大意：

给一个有可能包含重复的整数集合S，返回其所有可能子集。

###分析：

本题适合采用动态规划法和回溯法

在一个整数集合S中添加一个元素m（整数）时，即将其与S的所有子集加上S的所有子集依次与m合并所得的子集，即得到了S+m的所有整数。
如若遇到重复的正数，依次将1个至n（n为重复次数）个重复的整数视为一个整体添加到一个整数集合中，方法同上。

代码如下：
```java
public class Solution {
	public List<List<Integer>> subsetsWithDup(int[] num){
		List<List<Integer>> allSubsets = new ArrayList<List<Integer>>();
		List<Integer> subset = new ArrayList<Integer>();
		allSubsets.add(subset);
		Arrays.sort(num);
		List<Integer> subset3;
		int count =1;
		int i=0;
		while(i < num.length){
			int len = allSubsets.size();
			int ii = i;
			while(ii+1< num.length && num[ii] == num[ii+1]){
				count++;
				ii++;
			}
			List<Integer> list = new ArrayList<Integer>();
			for(int j=0; j < count; j++){
				list.add(num[i+j]);	
				for(int m = 0; m < len; m++){
					subset3 = new ArrayList<Integer>(allSubsets.get(m));
					subset3.addAll(list);
					allSubsets.add(subset3);
				}
			}
			i=ii+1;
			count=1;
		}
		return allSubsets;
	}
}
```