2015/02/14
###题目：

> Given a collection of intervals, merge all overlapping 
> intervals.

> For example,
> Given [1,3],[2,6],[8,10],[15,18],
> return [1,6],[8,10],[15,18].

###题目大意：

给一个由间隔组成的集合，消除所有重叠部分并返回融合后的间隔。

###分析：

首先要解决的是判断两个间隔是否重叠并消除的方法，想到的是消除间隔的方法，两个间隔，分别取其start和end，进行排序，如果位置不变或者是start和end作为整体置换位置，那么说明两个间隔没有重叠，但是还要考虑前一个间隔的end和后一个间隔的start是否相等，是的话也要合并。

其次要解决的问题是如何遍历所有间隔进行合并，我采用的方法是分步的合并方式

1. 将第一个间隔与后面的间隔一次进行合并
2. 无需合并就跳到下一个，需要合并就合并完之后赋值给第一个间隔，再与后面的间隔进行合并，第一个间隔与后面所有的间隔都不重复为止。
3. 取出第一个元素，再回到第一步，直到取出最后一个间隔。

代码如下：
```java
/**
 * Definition for an interval.
 * public class Interval {
 *     int start;
 *     int end;
 *     Interval() { start = 0; end = 0; }
 *     Interval(int s, int e) { start = s; end = e; }
 * }
 */
public class Solution {
	public List<Interval> merge(List<Interval> intervals){
		List<Interval> merged =new ArrayList<Interval>();
		while(intervals.size()>0){
			intervals = deleOverlap4First(intervals);
			merged.add(intervals.get(0));
			intervals.remove(0);
		}
		return merged;
	}
	private List<Interval> deleOverlap4First(List<Interval> intervals){
		int i = 1;
		int[] sortArr = new int[4];
		sortArr[0] = intervals.get(0).start;
		sortArr[1] = intervals.get(0).end;
		while(i < intervals.size()){
			sortArr[2] = intervals.get(i).start;
			sortArr[3] = intervals.get(i).end;
			Arrays.sort(sortArr);
			if(sortArr[1]==sortArr[2]){
				 intervals.remove(i);
				 intervals.get(0).start = sortArr[0];
				 intervals.get(0).end = sortArr[3];
				 sortArr[1] = sortArr[3];
				 i=1;
				 continue;
			}
			if((sortArr[0] == intervals.get(0).start &&
			sortArr[1] == intervals.get(0).end &&
			sortArr[2] == intervals.get(i).start &&
			sortArr[3] == intervals.get(i).end )||
			(sortArr[0] == intervals.get(i).start &&
			 sortArr[1] == intervals.get(i).end &&
			 sortArr[2] == intervals.get(0).start &&
			 sortArr[3] == intervals.get(0).end)){
			    sortArr[0] = intervals.get(0).start;
				sortArr[1] = intervals.get(0).end;
				i++;
			 }else{
				 intervals.remove(i);
				 intervals.get(0).start = sortArr[0];
				 intervals.get(0).end = sortArr[3];
				 sortArr[1] = sortArr[3];
				 i=1;
			 }
		}
		return intervals; 
	}
}
```


