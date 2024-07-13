# C++

## 字符串

---

```c++
cin会自动过滤
scanf读入字符时 不会自动化过滤空格和制表符(Tab键)
scanf格式化输入 printf格式化输出
    
char s[100];
cin >> s + 2;
cout << s + 2 << endl ;
cout << s[2] << endl ;
//从s+2开始读入 读入字符首项放在是s[2]
//字符串遇到空格 回车 文件结束符 才会停止

//读入一行字符串
gets(s) //不安全已被淘汰
fgets(s, 100, stdin) //最新读入函数 字符串首地址 字符长度 系统库  读到字符数组

string c;
getline(cin, c); //该函数也可以实现读入一行字符串 读到字符串

scanf
整数数组本身还是数字 要加取地址符号
字符数组和字符串不需要加取地址符号 数组名本身就是地址
1.字符数组名本身就是指向数组第一个元素的指针：在C语言中，数组名（例如char_array）本身就是指向该数组的指针。因此，char_array等价于&char_array[0]。两者都是指向数组第一个元素的指针。

2.scanf函数处理字符数组：scanf函数在读取字符串时，需要一个指向字符数组的指针。无论你是直接传递数组名还是传递&char_array[0]，它们都是指向数组首元素的指针。因此，scanf("%s", &char_array[0]);和scanf("%s", char_array);在功能上是等价的。


string s;
getline(cin, s);

char s[100];
cin.getline(s, 100);


#include <iostream>
#include <cstring>

using namespace std;

int main() 
{
    // string s ;
    // getline(cin ,s);
    // cout << s.size() << endl ;
    
    char a[105] ;
    cin.getline(a, 105);
    int num = strlen(a) ;
    cout << num << endl ;//string 用strlen会出错
/*
    string是basic_string<char>的一个typedef，长度可以由length得到。
    你用strlen要求的参数是一个char *型的，即C风格的字符串，以\0结尾的字符串。
    而string并不要求以\0结尾，可以先string.c_str()先转成C风格的字符串再使用strlen
*/
    return 0;
}


// (1) strlen(str)，求字符串的长度
// (2) strcmp(a, b)，比较两个字符串的大小，a < b返回-1，a == b返回0，a > b返回1。这里的比较方式是字典序！
// (3) strcpy(a, b)，将字符串b复制给从a开始的字符数组。


char s[100];
fgets(s,10000,stdin);

char s[100];
cin.getline(s,100)
//用于字符数组/字符串

string s;						
getline(cin, s);
//只用于字符串 

string s;
s.empty() 返回0/1查看字符串是否为空
s.pop_back 去除字符串最后一个符号
s.size() 返回字符串长度
s.substr(begin,len)两个参数分别为起始位和长度
    

```

`char*` 的例子

```c
char myString[] = "Hello, World!"; // 字符数组（字符串）  
char* myPointer = myString; // 指针指向字符串的首地址  
printf("%s\n", myPointer); // 输出: Hello, World!  
  
// 修改字符串中的字符（注意：这种方式修改了原始数组）  
myPointer[0] = 'h'; // 现在字符串变为 "hello, World!"  
printf("%s\n", myString); // 输出: hello, World!
```

`const char*` 的例子

```c
const char* myConstPointer = "Hello, World!"; // 指针指向一个常量字符串  
// printf("%c\n", myConstPointer[0]); // 这是合法的，可以读取  
// myConstPointer[0] = 'h'; // 这是非法的，因为字符串是常量，不能修改  
  
// 但是可以改变指针指向另一个字符串  
myConstPointer = "Another string";  
printf("%s\n", myConstPointer); // 输出: Another string
```

注意：在C中，字符串字面量（如 `"Hello, World!"`）通常存储在只读内存区域，因此即使你不使用`const`，尝试修改这样的字符串也是未定义行为（通常会导致程序崩溃）。使用`const`是一个好习惯，因为它可以捕获这类潜在的错误。

## STL

### vector容器

---

![image-20240523160819949](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240523160819949.png)

```cpp
vector deque提供iterator迭代器
std::stack<int, std::vector<int> > third; 
栈的内部结构，栈的底层实现可以是vector，deque，list 都是可以的， 主要就是数组和链表的底层实现
栈是以底层容器完成其所有的工作，对外提供统一的接口，底层容器是可插拔的（可以控制使用哪种容器来实现栈的功能）。
所以STL中栈往往不被归类为容器，而被归类为container adapter（容器适配器）。

栈提供push 和 pop 等等接口，所有元素必须符合先进后出规则，所以栈不提供走访功能，也不提供迭代器(iterator)。 不像是set 或者map 提供迭代器iterator来遍历所有元素。
队列中先进先出的数据结构，同样不允许有遍历行为，不提供迭代器, SGI STL中队列一样是以deque为缺省情况下的底部结构。std::queue 不提供迭代器。由于 std::queue 是一个队列容器适配器，它被设计成只支持队列的基本操作，如入队（push）、出队（pop）、查看队首元素（front）等，而不支持对队列中的元素进行随机访问或遍历。
    
    1. #include <vector>
vector是变长数组，支持随机访问，不支持在任意位置 O(1)插入。为了保证效率，元素的增删一般应该在末尾进行。

1.1 声明
#include <vector>   // 头文件
vector<int> a;      // 相当于一个长度动态变化的int数组
vector<int> b[233]; // 相当于第一维长233，第二位长度动态变化的int数组
struct rec{…};
vector<rec> c;      // 自定义的结构体类型也可以保存在vector中
1.2 size/empty
size函数返回vector的实际长度（包含的元素个数），empty函数返回一个bool类型，表明vector是否为空。二者的时间复杂度都是 O(1)所有的STL容器都支持这两个方法，含义也相同，之后我们就不再重复给出。

1.3 clear
clear函数把vector清空。

1.4 迭代器
迭代器就像STL容器的“指针”，可以用星号*操作符解除引用。

一个保存int的vector的迭代器声明方法为：

vector<int>::iterator it;
vector的迭代器是“随机访问迭代器”，可以把vector的迭代器与一个整数相加减，其行为和指针的移动类似。可以把vector的两个迭代器相减，其结果也和指针相减类似，得到两个迭代器对应下标之间的距离。

1.5 begin/end
begin函数返回指向vector中第一个元素的迭代器。例如a是一个非空的vector，则*a.begin()与a[0]的作用相同。

所有的容器都可以视作一个“前闭后开”的结构，end函数返回vector的尾部，即第n 个元素再往后的“边界”。*a.end()与a[n]都是越界访问，其中n = a.size()。

下面两份代码都遍历了vector<int> a，并输出它的所有元素。

for (int i = 0; i < a.size(); i ++)
    cout << a[i] << endl;

for (vector<int>::iterator it = a.begin(); it != a.end(); it ++)
    cout << *it << endl;
1.6 front/back
front函数返回vector的第一个元素，等价于*a.begin()和a[0]。
back函数返回vector的最后一个元素，等价于*--a.end()和a[a.size() – 1]。

1.7 push_back()和pop_back()
a.push_back(x)把元素x插入到vector a的尾部。
b.pop_back()删除vector a的最后一个元素。
    
vector()                       // 创建一个空vector
vector(int nSize)              // 创建一个vector, 元素个数为nSize
vector(int nSize, const t& t)  // 创建一个vector，元素个数为nSize, 且值均为t
vector(const vector&)          // 复制构造函数
vector(begin, end)             // 复制[begin, end)区间内另一个数组的元素到vector中
```

### queue队列

```
queue主要包括循环队列queue和优先队列priority_queue两个容器
注意pop操作返回void类型
循环队列
push    // 从队尾插入
pop     // 从队头弹出
front   // 返回队头元素
back    // 返回队尾元素
优先队列
push    // 把元素插入堆
pop     // 删除堆顶元素
top     // 查询堆顶元素（最大值）
```

### stack栈

```
push    // 向栈顶插入
pop     // 弹出栈顶元素
```

### deque双端队列

双端队列deque是一个支持在两端高效插入或删除元素的连续线性存储空间。它就像是vector和queue的结合。与vector相比，deque在头部增删元素仅需要 O(1)的时间；与queue相比，deque像数组一样支持随机访问。

```
[]              // 随机访问
begin/end       // 返回deque的头/尾迭代器
front/back      // 队头/队尾元素
push_back       // 从队尾入队
push_front      // 从队头入队
pop_back        // 从队尾出队
pop_front       // 从队头出队
clear           // 清空队列
```

### set集合

头文件set主要包括set和multiset两个容器，分别是“有序集合”和“有序多重集合”，即前者的元素不能重复，而后者可以包含若干个相等的元素。set和multiset的内部实现是一棵红黑树，它们支持的函数基本相同。

```cpp
5.1 声明
set<int> s;
struct rec{…}; set<rec> s;  // 结构体rec中必须定义小于号
multiset<double> s;
5.2 size/empty/clear
与vector类似

5.3 迭代器
set和multiset的迭代器称为“双向访问迭代器”，不支持“随机访问”，支持星号*解除引用，仅支持++和--两个与算术相关的操作。

设it是一个迭代器，例如set<int>::iterator it;

若把it ++，则it会指向“下一个”元素。这里的“下一个”元素是指在元素从小到大排序的结果中，排在it下一名的元素。同理，若把it --，则it将会指向排在“上一个”的元素。

5.4 begin/end
返回集合的首、尾迭代器，时间复杂度均为 O(1)
s.begin()是指向集合中最小元素的迭代器。
s.end()是指向集合中最大元素的下一个位置的迭代器。换言之，就像vector一样，是一个“前闭后开”的形式。因此-- s.end()是指向集合中最大元素的迭代器。

5.5 insert
s.insert(x)把一个元素x插入到集合s中，时间复杂度为 O(logn)
在set中，若元素已存在，则不会重复插入该元素，对集合的状态无影响。

5.6 find
s.find(x)在集合s中查找等于x的元素，并返回指向该元素的迭代器。若不存在，则返回s.end()。时间复杂度为 O(logn)

5.7 lower_bound/upper_bound
这两个函数的用法与find类似，但查找的条件略有不同，时间复杂度为 O(logn)

s.lower_bound(x)查找大于等于x的元素中最小的一个，并返回指向该元素的迭代器。

s.upper_bound(x)查找大于x的元素中最小的一个，并返回指向该元素的迭代器。

5.8 erase
设it是一个迭代器，s.erase(it)从s中删除迭代器it指向的元素，时间复杂度为 O(logn)

设x是一个元素，s.erase(x)从s中删除所有等于x的元素，时间复杂度为 O(k+logn)，其中k是被删除的元素个数。

5.9 count
s.count(x)返回集合s中等于x的元素个数，时间复杂度为 O(k+logn)，其中 k为元素x的个数。

```



