Given an integer n, return the number of trailing zeroes in n!.

Note: Your solution should be in logarithmic time complexity.

```java
public class Solution {
    public int trailingZeroes(int n) {
        int count2 = 0;
        int count5 = 0;
        for(int i=n;i>1;i--){
            count2 +=countOfDivided(i,2);
            count5 +=countOfDivided(i,5);
            
        }
        return count2<count5 ? count2 : count5;
     }
     
     public int countOfDivided(int dividend, int divider){
         long remainder = dividend % divider;
         long result = dividend /divider;
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