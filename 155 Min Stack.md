###题目：

> Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.
> * push(x) -- Push element x onto stack.
> * pop() -- Removes the element on top of the stack.
> * top() -- Get the top element.
> * getMin() -- Retrieve the minimum element in the stack.

###题目大意：

设计一个栈，支持以常数时间得到push、pop、top和getMin方法

* push(x) -- 将x推入栈顶
* pop() -- 将元素从栈顶移除
* top() -- 获取栈顶元素
* getMIn() -- 获取栈中最小的元素

###分析：

设计栈，我们可以采用数组，主要考虑的问题是防止数组溢出，包括过小溢出和过大溢出，当元素过多时候扩充栈。

代码如下：

```java
class MinStack {
    private final int Default_Length =10;
	private int[] stack = new int[Default_Length];
	private int[] minValueArr = new int[Default_Length];
   	private int top = -1;
   	public void push(int x) {
		checkCapacity(stack.length,top);
		if(top==-1){
		    this.minValueArr[0] = x;
		}else{
		    this.minValueArr[top+1] = x < this.minValueArr[top] ? x : this.minValueArr[top];
		}
       	this.stack[++top]=x;
 	}
 	public void pop() {
		if(this.top==-1){
			return;
		}
		this.minValueArr[top]=0;
        this.stack[top--]=0;
  	}
  	public int top() {
		if(this.top==-1){
			return 0;
		}
       	return this.stack[top];
  	}
  	public int getMin() {
		if(this.top==-1){
			return 0;
		}
        return minValueArr[this.top];
  	}
	public void checkCapacity(int arrayLength,int top){
		if(top == arrayLength-1){
		    int length = stack.length*2;
			stack = Arrays.copyOf(this.stack,length);
			minValueArr = Arrays.copyOf(this.minValueArr,length);
		}
	}
}
```
 

####改进一

对于上述代码中的最小元素数组minValueArr，可以考虑当有更小值的时候才放入数组，这样可以节省更多空间。

###参考

 参考他人的，发现直接可以用java中的Stack类。。。
 代码如下：

 ```java
class MinStack {
    Stack<Integer> stack;
    Stack<Integer> minStack;
    MinStack() {
        stack = new Stack<Integer>();
        minStack = new Stack<Integer>();
    }
    public void push(int x) {
        stack.push(new Integer(x));
        if(minStack.isEmpty()||x<=minStack.peek()){
            minStack.push(new Integer(x));
            }
    }
    public void pop() {
        if(stack.peek().equals(minStack.peek())){ // notice that stack.peek() returns an referrence of object, and can't be compared using "=="
            minStack.pop();
        }
        stack.pop();
    }
    public int top() {
        int result = stack.peek();
        return result;
    }
    public int getMin() {
        return minStack.peek();
    }
}
 ```