### map

```cpp
map容器是一个键值对key-value的映射，其内部实现是一棵以key为关键码的红黑树。Map的key和value可以是任意类型，其中key必须定义小于号运算符。

6.1 声明
map<key_type, value_type> name;

//例如：
map<long long, bool> vis;
map<string, int> hash;
map<pair<int, int>, vector<int>> test;
6.2 size/empty/clear/begin/end
均与set类似。

6.3 insert/erase
与set类似，但其参数均是pair<key_type, value_type>。

6.4 find
h.find(x)在变量名为h的map中查找key为x的二元组。

6.5 []操作符
h[key]返回key映射的value的引用，时间复杂度为 O(logn)
[]操作符是map最吸引人的地方。我们可以很方便地通过h[key]来得到key对应的value，还可以对h[key]进行赋值操作，改变key对应的value。


```





## C++语法

---

### 关键字

#### new

**1.动态创建一个类对象**

- 获得一块堆内存空间；
- 调用构造函数；
- 返回正确的指针。

**有一个类class Car，构造函数是Car()，创建一个该类的对象，并开辟一块空间存储，并返回空间的首地址；**

```abnf
Car *Audi = new Car();

delete Audi;
```

**2. 动态创建一个基本数据类型变量**

- 获得一块堆内存空间；
- 返回正确的指针。

没有了构造函数，但是可以同样在括号内赋初值。

```d
new int(10);//返回这个空间的首地址

int *arr=new int(10);//开辟一个存放整数的存储空间，附上初值,返回一个指向该存储空间的地址(即指针)

delete arr;
```

**3.动态创建一个一维数组**

```cpp
char* p = new char[10];//开辟一个存放字符数组(包括10个元素)的空间，返回首元素的地址

delete[] p;
```

 **4.动态创建一个二维数组**

```cpp
//开辟一个存放二维整型数组(大小为3*2)的空间，返回首元素的地址 　

int** pc = new int*[3];//这边表示开辟行数为3
//int*[3]表示的为开辟三个存放int*元素的数组,所以才有了下一步pc[i]中对列数的开辟
	for (int i = 0; i < 3; i++)
	{
		pc[i] = new int[2];//这边表示开辟列数为2
		for (int j = 0; j < 2; j++)
		{
			pc[i][j] = i + j;
			cout <<pc[i][j] << " ";
		}
		cout << endl;
	}

//或者使用另一种方法
int(*pc)[2] = new int[3][2];//创建数组指针pc,注：数组指针与二级指针不一样
for (int i = 0; i < 3; i++)
	{
		for (int j = 0; j < 2; j++)
		{
			pc[i][j] = i + j;
			cout <<pc[i][j] << " ";
		}
		cout << endl;
	}　

delete[] pc;
```

**5.动态创建一个结构体对象**

```cpp
#include<iostream>
using namespace std;
struct MyStruct
{
	int a;
	MyStruct* b;
    MyStruct(int x): a(x), b(NULL) {};  //初始化列表构造函数  函数名与类名相同是构造函数
};
int main() {
	MyStruct* my = new MyStruct();
	delete my;
	system("pause");
	return 0;
}
```

#### explicit

在C++中，`explicit`关键字用于构造函数声明，用以防止构造函数在不明确的情况下被隐式调用。它主要用于单参数构造函数，以避免隐式类型转换可能导致的错误或意外行为。

```cpp
#include <iostream>

class A {
public:
    A(int x) {
        std::cout << "A(int x) called with " << x << std::endl;
    }
};

void func(A a) {
    std::cout << "func(A a) called" << std::endl;
}

int main() {
    func(10); // Implicitly converts int to A
    return 0;
}
在这个例子中，`func(10)`将隐式地调用`A`的构造函数，将整数`10`转换为`A`对象。这种隐式转换有时会导致意外行为或错误。
    
#include <iostream>

class A {
public:
    explicit A(int x) {
        std::cout << "A(int x) called with " << x << std::endl;
    }
};

void func(A a) {
    std::cout << "func(A a) called" << std::endl;
}

int main() {
    // func(10); // Error: no matching function for call to 'func(int)'
    func(A(10)); // Explicitly converts int to A
    return 0;
}
在这个例子中，由于构造函数`A(int x)`被标记为`explicit`，编译器将不允许隐式类型转换。调用`func`时必须显式地创建一个`A`对象，例如通过`func(A(10))`。
```

#### ->和.

---

箭头（->）左边必须为指针；点号（.）左边必须为实体。

while(std::cin >>num) 这句话会读取到EOF结束而不是enter windows为Ctrl+Z

#### 逻辑表达式

---

```cpp
逻辑表达式只看最后一个
if(cnt != 0 , x == 5) cout << "|" << endl ;
即使cnt等于0 决定进不进if看的是x是否等于5
```

#### >>

```cpp
>> 是一个位移操作符，用于将变量的值右移指定的位数。
>>= 是一个复合赋值操作符，用于将变量的值右移指定的位数，并将结果赋值回原变量。

x >> n 只进行右移操作，并返回右移后的值，不会修改 x 的原始值。
x >>= n 会进行右移操作，并将右移后的值赋值回 x，修改 x 的原始值。
```

#### &&

`&&` 运算符具有短路特性，这意味着如果左侧的表达式为假，右侧的表达式将不会被执行

```cpp
 n > 0 && (temp += getSum(n - 1));
```

#### swap

swap操作适用于值和指针



#### for(auto iter:vec)

for(auto iter:vec)不改变迭代对象的值，for(auto &iter:vec)可以改变迭代对象的值。用于遍历vec

#### cin

---

cin读入的一些写法

1.while(cin >> x)
cin >> 有返回值 返回0表示没有读到值
所有文件在后端有一个文件结束符 EOF -1
读到结束会返回一个0 false
2.此处可以用while(cin >> x && x)
**在while循环中，cin只有在到文件结束符(end-of-file)，或遇到一个无效输入时（例如输入的值不是一个整数），cin的状态会变为无效退出循环。**
3.第三种while(cin >> x , x)
用逗号隔开的表达式 最后一项的值为逻辑表达式的值

```
while(cin >> n , n <= 0);//直至n大于0 才继续进行注意分号;

统计个数
while(cin >> x) cnt ++ ;
cout << cnt << endl ;

while(scanf("%d",&x) != -1) cnt ++ ;
scanf读到-1表示结束
或者另一种写法
while(~scanf("%d", &x)) cnt ++ ;
异或位运算
```

#### 传值与传引用

---

```
static void create_player(std::vector<vec::Person> &v) 这句话有没有&有什么区别
```

这句话中的 & 符号表示引用，它使得函数参数 v 成为一个引用类型。在 C++ 中，引用是一个别名，它允许我们通过另一个变量名来访问同一个对象。在函数参数中使用引用可以让我们直接操作原始对象，而不是对象的副本。

如果去掉 & 符号，函数参数就不再是引用，而是传递的是对象的副本。这意味着在函数中对参数进行修改不会影响到原始对象，因为函数操作的是参数的副本。

下面是有无 & 符号的区别示例：

```cpp

#include <iostream>
#include <vector>

class Person {
public:
    std::string name;
    int score;

    Person(const std::string& n, int s) : name(n), score(s) {}
};

static void create_player(std::vector<Person> v) { // 没有 & 符号，传递的是副本
    v.push_back(Person("John", 100)); // 这里操作的是 v 的副本
}

static void create_player_with_ref(std::vector<Person>& v) { // 有 & 符号，传递的是引用
    v.push_back(Person("John", 100)); // 这里操作的是原始向量对象 v
}

int main() {
    std::vector<Person> players1;
    create_player(players1);
    std::cout << "Size of players1 after create_player: " << players1.size() << std::endl; // 输出 0

    std::vector<Person> players2;
    create_player_with_ref(players2);
    std::cout << "Size of players2 after create_player_with_ref: " << players2.size() << std::endl; // 输出 1

    return 0;
}
在上面的示例中，create_player 函数没有 & 符号，因此参数 v 是一个向量对象的副本。在函数内部对 v 进行的任何修改都不会影响到原始对象 players1。而 create_player_with_ref 函数有 & 符号，参数 v 是原始向量对象的引用，因此在函数内部对 v 进行的修改会影响到原始对象 players2。
```



### 高级语法

---

#### 友元

---

在C++中，友元（Friend）是一种特殊的机制，它允许一个类或函数访问另一个类的私有（private）或受保护（protected）成员。这种机制打破了类的封装性，但在某些特定场景下，如需要实现类之间的紧密协作或提高程序的执行效率时，友元的使用是必要的。以下是关于友元的详细解释：

一、友元函数

**定义**：友元函数是一种非成员函数，但它在类的内部被声明为友元，因此可以访问该类的私有和保护成员。

**特点**：

- 友元函数不是类的成员函数，因此它不能通过类的作用域操作符（::）来调用。
- 友元函数可以访问类的私有和保护成员，但不能访问类的静态私有成员（除非通过类对象）。
- 友元函数的声明通常位于类的内部，但其定义则位于类外部。
- 友元函数可以定义在类外部的任何位置，只要它在被调用之前已经被声明。

**语法示例**：

```cpp
class MyClass {  
private:  
    int privateData;  
public:  
    MyClass(int data) : privateData(data) {}  
    // 声明友元函数  
    friend void friendFunction(const MyClass& obj);  
};  
  
// 友元函数实现  
void friendFunction(const MyClass& obj) {  
    // 访问私有成员  
    std::cout << "Friend function accessing private data: " << obj.privateData << std::endl;  
}
```

二、友元类

**定义**：友元类是一个类，它被另一个类声明为友元。这意味着友元类可以访问另一个类的所有成员，包括私有和保护成员。

