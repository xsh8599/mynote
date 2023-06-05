stack翻译为栈，是STL中实现的一个后进先出的容器。要使用 stack，应先添加头文件include<stack>，并在头文件下面加上“ using namespacestd;"

## stack的定义

其定义的写法和其他STL容器相同, typename可以任意基本数据类型或容器：

```c++
stack<typename> name;
```


## stack容器内元素的访问

由于栈(stack)本身就是一种后进先出的数据结构，在STL的 stack中只能通过top()来访问栈顶元素。

程序代码：


```c++
#include<cstdio> 
#include<stack> 
using namespace std;
int main(){
stack<int> st;
for(int i=1;i<=5;i++){
	st.push(i);		//push(i)将i压入栈 
}
printf("%d\n",st.top()); 	//top()取栈顶元素 
return 0;
  }
```

运行结果： 5



## stack常用函数实例解析

**（1）push()**
push(x)将x入栈，时间复杂度为O(1)，实例见“ stack容器内元素的访问”。

**（2）top()**
top()获得栈顶元素，时间复杂度为O(1)，实例见“ stack容器内元素的访问”。

**（3）pop()**
pop()用以弹出栈顶元素，时间复杂度为O(1)。

程序代码：


	#include<cstdio> 
	#include<stack> 
	using namespace std;
	int main(){
	stack<int> st;
	for(int i=1;i<=5;i++){
		st.push(i);		//push(i)将i压入栈 ,1 2 3 4 5 依次入栈 
	}
	for(int i=1;i<=3;i++){
		st.pop();		//pop()将栈顶元素出栈，即将5 4 3 依次出栈 
	}
	printf("%d\n",st.top()); 	//top()取栈顶元素 
	return 0;
	}

运行结果：2



**（4）empty()**
empty()可以检测stack是否为空，返回true为空，返回false为非空，时间复杂度为O(1)。

程序代码：


```c++
#include<cstdio> 
#include<stack> 
using namespace std;
int main(){
stack<int> st;
printf("%d\n",st.empty()); //true=1;false=0
for(int i=1;i<=5;i++){
	st.push(i);		//push(i)将i压入栈 ,1 2 3 4 5 依次入栈 
}
printf("%d\n",st.empty()); //true=1;false=0
return 0;
  }
```

运行结果：1 0



**（5）size()  **
size()返回stack内元素的个数，时间复杂度为O(1)。

程序代码：


```c++
#include<cstdio> 
#include<stack> 
using namespace std;
int main(){
stack<int> st;
for(int i=1;i<=5;i++){
	st.push(i);		//push(i)将i压入栈 ,1 2 3 4 5 依次入栈 
}
printf("%d\n",st.size()); 
return 0;
```
}
运行结果：5



## 注意：

在使用pop()和top()函数之前必须先使用empty()函数判断栈是否为空。

## stack的常见用途

stack用来模拟实现一些递归，防止程序对栈内存的限制而导致程序运行出错。一般来说，程序的栈内存空间很小，对有些题目来说，如果用普通的函数来进行递归，一旦递归层数过深(不同机器不同，约几千至几万层)，则会导致程序运行崩溃。如果用栈来模拟递归算法的实现，则可以避免这一方面的问题(不过这种应用出现较少)。