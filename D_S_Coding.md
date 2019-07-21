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


###题目四
有一个整形数组 arr和一个大小为w的窗口从数组的最左边开始向右边滑动，每次滑动一个位置。例如：

4 3 5 4 3 3 6 7的数组和w=3的窗口，有下面的情况：

【4 3 5】 4 3 3 6 7       得到结果5

4 【3 5 4】 3 3 6 7       得到结果5

4 3 【5 4 3 】3 6 7       得到结果5

4 3 5 【4 3 3 】6 7       得到结果4

4 3 5 4 【3 3 6 】7       得到结果6

4 3 5 4 3 【3 6 7】       得到结果7

**要求**

如果数组长度为n，窗口大小为w，一共会产生n-w+1个数。

要求：输入一个数组arr，窗口大小为w

输出一个长度为n-w+1的res数组，表示状态下得到的最大值。

例如本例题中输出结果为：5 5 5 4 6 7。

**分析**

现在这个题目我们能想到的就是分别遍历n次，每次再遍历w，寻找到最大值保存即可。这样时间复杂度就是O(n*w)，显然我们不应该这样做，应当去想一下有没有更好的方法来使得时间复杂度为O(n)。

	import java.util.LinkedList;
	
	public class getMaxWindow {
		public static int[] getMaxWindow1(int[] arr,int w) {
			//判断是否符合标准
			if(arr == null || w < 1 || arr.length < w) {
				return null;
			}
			//创建一个双端队列执行操作
			LinkedList<Integer> max = new LinkedList<Integer>();
			//用于保存最后的结果输出
			int[] res = new int[arr.length - w +1];
			//下标用于res操作
			int index = 0;
			//执行1次遍历操作全部数据
			for(int i = 0;i < arr.length;i++) {
				//当我们的max双端队列不为空并且队列的头部小于等于传入数组的i下标时候
				while(!max.isEmpty() && arr[max.peekFirst()] <= arr[i]) {
					//删除尾部数据
					max.pollLast();
				}
				//保存当前下标值
				max.addLast(i);
				//当双端队列头部等于长度时候（保存值过期）
				if(max.peekFirst() == i - w) {
					//删除头部节点
					max.pollFirst();
				}
				//保存当前的结果值
				if(i >= w-1) {
					res[index++] = arr[max.peekFirst()];
				}
			}
			return res;
		}
		public static void main(String[] args) {
			//创建实例对象
			getMaxWindow t1 = new getMaxWindow();
			int[] arr = {4,3,5,4,3,3,6,7};
			int w = 3;
			int[] res = new int[arr.length-w+1];
			res = t1.getMaxWindow1(arr,w);
			//输出相应的结果
			for(int i = 0;i < res.length;i++) {
				System.out.print(res[i]+" ");
			}
		}
	}

###题目五
打印两个有序链表的公共部分，给定两个链表的头指针head1和head2，打印两个链表的公共部分。

**分析**

