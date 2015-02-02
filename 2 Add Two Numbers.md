2015/02/02
###题目

> You are given two linked lists representing two non-negative numbers. The digits are stored in reverse order and each of their nodes 
> contain a single digit. Add the two numbers and return it as a linked list.

> Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
> Output: 7 -> 0 -> 8

###分析

####方法一

该算法的设计主要应该是考虑链节点相加时遇十进一的问题。
再具体点分两种情况考虑：

1. 相加的两个链列长度相等，要注意链列最后两个节点相加的遇十进一问题。
1. 相加的两个链列长度不等，要注意长链列的节点和短链列最后一个节点相加的遇十进一问题。

代码如下：

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        int tenVal = 0;			//如果遇十进一，则tenVal=1
		int val = l1.val + l2.val + tenVal;
		if(val > 9){
			tenVal =1;
			val -= 10;
		}
		ListNode node = new ListNode(val);
		ListNode temp = node;		//用于跟踪保存头结点后面的节点
		l1=l1.next;
		l2=l2.next;
		if(l1 == null && l2 == null && tenVal ==1){
		    temp.next = new ListNode(1);
		}
		while(l1 != null && l2 != null){
			val = l1.val + l2.val + tenVal;
			tenVal =0;
			if(val >9){
				val -= 10;
				tenVal =1;
			}
			temp.next = new ListNode(val);
			temp = temp.next;
			l1 = l1.next;
			l2 =l2.next;
			if(l1 == null && l2 == null && tenVal ==1){              //如果到达两条链列的末尾节点，遇十进一
		        temp.next = new ListNode(1);
		    }
		}
		while(l1 != null){
		    if(tenVal ==1){                //考虑两链列最后一次相加的进一问题
			    if(l1.val ==9){
			        temp.next = new ListNode(0);
			    }else{
			        temp.next = new ListNode(l1.val+1);
			        tenVal =0;
			    }
		    }else{
		        temp.next = new ListNode(l1.val);
		    }
			temp = temp.next;
			l1 = l1.next;
			if(l1 == null && tenVal ==1){           //考虑单链列末节点进一问题
			    temp.next = new ListNode(1);
			}
		}
		while(l2 != null){
		    if(tenVal ==1){
		        if(l2.val ==9){
		            temp.next = new ListNode(0);
		        }else{
		            temp.next = new ListNode(l2.val+1);
			        tenVal =0; 
		        }
		    }else{
		        temp.next = new ListNode(l2.val);
		    }
			temp = temp.next;
			l2 = l2.next;
			if(l2 == null && tenVal ==1){
			    temp.next = new ListNode(1);
			}
		}
		return node;
    }
}
```