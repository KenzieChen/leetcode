2015/02/14
###题目：

> Given numRows, generate the first numRows of Pascal's triangle.

> For example, given numRows = 5,
> Return

> [
>      [1],
>     [1,1],
>    [1,2,1],
>   [1,3,3,1],
>  [1,4,6,4,1]
> ]


###分析

即求解杨辉三角

代码如下：

```java
public class Solution {
    public List<List<Integer>> generate(int numRows){
		List<List<Integer>> triangle = new ArrayList<List<Integer>>();
		List<Integer> prev = new ArrayList<Integer>();
		for(int count = 1; count <= numRows; count++){
			prev = generateNext(prev, count);		
			triangle.add(prev);
		}
		return triangle;
	}
	private List<Integer> generateNext(List<Integer> prev,int count){
		List<Integer> curr = new ArrayList<Integer>();
		for(int i=0;i< count;i++){
			if(i==0 || i==count-1){
				curr.add(1);
			}else{
				curr.add(prev.get(i-1)+prev.get(i));
			}
		}
		return curr;
	}
}
```