**特点**：

- 友元关系是单向的，即如果类B是类A的友元，那么类A不是类B的友元。
- 友元关系不具有继承性，子类不能直接继承父类的友元类。
- 友元类通常用于需要多个类之间紧密协作的场景，允许这些类访问彼此的私有成员。

**语法示例**：

```cpp
class MyClass {  
private:  
    int privateData;  
public:  
    MyClass(int data) : privateData(data) {}  
    // 声明友元类  
    friend class FriendClass;  
};  
  
class FriendClass {  
public:  
    void accessPrivateData(const MyClass& obj) {  
        // 访问私有成员  
        std::cout << "Friend class accessing private data: " << obj.privateData << std::endl;  
    }  
};
```

三、友元模板

在C++中，模板也可以被声明为友元。这包括函数模板和类模板。

**函数模板作为友元**：

当函数模板被声明为友元时，它可以访问类的私有成员。但需要注意的是，模板的实例化发生在模板被调用时，因此友元模板函数必须在调用点之前被声明。

**类模板作为友元**：

类模板也可以被声明为友元，但这通常涉及到模板的实例化问题。类模板作为友元时，其所有实例化都将被视为友元。

四、注意事项

- 友元机制破坏了类的封装性，因此必须谨慎使用。
- 友元关系不能继承、传递或交换。
- 在使用友元时，要确保不会破坏类的整体设计和封装原则。

总之，友元是C++中一种强大的特性，它允许类之间的紧密协作和高效访问。但同时，友元也破坏了类的封装性，因此在使用时必须权衡利弊，谨慎决策。

#### 模板

---

在C++中，模板（Templates）是一种强大的特性，它允许程序员编写与类型无关的代码。通过使用模板，你可以定义函数、类或数据结构，使得它们能够处理多种不同的数据类型。模板是泛型编程的基础，泛型编程是一种编程范式，旨在编写与数据类型无关的代码，从而提高代码的重用性和灵活性。

模板的分类

C++中的模板主要分为两种：

1. **函数模板（Function Templates）**：允许你定义一个函数，该函数可以操作多种类型的数据。在函数模板中，类型参数（也称为模板参数）是函数定义的一部分，而不是函数参数。
2. **类模板（Class Templates）**：允许你定义一个类，该类可以操作多种类型的数据。与函数模板类似，类模板也使用类型参数来定义与数据类型无关的类。

函数模板的示例

```cpp
#include <iostream>  
  
// 函数模板定义  
template<typename T>  
T max(T a, T b) {  
    return (a > b) ? a : b;  
}  
  
int main() {  
    std::cout << "Max of 5 and 3 is " << max<int>(5, 3) << std::endl;  
    std::cout << "Max of 5.5 and 4.6 is " << max<double>(5.5, 4.6) << std::endl;  
    return 0;  
}
```

在这个例子中，`max`函数模板接受两个相同类型的参数，并返回它们中的较大值。通过指定模板参数（如`int`或`double`），编译器可以生成针对特定类型的`max`函数的实例。

类模板的示例

```cpp
#include <iostream>  
#include <vector>  
  
// 类模板定义  
template<typename T>  
class Box {  
private:  
    T m_value;  
  
public:  
    Box(T value) : m_value(value) {}  
  
    void setValue(T value) {  
        m_value = value;  
    }  
  
    T getValue() const {  
        return m_value;  
    }  
};  
  
int main() {  
    Box<int> intBox(10);  
    std::cout << "Integer Box: " << intBox.getValue() << std::endl;  
  
    Box<std::string> stringBox("Hello, World!");  
    std::cout << "String Box: " << stringBox.getValue() << std::endl;  
  
    return 0;  
}
```

在这个例子中，`Box`类模板定义了一个可以存储任何类型数据的盒子。通过指定模板参数（如`int`或`std::string`），可以创建`Box`类的不同实例，这些实例能够存储并处理相应类型的数据。

模板的实例化

当编译器遇到模板的使用时（如函数模板的调用或类模板的实例化），它会根据提供的模板参数（显式或隐式）来生成模板的特定实例。这个过程称为模板的实例化。

总结

模板是C++中非常强大的特性，它允许程序员编写灵活且可重用的代码。通过使用模板，你可以定义与类型无关的函数和数据结构，从而提高代码的可读性、可维护性和可重用性。



#### 多态与虚函数

---

![image-20240712173840886](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240712173840886.png)![image-20240712173902373](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240712173902373.png)

在C++中，多态性通常通过虚函数来实现，特别是当你想通过基类指针或引用来调用派生类中重写（Override）的成员函数时。下面是一个简单的例子，展示了多态性和虚函数的使用。

首先，我们定义一个基类`Animal`，它有一个虚函数`speak()`，然后定义两个派生类`Dog`和`Cat`，它们分别重写了`speak()`函数。

```cpp
#include <iostream>  
#include <string>  
  
// 基类  
class Animal {  
public:  
    // 虚析构函数，确保通过基类指针删除派生类对象时能正确调用派生类的析构函数  
    virtual ~Animal() {}  
  
    // 虚函数  
    virtual void speak() const {  
        std::cout << "Some animal sound\n";  
    }  
};  
  
// Dog类，继承自Animal  
class Dog : public Animal {  
public:  
    // 重写speak函数  
    void speak() const override { // 使用override关键字明确表明意图  
        std::cout << "Woof!\n";  
    }  
};  
  
// Cat类，继承自Animal  
class Cat : public Animal {  
public:  
    // 重写speak函数  
    void speak() const override {  
        std::cout << "Meow!\n";  
    }  
};  
  
// 使用多态性的函数  
void makeItSpeak(const Animal& animal) {  
    animal.speak(); // 调用的是Animal类中的speak，还是Dog/Cat中的speak？取决于animal的实际类型  
}  
  
int main() {  
    Dog myDog;  
    Cat myCat;  
  
    // 通过基类引用调用派生类的函数，展示了多态性  
    makeItSpeak(myDog); // 输出: Woof!  
    makeItSpeak(myCat); // 输出: Meow!  
  
    // 注意：下面的代码不会通过编译，因为makeItSpeak期望一个const Animal&类型的参数  
    // 但Animal类型的对象并不具有Dog或Cat特有的speak()实现  
    // Animal myAnimal;  
    // makeItSpeak(myAnimal); // 如果取消注释，将调用Animal的speak()  
  
    return 0;  
}
```

但是，上面的`makeItSpeak`函数实际上并没有完全展示多态性的全部力量，因为它通过值传递（虽然这里是通过引用传递，但传递的是`const`引用，这限制了多态的某些方面）接收`Animal`对象。为了更充分地展示多态，我们通常会通过基类指针来操作派生类对象。

下面是修改后的示例，使用基类指针来展示多态性：

```cpp
#include <iostream>  
#include <memory> // 用于std::unique_ptr  
  
// ...（之前的类定义保持不变）  
  
int main() {  
    std::unique_ptr<Animal> myDog(new Dog());  
    std::unique_ptr<Animal> myCat(new Cat());  
  
    // 通过基类指针调用派生类的函数  
    myDog->speak(); // 输出: Woof!  
    myCat->speak(); // 输出: Meow!  
  
    return 0;  
}
```

在这个修改后的例子中，我们使用了`std::unique_ptr<Animal>`来管理`Dog`和`Cat`对象的生命周期，并通过基类`Animal`的指针来调用这些对象的`speak()`函数。由于`speak()`函数在基类中被定义为虚函数，并且在派生类中被重写，因此通过基类指针调用时，会调用与指针实际指向的对象类型相对应的`speak()`函数，这就是多态性的体现。

#### enum class

---

![image-20240711175515130](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240711175515130.png)

![image-20240711180737239](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240711180737239.png)

`enum class Status:std::uint32_t; *//修改底层型别为uint32_t*`

#### 重载与重载操作符

---

重载

![image-20240712173706574](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240712173706574.png)

重载操作符

1. `double Attr::operator[](int index) const`

```
cpp复制代码double Attr::operator[](int index) const
{
    return __attr[index];
}
```

这个函数重载了下标运算符，使得 `Attr` 类的对象可以像数组一样通过下标访问元素。返回的是私有成员变量 `__attr` 中对应索引的元素。

2. `Attr Attr::operator()(const Attr& attr)`

```
cpp复制代码Attr Attr::operator()(const Attr& attr)
{
    __attr.resize(attr.size());
    for (int i = 0; i < attr.size(); ++i)
        __attr[i] = attr[i];
    return *this;
}
```

这个函数重载了函数调用运算符，使得 `Attr` 类对象可以像函数一样被调用。这个重载函数用于将一个 `Attr` 对象的内容复制到另一个 `Attr` 对象中。

3. `Attr Attr::operator=(const Attr& attr)`

```
cpp复制代码Attr Attr::operator=(const Attr& attr)
{
    return operator()(attr);
}
```

这个函数重载了赋值运算符。它通过调用已经定义的 `operator()` 函数来实现赋值操作。

4. `Attr Attr::operator()(const std::vector<double>& attr_v)`

```
cpp复制代码Attr Attr::operator()(const std::vector<double>& attr_v)
{
    __attr.resize(attr_v.size());
    for (int i = 0; i < attr_v.size(); ++i)
        __attr[i] = attr_v[i];
    return *this;
}
```

这个函数也是重载了函数调用运算符，但它接受一个 `std::vector<double>` 作为参数，并将这个向量的内容复制到 `Attr` 对象中。

调用示例

```cpp
#include <vector>
#include <iostream>

class Attr {
public:
    double operator[](int index) const {
        return __attr[index];
    }

    Attr operator()(const Attr& attr) {
        __attr.resize(attr.size());
        for (int i = 0; i < attr.size(); ++i)
            __attr[i] = attr[i];
        return *this;
    }

    Attr operator=(const Attr& attr) {
        return operator()(attr);
    }

    Attr operator()(const std::vector<double>& attr_v) {
        __attr.resize(attr_v.size());
        for (int i = 0; i < attr_v.size(); ++i)
            __attr[i] = attr_v[i];
        return *this;
    }

    size_t size() const { return __attr.size(); }

private:
    std::vector<double> __attr;
};

int main() {
    // 初始化 Attr 对象
    Attr attr1, attr2;
    std::vector<double> vec = {1.0, 2.0, 3.0};

    // 使用 operator()(const std::vector<double>&) 初始化 attr1
    attr1(vec);

    // 通过下标运算符访问元素
    double value = attr1[1];
    std::cout << "Value at index 1: " << value << std::endl;

    // 使用 operator()(const Attr&) 将 attr1 的内容复制到 attr2
    attr2(attr1);

    // 使用赋值运算符将 attr2 的内容赋值给 attr1
    attr1 = attr2;

    return 0;
}

```

