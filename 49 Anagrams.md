2015/02/14
###题目：

> Given an array of strings, return all groups of strings that 
> are anagrams.

> Note: All inputs will be in lower-case.

###题目大意：

给一个由字符串组成的数组，返回所有字符串中由字符颠倒组成的字符串组。具体意思就是，如果所有字符串中，有两个以上的字符串是由相同字符颠倒排序组成的（包括两个相同字符串），那么这几个字符串就视为一组，找出所有组，合并后返回。

###分析

用hashmap保存由相同字符组成的所有字符串到一个list中，key设置为字符排序后得到的字符串，遍历完所有字符串后，遍历key，找出所有list中size大于1的字符串，添加到要返回的list中去。

代码如下：
```java
public class Solution {
	public List<String> anagrams(String[] strs){
		List<String> anagramsList = new ArrayList<String>();
		HashMap<String,List<String>> map = new HashMap<String,List<String>>(); 
		for(int i = 0;i < strs.length;i++){
			String s = sort(strs[i]);
			if(!map.containsKey(s)){
				map.put(s,new ArrayList<String>());
			}	
			map.get(s).add(strs[i]);
		}
		for(String key : map.keySet()){
			List<String> list = map.get(key);
			if(list.size()>1){
				anagramsList.addAll(list);	
			}
		}
		return anagramsList;
	}
	private String sort(String str){
		char[] charArr = str.toCharArray();
		Arrays.sort(charArr);
		return Arrays.toString(charArr);
	}
}
```
