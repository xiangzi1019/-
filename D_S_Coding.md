#程序员代码面试指南（学习）
##第一章-栈和队列
###题目1

实现一个特殊的栈，在实现栈的基本功能基础上，在实现返回栈中的最小元素的操作。

**要求**

 1)pop，push和getMin的操作时间都是O(1);

 2) 设计的栈类型可以使用现成栈结构。

	import java.util.Stack;
	
	public class MyStack1 {
		//创建两个栈
		private Stack<Integer> stackData;
		private Stack<Integer> stackMin;
		
		//构成函数构造两个栈列表
		public MyStack1() {
			this.stackData = new Stack<Integer>();
			this.stackMin = new Stack<Integer>();
		}
		
		//实现push方法
		public void push(int newNum) {
			//当保存较小值的栈中为空时，添加元素
			if(this.stackMin.isEmpty()) {
				this.stackMin.push(newNum);
				//如果新插入的值小于最小值栈的顶部元素，就把这个值压入其中
			}else if(newNum <= this.getmin()) {
				this.stackMin.push(newNum);
			}
			//同时把这个值放入数据栈
			this.stackData.push(newNum);
		}
		//pop操作
		public int pop() {
			//数据为空时不能弹出
			if(this.stackData.isEmpty()) {
				throw new RuntimeException("Your stack is empty.");
			}
			//当前数据栈中值等于最小站值时候就把最小栈值放出
			int value = this.stackData.pop();
			if(value == this.getmin()) {
				this.stackMin.pop();
			}
			//得到这个值
			return value;
		}
		public int getmin() {
			if(this.stackMin.isEmpty()) {
				throw new RuntimeException("Your stack is empty.");
			}
			//返回栈顶的元素
			return this.stackMin.peek();
		}
		public static void main(String[] args) {
			MyStack1 test = new MyStack1();
			//用于测试数据
			int[] arr = {3,4,5,1,2,1,7};
			for(int i = 0; i < arr.length;i++) {
				test.push(arr[i]);
			}
			//分别输出数据栈的头部和最小元素
			System.out.println(test.pop());
			System.out.println(test.getmin());
		}
	}

###题目2
编写一个类，用两个栈实现队列，支持队列的基本操作（add,poll,peek）。

	import java.util.Stack;
	
	public class TwoStackQueue {
		//创建两个栈来实现操作
		public Stack<Integer> stackPush;
		public Stack<Integer> stackpop;
		//构造函数
		public TwoStackQueue() {
			// TODO Auto-generated constructor stub
			stackPush = new Stack<Integer>();
			stackpop = new Stack<Integer>();
		}
		
		//push栈向pop栈导入数据
		public void PushToPop() {
			//如果pop栈为空的情况下,并且push栈不为空,我们九八push里面的数据放到pop栈中
			if(stackpop.empty()) {
				while(!stackPush.empty()) {
					stackpop.push(stackPush.pop());
				}
			}
		}
		//实现add操作,向push栈中添加数据
		public void add(int PushInt) {
			stackPush.push(PushInt);
			PushToPop();
		}
		//实现poll操作，得到第一个值
		public int poll() {
			//当两个栈都不为空的时候
			if(stackpop.empty() && stackPush.empty()) {
				throw new RuntimeException("Queue is empty!");
			}
			//实现数据的转移
			PushToPop();
			return stackpop.pop();
		}
		//原理同上
		public int peek() {
			if(stackpop.empty() && stackPush.empty()) {
				throw new RuntimeException("Queue is empty!");
			}
			PushToPop();
			return stackpop.peek();
		}
		public static void main(String[] args) {
			//创建对象
			TwoStackQueue t1 = new TwoStackQueue();
			int[] list = {1,2,3,4,5,6,7,8};
			//向对象中添加数据
			for(int i = 0; i < list.length;i++) {
				t1.add(list[i]);
			}
			System.out.println(t1.peek());
			System.out.println(t1.poll());
		}
	}


###题目三
一个栈元素类型为整数，现在将该栈按照从大到小顺序排序，只允许申请一个栈，除此之外还可以申请一个变量，但不能申请额外的数据结构，如何完成该排序。（实际上就是利用两个栈和一个int类型的变量存可放一个值来实现栈上数据的排序）

**解释** 

要排序的栈记为stack，而辅助栈记为help。在stack中只执行pop操作，弹出的数据记为cur。

1.如果cur小于或等于help的栈顶元素，那么就直接把数据压入help中。

2.如果大于的话，就先把help中的数据压入stack中，直到找到一个小于或等于的数据，再把cur压入help中。在反复执行1 2操作即可。

3.最后当stack为空的时候，就把help中的数据压入stack中就完成了这个排序。

4.那么想一下，我们要执行从小到大的排序呢？操作一下。


	import java.util.Stack;
	
	public class sortStackByStack {
		//写一个方法实现排序功能
		public static void sortSatckByStack(Stack<Integer> stack) {
			//创建一个help栈来协助排序
			Stack<Integer> help = new Stack<Integer>();
			//当我们的stack不为空的时候执行操作
			while(!stack.empty()) {
				int cur = stack.pop();
				//如果我们help的顶部元素小于我们要放入的元素我们就把这个些元素放回到stack中
				while(!help.empty() && help.peek() < cur) {
					stack.push(help.pop());
				}
				//最后呢我们把这个当前current元素放入help中
				help.push(cur);
			}
			//这是在执行完毕后我们得到了从小到大的排序，鉴于此我们要再转换一次。
			while(!help.isEmpty()) {
				stack.push(help.pop());
			}
		}
		public static void main(String[] args) {
			//创建一个栈并且放入数据7 1 5 3 2
			Stack<Integer> stack = new Stack<Integer>();
			stack.push(7);
			stack.push(1);
			stack.push(5);
			stack.push(3);
			stack.push(2);
			//创建实例对象
			sortStackByStack t1 = new sortStackByStack();
			//调用我们的方法
			t1.sortSatckByStack(stack);
			//输出打印
			while(!stack.isEmpty()) {
				System.out.println(stack.pop());
			}
				
		}
	}