![image-20240711180323084](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240711180323084.png)![image-20240711180536463](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240711180536463.png)



## 算法

### 数据结构

#### 链表

---

##### 递归和迭代

在 C++ 中，链表的迭代（iteration）和递归（recursion）都是常用的方法来遍历链表和执行相关操作，它们各自有着不同的特点和适用场景。迭代通常使用循环语句（如 `for`、`while`）来实现，通过迭代器或指针依次访问链表中的每个节点，执行相应的操作。递归即是函数嵌套。但递归函数的调用会占用额外的栈空间，可能导致栈溢出，且递归深度过大时性能较差

对于链表递归而言，个人现在理解相当于利用压栈弹栈重构一个新的链表。在链表递归中，一般两种情况：一是移除某个节点，二是反转链表。

对于反转链表而言，由于链表头是固定的，可以不返回值或者用其他相同类型返回值接收，可以一直返回该链表头，注意这里head == nullptr || head->next == nullptr二者缺一不可。**如果只有前者，由于后面调用了p=head->next以及p->next空指针是不能访问next节点**，只有后者空链表会报错。此外注意一定先head再head->next否则空链表会野指针发生段错误。如果毫不考虑空链表只需要head->next == nullptr走到最后一个节点。只写head == nullptr会造成newHead空指针。**空指针不能访问节点数据**

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        // if(head == nullptr || head->next == nullptr){
        if(head == nullptr || head->next == nullptr){
            return head;
        }
        ListNode* newHead = reverseList(head->next);
        ListNode* p = head->next;
        p->next = head;
        head->next = nullptr;
        return newHead;//这里递归是因为头节点固定 return newHead

    }
};
```

对于删除链表节点而言，**如果加上head->next == nullptr就可能会返回head指针并且处理不了最后一个元素**，这并不是我们想要看到的，因为不能保证这个值被检验是否会删除。我们想要看到是返回值均为return，因而只加head == nullptr

```cpp
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        if(head == nullptr)
            return head;
        head->next = removeElements(head->next, val); 
        return head->val == val ? head->next : head;
    }
};
```



##### 内存空间管理

链表删除节点释放空间 注意此时要先保存head节点 否则后续删除head已经是赋值后的值了

```cpp
Node* r = nullptr;
while(head != nullptr){
    r = head;
    head = head->next;
    delete r;
}
```

##### 单链表删除节点

由于是单链表，我们不能找到前驱节点，所以我们不能按常规方法将该节点删除。
我们可以换一种思路**，将下一个节点的值复制到当前节点**，然后将下一个节点删除即可。

##### 虚拟头节点

```cpp
class Solution {
public:
    ListNode* deleteDuplication(ListNode* head) {
        auto dummy = new ListNode(-1);
        dummy->next = head;

        auto p = dummy;
        while (p->next) {
            auto q = p->next;
            while (q && p->next->val == q->val) q = q->next;

            if (p->next->next == q) p = p->next;
            else p->next = q;
        }

        return dummy->next;
    }
};
1.设置虚拟头节点 可以删除头节点
2.p指向dummy 只有当p指向dummy同时且改变p->next的时候才会改变
```

#### 栈与队列

```
stack和queue pop操作返回void类型
stack.size()栈中元素
queue.empty() 队列为空返回true 不空返回false
```



#### 树

---

二叉树的前中后序遍历
![image-20240623231835657](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240623231835657.png)

![image-20240623231900578](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240623231900578.png)

![image-20240623231911692](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240623231911692.png)



![image-20240623232008070](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240623232008070.png)

![在这里插入图片描述](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/202012091634524.gif)

![image-20240623232238774](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240623232238774.png)

![在这里插入图片描述](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/20201209164211397.gif)

![image-20240623232153493](C:\Users\30116\AppData\Roaming\Typora\typora-user-images\image-20240623232153493.png)



![在这里插入图片描述](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/2020120916532175.gif)

#### 图

---

邻接表处理稀疏图 邻接矩阵处理稠密图

![image-20240625222421704](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240625222421704.png)

### 哈希表

![image-20240625230009665](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240625230009665.png)

![image-20240625225928678](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240625225928678.png)

```cpp
class Solution {
public:
    bool isAnagram(string s, string t) {
        int record[26] = {0};
        for (int i = 0; i < s.size(); i++) {
            // 并不需要记住字符a的ASCII，只要求出一个相对数值就可以了
            record[s[i] - 'a']++;
        }
        for (int i = 0; i < t.size(); i++) {
            record[t[i] - 'a']--;
        }
        for (int i = 0; i < 26; i++) {
            if (record[i] != 0) {
                // record数组如果有的元素不为零0，说明字符串s和t 一定是谁多了字符或者谁少了字符。
                return false;
            }
        }
        // record数组所有元素都为零0，说明字符串s和t是字母异位词
        return true;
    }
};
```



### 动态规划

Dynamic Programing dp是前一个状态推出来的，而贪心是取局部最优
**对于动态规划问题，我将拆解为如下五步曲，这五步都搞清楚了，才能说把动态规划真的掌握了！**

1. 确定dp数组（dp table）以及下标的含义
2. 确定递推公式
3. dp数组如何初始化
4. 确定遍历顺序
5. 举例推导dp数组

一些同学可能想为什么要先确定递推公式，然后在考虑初始化呢？

**因为一些情况是递推公式决定了dp数组要如何初始化！**



### 贪心算法

**贪心的本质是选择每一阶段的局部最优，从而达到全局最优**

![image-20240626001040538](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240626001040538.png)



![image-20240626001104620](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240626001104620.png)

```cpp
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        sort(g.begin(), g.end());
        sort(s.begin(), s.end());
        int index = s.size() - 1;//饼干数组下标
        int result = 0;
        for(int i = g.size() - 1; i >= 0; i --){ 
            //注意顺序不可以更换 先胃口再饼干 外面固定移动 里面符合条件移动
            if(index >= 0 && s[index] >= g[i]){
                result ++;
                index --;
            }
        }
        return result;
    }
};

//从代码中可以看出我用了一个 index 来控制饼干数组的遍历，遍历饼干并没有再起一个 for 循环，而是采用自减的方式，这也是常用的技巧。

//有的同学看到要遍历两个数组，就想到用两个 for 循环，那样逻辑其实就复杂了。
```



注意版本一的代码中，可以看出来，是先遍历的胃口，在遍历的饼干，那么可不可以 先遍历 饼干，在遍历胃口呢？
其实是不可以的。
外面的 for 是里的下标 i 是固定移动的，而 if 里面的下标 index 是符合条件才移动的。
如果 for 控制的是饼干， if 控制胃口，就是出现如下情况 ：

![image-20240626001221337](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240626001221337.png)

### 回溯算法

![image-20240625234643254](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240625234643254.png)

![image-20240625234715438](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240625234715438.png)

```cpp
class Solution {
private:
    vector<vector<int>> result; // 存放符合条件结果的集合
    vector<int> path; // 用来存放符合条件结果
    void backtracking(int n, int k, int startIndex) {
        if (path.size() == k) {
            result.push_back(path);
            return;
        }
        for (int i = startIndex; i <= n; i++) {
            path.push_back(i); // 处理节点
            backtracking(n, k, i + 1); // 递归
            path.pop_back(); // 回溯，撤销处理的节点
        }
    }
public:
    vector<vector<int>> combine(int n, int k) {
        result.clear(); // 可以不写
        path.clear();   // 可以不写
        backtracking(n, k, 1);
        return result;
    }
};
```

![image-20240625234613342](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240625234613342.png)



## 编译

### Makefile


在 Makefile 中，这些变量和标志用于编译和链接程序。以下是这些变量和标志的解释：

自动变量

1. `$@`
   - 表示当前规则中的目标文件名。在目标文件规则中表示目标文件名，在链接规则中表示生成的可执行文件名。
2. `$<`
   - 表示第一个依赖文件的文件名。通常在编译规则中使用，表示源文件名。
3. `$^`
   - 表示所有的依赖文件名。通常在链接规则中使用，表示所有需要链接的目标文件名。

标志

1. **`-c`**
   - 编译源文件但不链接。生成目标文件（.o 文件）而不是可执行文件。
2. **`--libs`**
   - 这是 `pkg-config` 的一个选项，表示获取指定库的链接标志。例如，`pkg-config --libs opencv4` 会返回 OpenCV 库的链接选项。
3. **`--cflags`**
   - 这是 `pkg-config` 的一个选项，表示获取指定库的编译标志。例如，`pkg-config --cflags opencv4` 会返回 OpenCV 库的编译选项（包含路径等）。

![image-20240620192551697](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240620192551697.png)



![image-20240620192707613](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240620192707613.png)
![image-20240620192636949](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240620192636949.png)

![image-20240620193158003](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240620193158003.png)

![image-20240620193417049](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240620193417049.png)

![image-20240627112218978](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240627112218978.png)

![image-20240627112044939](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240627112044939.png)

![image-20240627112122161](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240627112122161.png)

![image-20240627112159454](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240627112159454.png)



在C++项目中，头文件的引用路径是相对于包含目录 (`include directories`) 来确定的，而不是相对于源文件的目录结构。在你的Makefile中，`CXXFLAGS` 选项中添加了 `-I./inc` 和其他相关目录，这意味着这些目录中的头文件在所有源文件中都是可见的。

所以，当你在 `src/configs/pics_config.cpp` 中写 `#include "pics_config.h"` 时，编译器会从你指定的所有包含目录中搜索这个头文件。只要 `pics_config.h` 位于这些目录之一中，编译器就能找到它。

### 编译流程

