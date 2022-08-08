1. Variadic Templates可变参数模版

```C++
#include <iostream>
void printX(){}
template <typename T, typename... Types>
void printX(const T& firstArg, const Types&... args)
{
  std::cout << firstArg << std::endl;
  printX(args...);
}
int main()
{
  printX("ad", "fafd", "fadf");
}
```

...就是所谓的包

```c++
#include <iostream>

template<typename... Values> class tuple;
template<>class tuple<>{};

template <typename Head, typename... Tail>
class tuple<Head, Tail...>:private tuple<Tail...>
{
    typedef tuple<Tail...> inherited;
    public:
        tuple(){}
        tuple(Head v,Tail... vtail):m_head(v), inherited(vtail...){}
        Head head() {return m_head;}
        inherited& tail(){return *this;};
    protected:
        Head m_head;
};

int main()
{
    tuple<int, float> a(12, 21.3);
    return 0;
}
```

2. Spaces in Template Expression 

```c++
vector<list<int> >
vector<list<int>>   //C++11不再使用空格
```
3. nullptr and std::nullptr_t
```c++
void f(int)
void f(void*)
f(0)           //call f(int)
f(NULL)        //call f(int)
f(nullptr)     //call f(void*)

typedef decltype(nullptr) nullptr_t //std::nullptr_t defined in <cstddef>
```

4. auto 实参推导

```c++
list<string> c;
list<string>::iterator iter
iter = find(c.begin(), c.end(), target)

//auto 写法
list<string> c;
auto iter = find(c.begin(), c.end(), target)


#if __cplusplus >= 201103L
	inline auto
	operator-(c...)
#else
	inline typename ...
	operator-(...)
```

5. uniform Initialization

```c++
int values[] {1,2,3};
vector<int> v {2,3,5,7};
vector<string> cities {
	"aaa", "bbb"
};
complex<double> c {4.0, 3.0};
//原理：编译器看到{t1, t2...tn}便作出一个initializer_list<T>，它关联至一个array<T, n>

//以前的赋值
int ia[6] = {1,2,3,4,5,6};
```

6. Initializer Lists

```c++
void print(std::initializer_list<int> vals)
{

}
print({1, 2, 3});

class P
{
  public:
  P(int a, int b)
  {
  }
  P(initializer_list<int> initlist)
  {
  }
}

P p(77, 5);  //P(int, int)
P p{77, 5};  //P(initializer_list<int>)
P p{1,2,3};  //P(initializer_list<int>)
P s={1,2};   //P(initializer_list<int>)
```

7. $explict$ for ctors taking more than one arguments
禁止自动调用构造函数

8. range-based for statement
```c++
for(decl: coll){
    statementt
}


for (auto _pos = coll.begin(), _end=coll.end();_pos != _end; ++_pos)
{
	decl = *_pos
	statement;
}

```

9. =default 默认, =delete 去掉

如果你自行定义了一个ctor，那么编译器就不会再给你一个default ctor
如果你强制加上 =default，就可以重新获得并使用default ctor。
default用于big-five之外无意义
delete可用于任何函数身上，=0只能用于virtual函数

10. alias Template  "using"
```c++
template <typename T>
using Vec = std::vector<T, MyAlloc<T>>

Vec<int> coll

//使用macro和typedef无法达到相同效果。
```
11. template template parameter

```c++
template <typename T, template <class> class Container>

class XCls
{
private:
	Contaoner<T> c;
	
}

//直接使用报错：
XCls<String, Vector> cl;
//规避错误
template <typename T>
using Vec = Vector<T, allocator<T>>;

XCls<string, Vec> cl;
```

12. Type Alias, noexcept, override, final