因为时有序的，所以只需要那个大就向后移动一个值查找即可。找到相同的就输出然后两边分别后移一位。

	public class printCommonPart {
		//创建链表节点
		public class Node{
			public int value;
			public Node next;
			public Node(int val) {
				this.value = val;
			}
		}
		
		public void printCommonPart(Node head1,Node head2) {
			//打印相同的数
			System.out.println("Common Part :");
			//当节点1 2 都不为空的情况下执行
			while(head1 != null && head2 != null) {
				//哪一方值大就向后移动和一个节点，当两个节点相同大小时候输出，并且同时后移
				if(head1.value < head2.value) {
					head1 = head1.next;
				}else if(head1.value > head2.value) {
					head2 = head2.next;
				}else {
					System.out.println(head1.value + " ");
					head1 = head1.next;
					head2 = head2.next;
				}
			}
			System.out.println();
		}
###题目流六
给定一个不含重复值的数组arr，找到每一个i位置左右两边离i最近的比arr[i]值小的位置，返回所有位置信息。不存在返回-1；

例如：arr={3,4,1,5,6,2,7}

结果为：{{-1，2}{0,2}{-1,-1}{2,5}{3,5}{2,-1}{5,-1}}

当i = 3的时候，arr[i] = 5,其左右小的值下标为：2，5因此输出。

**要求** 

请分析题目，给出相应时间复杂度的解。	

	public class findLessIndex {
		public int[][] rightWay(int[] arr){
			//定义数组用来保存下标对应的左右值
			int[][] res = new int[arr.length][2];
			//for循环查找
			for(int i = 0;i < arr.length; i++) {
				int leftLessIndex = -1;
				int rightLessIndex = -1;
				int cur = i-1;
				//左边探测
				while(cur >= 0) {
					//如果这个值小于我们的下标位置，就保存这个值
					if(arr[cur] < arr[i]) {
						leftLessIndex = cur;
						break;
					}
					//继续向前探测
					cur--;
				}
				//开始执行右边部分，理解同上
				cur = i +1;
				while(cur < arr.length) {
					if(arr[cur] < arr[i]) {
						rightLessIndex = cur;
						break;
					}
					cur++;
				}
				//保存我们的所得下标
				res[i][0] = leftLessIndex;
				res[i][1] = rightLessIndex;
			}
			return res;
		}
		
		public static void main(String[] args) {
			findLessIndex t1 = new findLessIndex();
			int[] arr = {3,4,1,5,6,2,7};
			int[][] temp ;
			temp = t1.rightWay(arr);
			for (int i = 0; i < temp.length; i++) {
				System.out.print(temp[i][0] + " ");
				System.out.println(temp[i][1]);
			}
		}
	}
	显然这样做的时间复杂度为O(n²),不太符合理想的解题思维。所以我们接下来去考录另一个方法。

------------
	public int[][] getNearLessWay(int[] arr){
			//创建保存数组
			int[][] res = new int[arr.length][2];
			//创建用于操作数据的栈
			Stack<Integer> stack = new Stack<Integer>();
			for (int i = 0; i < arr.length; i++) {
				//当我们栈不为空，并且栈顶元素值大于当前arr的值时候
				while(!stack.isEmpty() && arr[stack.peek()] > arr[i]) {
					//弹出栈顶元素
					int popIndex = stack.pop();
					//如果栈为空了，说明其左边没有值小于这个数，如果有就证明这个值小于我们要查找的值
					int leftLessIndex = stack.isEmpty() ? -1:stack.peek();
					//赋值操作
					res[popIndex][0] = leftLessIndex;
					//由于当前值小于才操作，所以其右端就是我们这个i值
					res[popIndex][1] = i;
				}
				//将当前值再入栈
				stack.push(i);
			}
			//理解方法同上
			while(!stack.isEmpty()) {
				int popIndex = stack.pop();
				int leftLessIndex = stack.isEmpty() ? -1:stack.peek();
				res[popIndex][0] = leftLessIndex;
				res[popIndex][1] = -1;
			}
			return res;
		}
###题目七
给定两个字符串str1和str2，返回两个字符串中最长的公共子串序列。

**例如**

str1 = 1A2C3D4B56  以及 str2 = B1D23CA45B6A

得到结果123456或者12C4B6  都是最长公共子序列。返回其一即可。


题目讲解可以去查阅B站视频：UID **315496975**

	public class longestcomsubsequence {
		//用于得到二位数组的方法
		public int[][] getdb(char[] str1,char[] str2){
			//创建dp用于保存得到的二维数组值
			int[][] dp = new  int[str1.length][str2.length];
			dp[0][0] = str1[0] == str2[0] ? 1:0;
			//先保证行列的0位置有值
			for(int i = 1; i < str1.length;i++) {
				dp[i][0] =Math.max(dp[i-1][0],str1[i] == str2[0] ? 1:0);
			}
			for(int i = 1; i < str2.length;i++) {
				dp[0][i] =Math.max(dp[0][i-1],str1[0] == str2[i] ? 1:0);
			}
			//两个for循环开始操作数组，找到最大值放到当前位置，如果数据相同再+1
			for(int i = 1; i < str1.length;i++) {
				for(int j = 1; j < str2.length;j++) {
					//判断左和上的最大值然后赋值
					dp[i][j] = Math.max(dp[i-1][j],dp[i][j-1]);
					if(str1[i] == str2[j]) {
						//当值相同的时候，就找到最大值或者左上方值+1比较
						dp[i][j] = Math.max(dp[i][j],dp[i-1][j-1]+1);
					}
				}
			}
			return dp;
		}
		//得到结果方法
		public String getLastvalue(String str1,String str2) {
			//首先判断str1和str2是否有值，我们这里不做判断
			
			//转换成char类型的数据
			char[] chs1 = str1.toCharArray();
			char[] chs2 = str2.toCharArray();
			int[][] dp = getdb(chs1, chs2);
			int m = chs1.length-1;
			int n = chs2.length-1;
			//c创建res用于保存最后结果
			char[] res = new char[dp[m][n]];
			int index = res.length-1;
			//开始探索
			while(index>=0) {
				//如果矩阵值上下相等，就上移一位
				if(n > 0 && dp[m][n] == dp[m][n-1]) {
					n--;
					//如果矩阵值左右相等，就左移一位
				}else if(m > 0 && dp[m][n] == dp[m-1][n]) {
					m--;
					//如果值相同就左上方一位
				}else {
					res[index--] = chs1[m];
					m--;
					n--;
				}
			}
			return String.valueOf(res);
		}
		public static void main(String[] args) {
			String str1 = "1A2C3D4B56";
			String str2 = "B1D23CA45B6A";
			longestcomsubsequence t1 = new longestcomsubsequence();
			String Temp = t1.getLastvalue(str1, str2);
			System.out.println(Temp);
		}
	}





###题目八
给定两个字符串str1和str2，返回两个字符串中最长的公共子序列。

**例如**

str1 = 1AB2345CD  以及 str2 = 12345EF

得到结果为：2345

想一下能否利用上一题的大致思想解决这个问题呢？