```
指定文件夹下解压
unzip -d ./test moudle.zip

四、编译的四个步骤
gcc hello.c -o hello
g++ hello.cpp -o hello


使用gcc把C文件编译成可执行文件可分为四步：预编译、编译、汇编、连接。

1、预编译（生成.i文件）

预编译器cpp把源文件和相关的头文件（如实例代码中的头文件stdio.h）预编译成一个.i的文件。

执行的命令：gcc -E hello.c -o hello.i

预编译的作用：

a、处理所有的“#include”预编译指令

b、处理所有的"#define"指令，将代码中所有的"#define"删除，并展开所有的宏定义

c、处理所有的条件预编译指令，如#if #elif #else #ifdef #ifnodef #endif等

d、删除所有的注释

e、添加行号和文件名标识，以便产生错误时给出提示信息

2、编译（生成.s文件）

编译器gcc把预处理后的文件进行语法分析、语义分析以及优化后生成汇编代码文件。

执行的命令：gcc -S hello.i -o hello.s

3、汇编(生成.o文件)

汇编器把汇编代码文件转换成中间目标文件

执行的命令：gcc -c hello.s -o hello.o  (注意：这里是小写的-c，而不是大写的-C，本人在此处踩坑，出现异常)

4、链接(生成可执行文件)

链接器ld把目标文件与所需要的所有的附加的目标文件(如静态链接库、动态链接库)链接起来成为可执行的文件

执行的命令：gcc hello.o -o hello

```

```sh
g++ -c main.cpp src/*.cpp
gcc -o main *.o
g++ *.o -o main # -o 后面接生成文件outputfile
```

## C++ Primer Plus



## Cherno C++

指针---`static_cast dynamic_cast delete[]`

```cpp
void* 转换为具体类型的指针，通常使用 static_cast，而不是 dynamic_cast。

static_cast 和 dynamic_cast 区别
static_cast:
用于执行编译时类型检查的强制转换。
可以用于将 void* 转换为任何其他指针类型。
适用于不涉及多态的类型转换。
不安全：如果转换类型不正确，结果未定义，但不会在编译时捕捉到错误。
    
dynamic_cast:
用于运行时类型检查的强制转换。
主要用于在继承体系中进行向下转换（如基类指针转换为派生类指针）。
需要对象是多态类型（即至少有一个虚函数）。
安全：如果转换失败，返回 nullptr。
对于将 void* 转换为其他类型的指针，应使用 static_cast，因为 dynamic_cast 不能用于 void* 转换。
    
为什么 dynamic_cast 不能用于 void*
dynamic_cast 主要用于进行带有多态类型的类型转换。在这种情况下，dynamic_cast 需要运行时类型信息（RTTI），这要求被转换的类型必须包含至少一个虚函数。因此，不能将 void* 直接转换为其他类型的指针。
    
#include <iostream>

int main() {
    int value = 42;
    void* ptr = &value; // void* 指针指向 int 类型的 value
    
    // 使用 static_cast 将 void* 转换为 int* dynamic_cast会报错
    int* int_ptr = static_cast<int*>(ptr);
    
    // 现在可以解引用 int* 指针
    std::cout << *int_ptr << std::endl;
    
    return 0;
}
    
#include<iostream>
#define LOG(x) std::cout << x << std::endl;
    
int main(){
	char* buffer = new char[8];
    memset(buffer, 0, 8);
    
    delete[] buffer;//删除数组
    std::cin.get();//暂停
}
```

引用& 并不是指针，编译器不会新开辟内存新建变量，这点不同于指针，可以减少内存开销

```cpp
#include<iostream>
#define LOG(x) std::cout << x << std::endl;
//void inc (int* val){
// 	(*val) ++;
//}
void inc(int& val){ 
	val ++;
}
int main(){
	int a = 2;
    int b = 5;
    inc(a);//传引用不同于传指针，无需特殊符号
    int& ref = a;//引用必须要赋值 他不像指针是一个新的变量
    ref = b;//想让引用ref从a变到b 这种方法是不允许的 这样是把b的值赋值给a 只能通过指针实现
    std::cin.get();//暂停
}
```

类 类中private变量只有类中函数能够访问 类中成员默认是private 结构体默认是public
一般不必过度区分使用：我们可以在表示一堆数据集合使用struct；在涉及到继承或者比较复杂功能使用class

实现类：log日志系统 error warning message 修改不同等级输出不同的等级信息



static：分为两种类和结构体内/外
类外的只对定义它的翻译单元可见
类中所有实例共享这一个static变量在C++中，如果你在头文件（.h）中定义了一个`static`变量，并且这个头文件被两个或多个`.cpp`文件包含（include），那么实际上每个`.cpp`文件都会得到该`static`变量的一个独立副本。这是因为`static`变量具有文件作用域（file scope）和内部链接性（internal linkage），意味着它仅在其定义的文件内部可见和可用。

当你改变其中一个`.cpp`文件中的这个`static`变量时，它只会影响该`.cpp`文件内的那个副本，不会影响到其他`.cpp`文件中该变量的副本。这可能会导致一些意外的行为，尤其是如果你期望在多个文件中共享和修改同一个变量时。

类/结构体中静态方法不能访问非静态变量，原因是静态方法没有类实例。实际上在类中每个非静态的方法总是获得当前类的一个实例作为参数。静态方法与在类外部编写方法相同

```cpp
#include<iostream>
struct Entity{
	static int x, y;
    void Print(){
		std::cout << x << " , " << y << std::endl;
    }
};
int Entity::x;//事实上相当于命名空间 两个类的x，指向同一个内存空间。e1.x就没影意义了
int Entity::y;
int main(){
    Entity e;
    Entity::x = 2;
    Entity::y = 3;
    Entity e1;
    Entity::x = 5;//就像我们在命名空间声明了两个变量，可以是public/private是类的一部分效果等同于命名空间
    Entity::y = 8;
    e.Print();
    e1.Print();
    std::cin,get();
}
```

局部静态变量，考虑两个因素：变量作用域和生存期
局部静态变量自动处理了静态成员变量的定义和初始化
静态成员变量存在内存泄漏风险
static Singleton* instance_ = nullptr;这样会报错
静态成员变量必须在类定义外部进行定义和初始化![image-20240710112903242](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240710112903242.png)![image-20240710113627568](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240710113627568.png)![image-20240710113738154](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240710113738154.png)

enum必须是整数 可以是char/unsigned char不能是float
![image-20240710122156185](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240710122156185.png)

构造函数：用于实例初始化，我们可以删除构造函数，这样对于该类，只能引用它的方法
对于new关键字也是利用类的构造函数

继承与多态相关，基类接口也可以由其子类传入
对于虚函数，我们需要额外的内存来存储V表，这样保证我们能够分配到正确的函数，基类要有一个成员指针，指向V表
其次我们每次调用虚函数的时候，我们都需要遍历这个表，来确定要映射到哪个函数
![image-20240710170821909](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240710170821909.png)

纯虚函数允许我们在基类中定义一个没有实现的函数，强制子类去实现该函数
我们想要强制子类为特定函数提供自己的定义，面向对象编程，创建一个类，只由未实现的方法组成，然后强制子类去实现他们，通常被称为接口。类中的接口是包未实现的方法，作为模板

当有了纯虚函数之后，该类就不能进行实例化 即纯虚函数必须被实现，才能够创建这个类的实例。
使用纯虚函数，我们可以确保类都有一个特定的方法，那么可以将这个抽象基类作为参数（类型）传入一个通用函数

Cpp可见性 public/private/protected
friend友元，可以从类中访问私有成员
private：类内与友元可见，类外不可见，子类也不能访问
protected：继承体系可见，类外不可见
public：可见

## Google Cpp Style



# 计算机网络

## 概念

---

### OSI七层模型

---

![image-20240523154000771](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240523154000771.png)

![image-20240523154046666](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240523154046666.png)

网络通信是进程间通信，网络通信大概分为三个层次
（1）、硬件层：网卡设备，收发网络数据
（2）、驱动层：网卡驱动（Linux 内核网卡驱动代码）内核层
（3）、应用层：上层应用程序（调用 socket 接口或更高级别接口实现网络相关应用程序）

网络互连模型：OSI七层模型 （Open System Interconnection）

![image-20240615120505109](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615120505109.png)
<img src="https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615120535102.png" alt="image-20240615120535102"  />

**应用层**

​	应用层（Application Layer）是 OSI 参考模型中的最高层，是最靠近用户的一层，为上层用户提供应用接口，也为用户直接提供各种网络服务。我们常见应用层的网络服务协议有：`HTTP、FTP、TFTP、SMTP、``SNMP、DNS、TELNET、HTTPS、POP3、DHCP。`

**表示层**

​	表示层（Presentation Layer）提供各种用于应用层数据的编码和转换功能，确保一个系统的应用层发送的数据能被另一个系统的应用层识别。如果必要，该层可提供一种标准表示形式，用于将计算机内部的多种数据格式转换成通信中采用的标准表示形式。数据压缩/解压缩和加密/解密（提供网络的安全性）也是表示层可提供的功能之一。

**会话层**

​	会话层（Session Layer）对应主机进程，指本地主机与远程主机正在进行的会话。会话层就是负责建立、管理和终止表示层实体之间的通信会话。该层的通信由不同设备中的应用程序之间的服务请求和响应组成。将不同实体之间表示层的连接称为会话。因此会话层的任务就是组织和协调两个会话进程之间的通信，并对数据交换进行管理。

**传输层**

​	传输层（Transport Layer）定义传输数据的协议端口号，以及端到端的流控和差错校验。该层建立了主机端到端的连接，传输层的作用是为上层协议提供端到端的可靠和透明的数据传输服务，包括差错校验处理和流控等问题。我们通常说的，`TCP、UDP` 协议就工作在这一层，端口号既是这里的“端”。

**网络层**

​	进行逻辑地址寻址，实现不同网络之间的路径选择。本层通过 IP 寻址来建立两个节点之间的连接，为源端发送的数据包选择合适的路由和交换节点，正确无误地按照地址传送给目的端的运输层。网络层（Network Layer）也就是通常说的 IP 层。该层包含的协议有：`IP（Ipv4、Ipv6）、ICMP、IGMP` 等。

**数据链路层**

