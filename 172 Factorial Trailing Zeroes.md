###题目：

> Given an integer n, return the number of trailing zeroes in n!.
> Note: Your solution should be in logarithmic time complexity.


###题目大意：

给定一个整数，求阶乘n!的值后面有多少个0，时间复杂度为log。

###分析：

数与数之间相乘，要想得到值的尾数为0，只能是5*2，一对5和2就能得到一个尾数0，当有很多个5和2的时候，根据5和2中数量少的一方决定有多少个0。

对于n!，我们可以看成是`count5`个5和`count2`个2与其他数相乘，`count5`和`count2`哪个较小，n!值的尾数就有多少个0。

代码如下：

```java
public class Solution{
    public int trailingZeroes(int n) {
    	//初始化2和5的个数
        int count2 = 0;
        int count5 = 0;
        for(int i = n ; i > 1; i--){
            count2 += countOfDivided(i,2);
            count5 += countOfDivided(i,5);
        }
        return count2 < count5 ? count2 : count5;
     }
     //根据被除数dividend和除数divider，确定被除数能被除数整除多少次
     public int countOfDivided(int dividend, int divider){
         long remainder = dividend % divider;
         long result = dividend / divider;
         int count =0;
         while(remainder ==0){
             count++;
             remainder = result % divider;
             result =result / divider;
         }
         return count;
     }
}
```

运行结果为：
> Time Limit Exceeded
> Last executed input:	1808548329

提示超过时间限制

####改进一

进一步的分析，可知道n!中因数2的个数肯定比因数5的个数多，因为逢2进1比逢5进1的增长速度要快的多！因此，我们只需要求5的个数就可以了，将上面与2有关的代码去除。

代码如下：

```java
public class Solution{
    public int trailingZeroes(int n) {
    	//初始化5的个数
        int count5 = 0;
        for(int i = n ; i > 1; i--){
            count5 += countOfDivided(i,5);
        }
        return count2 < count5 ? count2 : count5;
     }
     //根据被除数dividend和除数divider，确定被除数能被除数整除多少次
     public int countOfDivided(int dividend, int divider){
         long remainder = dividend % divider;
         long result = dividend / divider;
         int count =0;
         while(remainder ==0){
             count++;
             remainder = result % divider;
             result =result / divider;
         }
         return count;
     }
}
```

运行结果为：

> Time Limit Exceeded
> Last executed input:	1808548329

提示超过时间限制

####改进二

现在已经确定只要求n!中因数5的个数，接下来换一种思路求5的个数。考虑1到n，5的倍数可分解为1个5，5^2的倍数可分解为2个5……，将n依次除以5,5^2……得到所有5的个数。


代码如下：

```java
public class Solution {
    public int trailingZeroes(int n) {
        int count =0;
        long nextCount =0;
        long flag =1;
        do{
            count +=nextCount;
            flag *=5;
            nextCount = n / flag;
        }while(nextCount > 0);
        return count;
    }
}
```