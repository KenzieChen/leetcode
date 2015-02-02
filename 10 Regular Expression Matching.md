2015/02/01
###题目

> Implement regular expression matching with support for '.' and '*'.

>     '.' Matches any single character.
>     '*' Matches zero or more of the preceding element.

>     The matching should cover the entire input string (not partial).
>     The function prototype should be:

>     bool isMatch(const char *s, const char *p)

>     Some examples:
>     isMatch("aa","a") → false
>     isMatch("aa","aa") → true
>	  isMatch("aaa","aa") → false
>     isMatch("aa", "a*") → true
>     isMatch("aa", ".*") → true
>     isMatch("ab", ".*") → true
>     isMatch("aab", "c*a*b") → true

###分析

####方法一

对于字符串匹配问题，我们可以用**动态规划算法**，首先考虑比较简单的情况，只匹配字符串字母和包含匹配规则‘.’，此时我们就可以将问题分为两部分：

一部分是s的最后一位s[m]和p的最后一位p[n]的匹配问题，一部分是s[0~m-1]和p[0~n-1]的匹配问题。

前面一个问题的求解简单，如果匹配正确，则考虑后面一部分问题，恰好后面一个问题为我们所求问题的子问题，于是所求的问题可以一直细化为一系列的子问题。

如果再加上匹配规则‘*’，则此时要考虑用**回溯算法**，分为普通回溯“字母+\*”和点回溯“.+\*”。

如果“*”前面无字符，则无此规则，为假。

对于普通回溯“字母+\*”：



对于点回溯“.+\*”：



此外还需要考虑的问题就是子问题的终结问题，即最小子问题。

- 两个空字符串，结果为真
- 子问题中s被完全匹配后，p中还有字符，如果包含“*”，子问题可能为真
- 子问题中p被完全匹配后，s中还有字符，那么该子问题为假。

代码如下：

```java
public class Solution {
	public boolean isMatch(String s,String p) {
		char[] sArr = s.toCharArray();
		char[] pArr = p.toCharArray();
		int sIndex = sArr.length - 1;
		int pIndex = pArr.length - 1; 
		return isPartMatch(sArr, sIndex, pArr, pIndex);
	}
	public boolean isPartMatch(char[] s,int sIndex, char[] p , int pIndex){
	    if(sIndex ==-1 && pIndex ==-1){
	        return true;
	    }
	    if(sIndex==-1 && pIndex >0){
				if((pIndex) % 2 == 1){
					boolean flag4 = true;
					for(int i=1;i<=pIndex;i=i+2){
						if(p[i]!='*'){
							flag4 = false;
							break;
						}
					}
					return flag4;
				}
		}
		if(sIndex < 0 || pIndex <0){
			return false;
		}
		if(p[pIndex] == '*'){
			if(pIndex == 0){
				return false;
			}
			if(p[pIndex-1] == '.'){
				if(pIndex == 1){
					return true;
				}
				//回溯
				int index1;
				for(index1 = sIndex; index1 >=-1 ; index1-- ){
					boolean flag2 =isPartMatch(s, index1, p, pIndex-2); 
					if(flag2){
						return flag2;	
					}
				}
				return false;		
			}else{
				boolean flag3 = false;
			    int repeatCount = 0;
			    int sIndex2 = sIndex;
				while(s[sIndex2] == p[pIndex-1]){
					repeatCount++;
					sIndex2--;
					if(sIndex2 == -1){ 
					    break;
					}
				}
				if(pIndex == 1 ){
					if(sIndex-repeatCount == -1){
						flag3 = true;
					}
				}else{
					//回溯
					int index2;
					for(index2 = sIndex; index2 >= (sIndex-repeatCount); index2--){
							flag3 =isPartMatch(s,index2,p,pIndex-2); 
                            if(flag3){
                            	break;
                            }
					}
				}
				if(flag3){
			        return true;
			    }
			    return false;
			}
		}else if(p[pIndex] == '.' || p[pIndex] == s[sIndex]){
			if(pIndex == 0 && sIndex == 0){	
				return true;	
			}
			sIndex--;
			pIndex--;
			return isPartMatch(s, sIndex, p, pIndex);
		}else{
			return false;
		}
	}
}
```