​	数据链路层（Data Link Layer）是 OSI 参考模型中的第二层，负责建立和管理节点间逻辑连接、进行硬件地址寻址、差错检测等功能。将比特组合成字节进而组合成帧，用 MAC 地址访问介质，错误发现但不能纠正。
​	数据链路层又分为 2 个子层：逻辑链路控制子层（LLC）和媒体访问控制子层（MAC）。MAC 子层的主要任务是解决共享型网络中多用户对信道竞争的问题，完成网络介质的访问控制；LLC 子层的主要任务是建立和维护网络连接，执行差错校验、流量控制和链路控制。
​	数据链路层的具体工作是接收来自物理层的位流形式的数据，并封装成帧，传送到上一层；同样，也将来自上层的数据帧，拆装为位流形式的数据转发到物理层；并且，还负责处理接收端发回的确认帧的信息，以便提供可靠的数据传输。

**物理层**

​	物理层（Physical Layer）是 OSI 参考模型的最低层，物理层的主要功能是：利用传输介质为数据链路层提供物理连接，实现比特流的透明传输，物理层的作用是实现相邻计算机节点之间比特流的透明传送，尽可能屏蔽掉具体传输介质和物理设备的差异。使数据链路层不必考虑网络的具体传输介质是什么。“透明传送比特流”表示经实际电路传送后的比特流没有发生变化，对传送的比特流来说，这个电路好像是看不见的。
​	实际上，网络数据信号的传输是通过物理层实现的，通过物理介质传输比特流。物理层规定了物理设备标准、电平、传输速率等。常用设备有（各种物理设备）集线器、中继器、调制解调器、网线、双绞线、同轴电缆等，这些都是物理层的传输介质.

![image-20240615121907367](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615121907367.png)

### 数据封装与拆封

---

![image-20240615122103118](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615122103118.png)

​	当用户发送数据时，将数据向下交给传输层，但是在交给传输层之前，应用层相关协议会对用户数据进行封装，譬如 `MQTT、HTTP` 等协议，其实就是在用户数据前添加一个应用程序头部，这是处于应用层的操作，最后应用层通过调用传输层接口来将封装好的数据交给传输层。
​	传输层会在数据前面加上传输层首部（此处以 TCP 协议为例，图中的传输层首部为 TCP 首部，也可以是 UDP 首部），然后向下交给网络层。
​	同样地，网络层会在数据前面加上网络层首部（IP 首部），然后将数据向下交给链路层，链路层会对数据进行最后一次封装，即在数据前面加上链路层首部（此处使用以太网接口为例，对应以太网首部），然后将数据交给网卡。
​	最后，由网卡硬件设备将数据转换成物理链路上的电平信号，数据就这样被发送到了网络中。这就是网络数据的发送过程，从图中可以看到，各层协议均会对数据进行相应的封装，可以概括为 TCP/IP 模型中的各层协议对数据进行封装的过程。
​	以上便是网络数据的封装过程，当数据被目标主机接收到之后，会进行相反的拆封过程，将每一层的首部进行拆解最终得到用户数据。所以，数据的接收过程与发送过程正好相反，可以概括为 TCP/IP 模型中的各层协议对数据进行解析的过程。

### IP地址

---

​	Internet Protocol Address IP 地址是软件地址，不是硬件地址，硬件 MAC 地址是存储在网卡中的，应用于局域网中寻找目标主机。IP地址的编址方式传统的 IP 地址是一个 32 位二进制数的地址，也叫 IPv4 地址，由 4 个 8 位字段组成。除了 IPv4 之外，还有 IPv6，IPv6 采用 128 位地址长度，8 个 16 位字段组成，在网络通信数据包中，IP 地址以 32 位二进制的形式表示；而在人机交互中，通常使用点分十进制方式表示，譬如 192.168.1.1，这就是点分十进制的表示方式。IP 地址中的 32 位实际上包含 2 部分，分别为网络地址和主机地址，可通过子网掩码来确定网络地址和主机地址分别占用多少位。

![image-20240615123206909](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615123206909.png)

![image-20240615124747173](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615124747173.png)

![image-20240615124828461](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615124828461.png)

![image-20240615125201243](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615125201243.png)
![image-20240615125217984](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615125217984.png)

![image-20240615130020540](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615130020540.png)
![image-20240615130030429](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615130030429.png)
![image-20240615130509386](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615130509386.png)

### TCP/IP协议

---

![image-20240615132435860](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615132435860.png)
![image-20240615132449451](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615132449451.png)



### TCP协议

---

#### 协议特性

![image-20240615132929193](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615132929193.png)
![image-20240615133005544](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615133005544.png)
![image-20240615133025600](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615133025600.png)

#### 报文格式

![image-20240615133212845](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615133212845.png)
![image-20240615133235495](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615133235495.png)
![image-20240615133252319](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615133252319.png)

#### 建立TCP连接

##### 三次握手

![image-20240615135102243](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615135102243.png)
![image-20240615135159756](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615135159756.png)

##### 四次挥手

![image-20240615135314892](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615135314892.png)
![image-20240615135807771](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615135807771.png)
![image-20240615135823846](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615135823846.png)

##### 整体流程

![image-20240615135932406](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615135932406.png)

#### TCP状态说明

![image-20240615134122651](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615134122651.png)
![image-20240615134132357](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615134132357.png)

#### UDP协议

![image-20240615133909573](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615133909573.png)
![image-20240615133921068](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615133921068.png)

### 端口号

---

![image-20240615132628686](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615132628686.png)

### 网卡和路由器

**网卡（Network Interface Card，NIC）**

**网卡的作用和功能：**

1. **连接计算机和网络：** 网卡是计算机与网络进行通信的硬件设备。它负责将计算机连接到局域网（LAN）或广域网（WAN）。
2. **数据帧传输：** 网卡负责在物理层和数据链路层之间传输数据帧。它将数据分组转换为电信号（有线网卡）或无线信号（无线网卡），并通过网络介质传输。
3. **数据封装和解封装：** 网卡在发送数据时，将数据封装成帧；在接收数据时，解封装收到的数据帧，提取数据。
4. **MAC地址管理：** 网卡有一个唯一的物理地址，即MAC地址，用于在局域网中标识设备。

**网卡在联网时的作用：**

- **建立连接：** 通过网卡，计算机可以连接到网络交换机、路由器或其他网络设备，从而进入网络。
- **数据传输：** 网卡负责将计算机的数据通过网络发送到目的设备，并接收来自网络的数据信息。

**路由器（Router）**

**路由器的作用和功能：**

1. **连接不同网络：** 路由器用于连接和隔离不同的网络。它在网络层工作，通过路由表决定数据包的转发路径。
2. **数据包转发：** 路由器根据目的IP地址和路由表，将数据包从一个网络转发到另一个网络。
3. **网络地址转换（NAT）：** 路由器可以进行NAT，将多个私有IP地址映射到一个公共IP地址，允许内部网络设备访问外部网络。
4. **防火墙功能：** 路由器通常具备防火墙功能，能够过滤和管理进出网络的数据流量，增强网络安全。

**路由器在联网时的作用：**

- **数据包转发：** 路由器根据路由表决定如何将数据包从源地址转发到目的地址。
- **网络连接管理：** 路由器管理内部网络和外部网络的连接，确保数据流量按照预期的路径传输。
- **访问控制：** 路由器可以配置访问控制列表（ACL），限制或允许特定设备和应用程序的网络访问。

**Ping操作**

**Ping的工作机制：**

1. **发送ICMP回显请求：** 当你执行ping命令时，计算机会向目标设备发送ICMP回显请求（Echo Request）。
2. **接收ICMP回显应答：** 目标设备接收到回显请求后，会发送ICMP回显应答（Echo Reply）回到源设备。

**Ping操作依赖的组件：**

- **网卡：** 在执行ping操作时，网卡负责发送和接收ICMP数据包。它将这些数据包通过网络传输。
- **路由器：** 如果ping目标在不同的网络，路由器将负责转发ICMP数据包，确保数据包到达目标设备并返回。

**总结：**

- **网卡负责设备与网络的物理连接和数据传输**。
- **路由器负责不同网络之间的数据转发和管理**。
- **Ping操作主要依赖网卡发送和接收数据包，**
- 网卡和交换机负责局域网内数据通信；路由器负责不同网络间数据传输；
  同一张网卡，经由不同路由器进行网络通信网速也会有差别

## 湖大计网网课

### 第一章 计网概述

1.2因特网概述

![image-20240701222640155](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240701222640155.png)

![image-20240701222812186](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240701222812186.png)

![image-20240701223222686](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240701223222686.png)

![image-20240701224234788](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240701224234788.png)

![image-20240701224803993](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240701224803993.png)

1.3电路交换/分组交换/报文交换

![image-20240702144231103](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702144231103.png)

![image-20240702144341519](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702144341519.png)

分组交换（现在主要使用 ） 
分组逐个传输，使得后一个分组的存储和前一个分组的转发可以同时进行
发送方：构造分组/发送分组
路由器：缓存分组/转发分组
接收方：接受分组/还原报文

![image-20240702144617905](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702144617905.png)

![image-20240702145029977](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702145029977.png)

![image-20240702145441433](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702145441433.png)

1.4计网定义和分类
![image-20240702145841994](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702145841994.png)

![image-20240702150323121](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702150323121.png)

1.5计网性能指标![image-20240702150635634](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702150635634.png)

![image-20240702151214930](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702151214930.png)

![image-20240702151404371](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702151404371.png)

![image-20240702151720018](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702151720018.png)

![image-20240702151957054](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702151957054.png)

![image-20240702152130863](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702152130863.png)

![image-20240702152240307](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702152240307.png)

![image-20240702152443806](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702152443806.png)

![image-20240702152548137](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702152548137.png)

![image-20240702152726386](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702152726386.png)

![image-20240702152842686](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702152842686.png)

![image-20240702152905702](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702152905702.png)

1.6计网体系结构
1.6.1.常见计网体系结构![image-20240702153508217](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702153508217.png)

![image-20240702153443869](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702153443869.png)

1.6.2.计网体系结构分层和必要性![image-20240702154757422](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702154757422.png)

1.6.3.计网体系结构分层思想举例![image-20240702155026306](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702155026306.png)

1.6.4.计网体系结构中专用术语
实体/协议/服务![image-20240702160101220](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702160101220.png)

