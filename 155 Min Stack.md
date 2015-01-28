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

####参考一

参考他人的，发现直接可以用java中的Stack类，下面代码用了两个stack类，一个储存元素，一个储存最小值。

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

####参考二

只用了一个栈同时存放最小值和元素，非常巧妙！先设置最小值为最大整数，push的时候，比较最小值和x的大小，如果x值小于当前最小值，将当前最小值存入栈，设置当前最小值为x，把x推入栈，即当出现新的最小值的时候，把原先的最小值存入栈中。pop的时候，如果栈顶第一个值与当前最小值相同，说明pop的时候会改变最小值，此时pop出当前最小值，再次pop，得到当前最小值push之前的最小值。两次pop之后，栈顶为之前加入的元素。

```java
 class MinStack {
    int min=Integer.MAX_VALUE;
    Stack<Integer> stack = new Stack<Integer>();
    public void push(int x) {
       // only push the old minimum value when the current 
       // minimum value changes after pushing the new value x
        if(x <= min){
            stack.push(min);
            min=x;
        }
        stack.push(x);
    }
    public void pop() {
       // if pop operation could result in the changing of the current minimum value, 
        pop twice and change the current minimum value to the last minimum value.
        if(stack.peek()==min) {
            stack.pop();
            min=stack.peek();
            stack.pop();
        }else{
            stack.pop();
        }
        if(stack.empty()){
            min=Integer.MAX_VALUE;
        }
    }
    public int top() {
        return stack.peek();
    }
    public int getMin() {
        return min;
    }
}
```