![image-20240702160051644](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702160051644.png)

![image-20240702160303421](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702160303421.png)

![image-20240702160929406](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702160929406.png)
![image-20240702161016542](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702161016542.png)

![image-20240702161119885](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702161119885.png)

![image-20240702161216681](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702161216681.png)

### 第二章 物理层

2.1物理层基本概念
![image-20240703102049573](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703102049573.png)

2.2物理层下面的传输媒体
传输媒体不属于计网体系结构中的任何一层
![image-20240703102336026](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703102336026.png)![image-20240703102452290](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703102452290.png)![image-20240703102557853](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703102557853.png)![image-20240703102731571](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703102731571.png)![image-20240703102805976](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703102805976.png)

![image-20240703103137867](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703103137867.png)![image-20240703103119893](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703103119893.png)![image-20240703103255827](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703103255827.png)![image-20240703103325772](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703103325772.png)

![image-20240703103436570](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703103436570.png)

2.3传输方式![image-20240703103827241](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703103827241.png)

2.4编码与调制![image-20240703110215315](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703110215315.png)

 ![image-20240703112647662](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703112647662.png)

![image-20240703112922374](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703112922374.png)

2.5信道的极限容量
调制速度即码元传输速度也即波特率

![image-20240703114340641](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703114340641.png)

![image-20240703114446853](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703114446853.png)

![image-20240703114509986](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703114509986.png)

![image-20240703114902744](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703114902744.png)

![image-20240703115135618](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703115135618.png)

![image-20240703115146873](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240703115146873.png)

### 第三章 数据链路层 

### 第四章 网络层

### 第五章 运输层

### 第六章 应用层

## socket编程

---

### socket_function

---

![image-20240615140328979](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240615140328979.png)



### socket_problem

```shell
lsof -i:8888 #查看8888端口是否被使用
netstat -tnlp #查看服务器进程使用哪些端口

在agx上实验可以运行 最终发现是防火墙
https://blog.csdn.net/HHHSSD/article/details/117410122

首先将需要连接的端口加到安全组中 例如8888
！！！注意这里查看打开端口号才是正确的 
加完成后将该端口拉入防火墙
1.开启防火墙
systemctl start firewalld
2设置打开的端口号（永久打开）
firewall-cmd --add-port=8000/tcp --permanent
3.更新，端口的设置
firewall-cmd --reload
4.查看已经打开的端口
firewall-cmd --list-all

使用 netstat -tulpn 查看 端口使用情况
# 以8888端口为例
netstat -tulpn | grep 8888
```



# 数据库

## MySQL

```cpp
第一个数据库项目cpp_canteen
项目中需要使用mysql.h需要将MySQL安装目录下的include文件夹和limysql.dll和libmysql.lib添加到根目录下
在Visual Studio中添加项目目录引用include文件夹，此外需要加上这句话
#pragma comment(lib, "libmysql.lib")即可正常运行
另外，程序没连上SQL会发生堆栈错误，这里我们可以先下载SQL WorkBench然后在里面新建类别cpp_canteen
再将对应.sql文件导入对应类别即可 最后别忘记在代码中修改连接的数据库名称和密码
```

## SQLite

```sqlite
常用语句
.help //打印可用的命令清单
.exit .quit //退出数据库系统
.databases //列出数据库名字和所依赖文件
.show //显示各种状态当前值
.table //显示数据库所有表格
 PRAGMA table_info([tablename]);   tablename 为实际的数据表名//查看表具体字段
 sqlite> SELECT * FROM COMPANY; //查询表具体信息
```



# 计算机组成原理

## CSAPP

### 第一章

程序运行流程：
首先我们按下./hello ，字符串从键盘被读取 shell会将读取到的字符串逐一加载到寄存器，处理器会把hello字符串放到内存中。（注意此处不可以DMA直接读取到内存，内存只是存储空间，不放在寄存器里面program count无法指向程序）按下回车键，指令结束，shell会通过一系列指令加载可执行文件hello。这些指令会将hello中的数据和指令从磁盘督导内存而不经过CPU，这既是DMA技术。当hello中的数据和指令到达内存时，处理器就开始执行指令。CPU会将hello world\n 这个字符串复制到寄存器文件，然后再从寄存器文件复制到显示设备。

![image-20240701095309869](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240701095309869.png)

![image-20240701095600280](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240701095600280.png)

![image-20240702105444786](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702105444786.png)

![image-20240702111734630](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702111734630.png)

![image-20240702105905118](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702105905118.png)

![image-20240702110710375](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702110710375.png)

![image-20240702111326538](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702111326538.png)

![image-20240702111503634](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702111503634.png)

![image-20240702111802383](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702111802383.png)

![image-20240702111905488](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702111905488.png)

![image-20240702112016418](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702112016418.png)

![image-20240702112105886](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240702112105886.png)

### 第二章

#### 2.1 信息存储

gcc -m32 -o hello32 hello.c #编译32位程序
gcc -m64 -o hello64 hello.c #编译64位程序
if(a && 5/a) //先判断a是否为0 如果是0，则不再执行后续5/a

![image-20240704100006629](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240704100006629.png)![image-20240704100206456](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240704100206456.png)

算术右移与逻辑右移不同之处：操作数最高位为1时，左端补1
一般有符号数算术右移 无符号数逻辑右移

![image-20240704100617873](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240704100617873.png)

#### 2.2 整数的表示

#### 2.3 整数的运算

#### 2.4 浮点数

# 项目

## QT

---

```sh
sudo apt-get install build-essential
sudo apt-get install qt5-default qtcreator
qt找不到serialport头文件
sudo apt-get install libqt5serialport5
sudo apt-get install libqt5serialport5-dev

QT报错 没有threadmanager.h文件
该文件在windows中存在 linux换成threads.h即可
```



## 国网偏振项目

---

### JPEG压缩

项目地址[Donkeyzhenbang/jpeg_compress (github.com)](https://github.com/Donkeyzhenbang/jpeg_compress)

##### turbojpeg压缩

```cpp
// Function to write JPEG file
void mat_write_jpeg(const std::string& filename, const cv::Mat& image, int quality) {
    tjhandle tj_instance = tjInitCompress();
    if (!tj_instance) throw std::runtime_error("Error: tjInitCompress failed");

    unsigned long jpeg_size = 0;
    unsigned char* jpeg_buf = nullptr;
    int width = image.cols;
    int height = image.rows;
    int channels = image.channels();
    int subsamp = (channels == 3) ? TJSAMP_444 : TJSAMP_GRAY;

    int tj_stat = tjCompress2(
        tj_instance,
        image.data,
        width,
        0,  // pitch
        height,
        (channels == 3) ? TJPF_BGR : TJPF_GRAY,
        &jpeg_buf,
        &jpeg_size,
        subsamp,
        quality,//图像压缩质量
        TJFLAG_ACCURATEDCT
    );

    if (tj_stat != 0) {
        tjDestroy(tj_instance);
        throw std::runtime_error("Error: tjCompress2 failed");
    }

    FILE* jpeg_file = fopen(filename.c_str(), "wb");
    if (!jpeg_file) {
        tjFree(jpeg_buf);
        tjDestroy(tj_instance);
        throw std::runtime_error("Error: Unable to open JPEG file for writing");
    }

    fwrite(jpeg_buf, jpeg_size, 1, jpeg_file);
    fclose(jpeg_file);
    tjFree(jpeg_buf);
    tjDestroy(tj_instance);
}
```



##### 文件夹批处理

```cpp
#include<filesystem>
//注意再makefile中添加 
//CXX = g++ -std=c++17
//获取文件名
std::string get_filename(const std::string& filepath) {
    size_t pos = filepath.find_last_of("/\\");
    if (pos == std::string::npos) {
        return filepath;
    } else {
        return filepath.substr(pos + 1);
    }
}

// 获取文件大小
std::uintmax_t get_file_size(const std::string& filepath) {
    return fs::file_size(filepath);
}

void compress_image_folder_ratio(const std::string& input_directory, const std::string& prefix, int quality,const std::string db_name) {
    std::string output_directory = input_directory + "compressed/";
    sqlite3* db;
    

    // Initialize SQLite database
    init_db(db, db_name);

    // 创建输出目录
    if (!fs::exists(output_directory)) {
        fs::create_directory(output_directory);
    }

    // 打开文件以保存压缩比结果
    std::ofstream compress_file(output_directory + "compress_q" + std::to_string(quality) + ".txt");
    if (!compress_file.is_open()) {
        std::cerr << "Failed to open compress.txt for writing." << std::endl;
        return;
    }

    // 扫描目录获取所有 PNG 文件
    std::vector<std::string> input_filenames;
    for (const auto& entry : fs::directory_iterator(input_directory)) {
        if (entry.path().extension() == ".png") {
            input_filenames.push_back(entry.path().string());
        }
    }

    double total_compress_ratio = 0.0;
    int file_count = 0;

    for (const std::string& input_filepath : input_filenames) {
        std::string filename = get_filename(input_filepath);
        std::string output_filename = output_directory + prefix + filename.substr(0, filename.find_last_of('.')) + ".jpg";

        try {
            // 获取原始文件大小
            std::uintmax_t original_size = get_file_size(input_filepath);

            // 压缩并将图像写入JPEG文件
            mat_jpeg_compress(input_filepath, output_filename, quality);

            // 获取压缩后的文件大小
            std::uintmax_t compressed_size = get_file_size(output_filename);

            // 计算压缩比
            double compress_ratio = static_cast<double>(original_size) / compressed_size;
            total_compress_ratio += compress_ratio;
            file_count++;

            // 写入每张图片的压缩比
            compress_file << filename << ": " << compress_ratio << std::endl;
            insert_into_db(db, filename, original_size, compressed_size, compress_ratio, quality);

            std::cout << "Image " << filename << " successfully compressed and saved as " << output_filename << std::endl;
        } catch (const std::runtime_error& e) {
            std::cerr << "Error processing " << filename << ": " << e.what() << std::endl;
            // 继续处理下一个文件
        }
    }

    // 计算并写入平均压缩比
    if (file_count > 0) {
        double average_compress_ratio = total_compress_ratio / file_count;
        compress_file << "Average compress ratio: " << average_compress_ratio << std::endl;
    }

    compress_file.close();
    sqlite3_close(db);
    return;
}

```

##### SQLite

```cpp
#include"pics_config.h"
#include<iostream>
#include<cstring>
#include<sqlite3.h>

void init_db(sqlite3* &db, const std::string& db_name) {
    int rc = sqlite3_open(db_name.c_str(), &db);
    if (rc) {
        std::cerr << "Can't open database: " << sqlite3_errmsg(db) << std::endl;
        exit(1);
    } else {
        std::cout << "Opened database successfully\n";
    }

    std::string sql = "CREATE TABLE IF NOT EXISTS COMPRESS_INFO("
                      "ID INTEGER PRIMARY KEY AUTOINCREMENT,"
                      "FILENAME TEXT NOT NULL,"
                      "JPG_QUALITY INTEGER NOT NULL,"
                      "ORIGINAL_SIZE INTEGER NOT NULL,"
                      "COMPRESSED_SIZE INTEGER NOT NULL,"
                      "COMPRESSION_RATIO REAL NOT NULL);";

    char* errmsg;
    rc = sqlite3_exec(db, sql.c_str(), 0, 0, &errmsg);
    if (rc != SQLITE_OK) {
        std::cerr << "SQL error: " << errmsg << std::endl;
        sqlite3_free(errmsg);
        exit(1);
    } else {
        std::cout << "Table created successfully\n";
    }
}

void insert_into_db(sqlite3* db, const std::string& filename, int original_size, int compressed_size, double compression_ratio, int quality) {
    std::string sql = "INSERT INTO COMPRESS_INFO (FILENAME, JPG_QUALITY, ORIGINAL_SIZE, COMPRESSED_SIZE, COMPRESSION_RATIO) VALUES (?, ?, ?, ?, ?);";
    sqlite3_stmt* stmt;
    sqlite3_prepare_v2(db, sql.c_str(), -1, &stmt, 0);
    sqlite3_bind_text(stmt, 1, filename.c_str(), -1, SQLITE_STATIC);
    sqlite3_bind_int(stmt, 2, quality);
    sqlite3_bind_int(stmt, 3, original_size);
    sqlite3_bind_int(stmt, 4, compressed_size);
    sqlite3_bind_double(stmt, 5, compression_ratio);

    if (sqlite3_step(stmt) != SQLITE_DONE) {
        std::cerr << "SQL error: " << sqlite3_errmsg(db) << std::endl;
    }

    sqlite3_finalize(stmt);
}
```

### 服务器传图

定义编译宏 make -j8 TYPE=LIB即为编译生成动态库  ![image-20240708134813558](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240708134813558.png)



## CMU15445-fall2022

#### 项目配置

参考文档 在对应project里面会有对应lab要求 在FAQ中有邀请码 fall2022年邀请码为PXWVR5
https://15445.courses.cs.cmu.edu/fall2022/assignments.html

gradescope网址 用于提交zip进行lab测试
[15-445/645 (Non-CMU) Dashboard | Gradescope](https://www.gradescope.com/courses/425272)

```shell
执行格式检查
安装clang-fromat,clang-tidy

sudo apt-get install clang-format
下方语句需管理员权限（输入su和密码）

apt-get install clang-tidy
然后在build文件夹下依次执行以下命令

make format
make check-lint
make check-clang-tidy-p0

打包 make submit-p0适用于2023年和2022年除p0外
zip project0-submission.zip \
	src/include/primer/p0_trie.h
```

#### git使用

```shell
首先克隆仓库
git clone --bare https://github.com/cmu-db/bustub.git bustub-public #https
git clone git@github.com:cmu-db/bustub.git #ssh

其次将代码push到自己的代码库
$ cd bustub-public

# If you pull / push over HTTPS
$ git push https://github.com/student/bustub-private.git master

# If you pull / push over SSH
$ git push git@github.com:student/bustub-private.git master

随后删除官方仓库
$ cd ..
$ rm -rf bustub-public

clone自己的私有仓库到运行机器
# If you pull / push over HTTPS
$ git clone https://github.com/student/bustub-private.git

# If you pull / push over SSH
$ git clone git@github.com:student/bustub-private.git

从官方仓库拉取更改
$ git pull public master #当时并未使用

接下来根据所选的版本切换分支 当时我做的是fall2022 去commit里找对应时间的分支进行切换
$ git checkout {对应分支号}
此时注意由于切换了commit 指针属于游离状态 需要新建分支才能进行push

编译
$ mkdir build
$ cd build
$ cmake ..
$ make
```

```shell
Windows
Cloning into bare repository 'bustub-public'...
fatal: unable to access 'https://github.com/cmu-db/bustub.git/': Failed to connect to github.com port 443 after 21135 ms: Timed out

解决方案git config --global http.sslVerify "false"
```

![image-20240523175709038](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240523175709038.png)

![image-20240523175733014](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240523175733014.png)

```shell
Ubuntu
fatal: 无法访问 'https://github.com/cmu-db/bustub.git/'：Could not resolve host: github.com
```



#### gdb调试

```shell
gdb main进入调试
run 程序运行
break LinkedList<int>::remove 插入断点
condition 1 item_to_remove==1 断点执行条件
step #下一步会执行相应跳转
next #下一步不会执行相应跳转
x 0xffbef014 #查看对应地址的值

start表示程序开始
list可以查看相应代码
Ctrl+x+a可以像现代编译器一样查看代码 Text UI
layout：用于分割窗口，可以一边查看代码，一边测试。主要有以下几种用法：
layout src：显示源代码窗口
layout asm：显示汇编窗口
layout regs：显示源代码/汇编和寄存器窗口
layout split：显示源代码和汇编窗口
layout next：显示下一个layout
layout prev：显示上一个layout
Ctrl + L：刷新窗口
Ctrl + x，再按1：单窗口模式，显示一个窗口
Ctrl + x，再按2：双窗口模式，显示两个窗口
Ctrl + x，再按a：回到传统模式，即退出layout，回到执行layout之前的调试窗口。
当layout src的时候上下箭头是滑动程序界面 此时使用Ctrl+P Ctrl+N实现上下一条命令

b main #插入断点
delete 1#删除断点
info break #查看断点
b 9 #第九行插入断点
此外gdb内置python解释器
python print(gdb.breakpoints())
python print(gdb.breakpoints()[0].location)
python gdb.Breakpoints('7')

生成并调试core文件
core dumped核心已转储
ls -lth core*
ulimit -a #查看core文件大小
ulimit -c unlimited #修改core文件限制大小
cat /proc/sys/kernel/core_pattern #查看core生成路径
因为ubuntu的服务apport.service。自动生成崩溃报告
直接用echo "/home/boy/corefile/core-%e-%p-%t"> /proc/sys/kernel/core_pattern 进行修改
因为我们修改的core_pattern文件是只读文件，没法这样修改。所以要换一种思路，修改不了就先停掉apport.service，这个服务对我们来说基本没用
//1.启用错误报告
sudo systemctl enable apport.service
//或
sudo service apport start
 
//2.关闭错误报告
sudo systemctl disable apport.service
//或
sudo service apport stop
使用sudo service apport stop停掉错误报告即可在可执行文件同目录生成
gdb -c core 查看错误报告 输入bt查看错误报告
```

#### Clion远程调试

#### Makefile

注意Makefile默认编译第一个文件产生可执行文件 需要加上生成所有可执行文件

```makefile
CXX = g++
FLAGS = -ggdb -Wall
EXECUTABLES = main linkedlist
all: $(EXECUTABLES)

linkedlist: linkedlist.cc 
		${CXX} ${FLAGS} -o linkedlist linkedlist.cc
main: main.cc 
		${CXX} ${FLAGS} -o main main.cc

clean:
	rm -f ${EXECUTABLES}
```



#### 内存管理

```cpp
int* pointer = new int(5);
delete pointer;
pointer = nullptr;
```

这行代码的作用是释放由 `pointer` 指针指向的内存。具体来说，它会：

- 调用指针指向对象的析构函数（如果有）。
- 释放对象占用的内存，使之可供其他用途。
- 将该指针标记为无效，指针指向的内存区域变得不可再使用。

在调用 `delete` 后，`pointer` 指针仍然存在，但它指向的内存已经被释放，因此再次使用它会导致未定义行为。

- 清除指针的值，使其不再指向任何有效的内存地址。
- 避免野指针（dangling pointer）问题。野指针指向的内存已经被释放，但指针本身没有被更新，仍然指向原来的内存地址。将指针设置为 `nullptr` 后，可以显式地表示该指针不再指向有效的对象。

```shell
For this project, we use LLVM Address Sanitizer (ASAN) and Leak Sanitizer (LSAN) to check for memory errors. To enable ASAN and LSAN, configure CMake in debug mode and run tests as you normally would. If there is memory error, you will see a memory error report.

valgrind \
    --error-exitcode=1 \
    --leak-check=full \
    ./test/starter_trie_test
```

![image-20240524222652490](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240524222652490.png)





## Webserver

#### Cpolar

可以将本地的服务器通过安全隧道公开至公网
同时服务器上可以安装xdg-utils firefox可以再命令行打开网页

```sh
cpolar 1316 #将本地运行1316端口公开至公网
```



#### Linux编程入门

![image-20240621151032826](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240621151032826.png)

![image-20240621222459720](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240621222459720.png)

![image-20240621224819986](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240621224819986.png)

```shell
#静态库制作
gcc -c add.c div.c sub.c mult.c #生成对应.o文件 gcc –o mylib.o –c mylib.c
ar rcs libcalc.a add.o div.o sub.o mult.o
#动态库制作
gcc –c -fpic add.c div.c sub.c mult.c
gcc -shared add.o div.o sub.o mult.o -o libcalc.so #生成对应.o文件
用户配置 .bashrc
系统配置 /etc/profile source /etc/profile
LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/home/veroll/Linux/lesson1.6/library/lib
$echo LD_LIBRARY_PATH #查看动态库路径

ls /etc/ld.so.cache
sudo vim /etc/ld.so.conf 
在该文件中直接添加目录即可/home/path/lib
```

![image-20240621234956382](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240621234956382.png)

![image-20240621235034068](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240621235034068.png)



## Google Test
