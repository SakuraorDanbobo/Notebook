#### int 转 string

```c++
string itos(int i) // 将int 转换成string{
    stringstream s;
    s << i;
    return s.str();
}
```



## 字符串

 strcmp：比较两个字符串a和b。(类似于Java 的equal，区别于==)

 strlen：获取一个字符串a的长度。

## map



## 优先队列(priority queue)



## set

**特点**：

1. set中的元素都是排序好的（默认升序）
2. set中的元素都是唯一的，没有重复的（去重）
3. 不能直接改变元素值，因为那样会打乱原本正确的顺序，要改变元素值必须先删除旧元素，则插入新元素

**begin和 end的区别**：

**c.begin()** ;      返回指向**容器最开始位置数据的指针** 

**c.end()**;       返回指向**容器最后一个数据单元+1的指针**，即**容器大小**

***注意*** ：若要输出最后一个元素的值，应该为 ***( --c.end() )** 或者 **c.rbegin()** ;

​			判空间只需要c.begin()==c.end();

```c++
set<int> s;
//输出最小数
cout<<*s.begin()<<endl;
//输出最大数(2种方法)
cout<<*(--s.end())<<endl;
cout<<*s.rbegin()<<endl; 
```

**set的成员函数列表** 

> begin()--返回指向第一个元素的迭代器
>
> clear()--清除所有元素
>
> count()--返回某个值元素的个数
>
> empty()--如果集合为空，返回true
>
> end()--返回指向最后一个元素的迭代器
>
> equal_range()--返回集合中与给定值相等的上下限的两个迭代器
>
> erase()--删除集合中的元素
>
> find()--返回一个指向被查找到元素的迭代器
>
> get_allocator()--返回集合的分配器
>
> insert()--在集合中插入元素
>
> lower_bound()--返回指向大于（或等于）某值的第一个元素的迭代器
>
> key_comp()--返回一个用于元素间值比较的函数
>
> max_size()--返回集合能容纳的元素的最大限值
>
> rbegin()--返回指向集合中最后一个元素的反向迭代器
>
> rend()--返回指向集合中第一个元素的反向迭代器
>
> size()--集合中元素的数目
>
> swap()--交换两个集合变量
>
> upper_bound()--返回大于某个值元素的迭代器
>
> value_comp()--返回一个用于比较元素间的值的函数

```c++
#include <bits/stdc++.h>
using namespace std;
int main()
{
//	1.创建时需要指定元素类型
	set<int> s;
//	2.元素的插入
	s.insert(3);
	s.insert(2);
	s.insert(5);
    s.insert(6);
	s.insert(3);//*重复元素不会插入
//  3.元素的删除
    s.erase(6);
//  4.元素的查找
//  使用find()方法对集合进行检索，如果找到查找的的键值，则返回该键值的迭代器位置；
//  否则，返回集合最后一个元素后面的一个位置，即end()。
　　set<int>::iterator index;
    index=s.find(3); //查找键值为3
    if(index!=s.end())
    {
        cout<<*index;
    }
//	5.元素的遍历(正向和反向)
	set<int>::iterator it; //定义迭代器（默认正向） 
	for(it=s.begin();it!=s.end();it++)
	{
		cout<<*it<<" ";
	}
	cout<<endl;
    
	set<int>::reverse_iterator rit; //定义反向迭代器 
	for(rit=s.rbegin();rit!=s.rend();rit++)
	{
		cout<<*rit<<" ";
	}
	cout<<endl;
	
    return 0;
}

```



## vector ##

## list ##

 定义：list将元素按顺序储存在链表中 ， 它允许快速的插入和删除，但是随机访问却比较慢。

```
//常用方法
back() 返回最后一个元素 
clear() 删除所有元素 
empty() 如果list是空的则返回true 
erase() 删除一个元素 
front() 返回第一个元素 
insert() 插入一个元素到list中 
pop_back() 删除最后一个元素 
pop_front() 删除第一个元素 
push_back() 在list的末尾添加一个元素 
push_front() 在list的头部添加一个元素 
rbegin() 返回指向第一个元素的逆向迭代器 
remove() 从list删除元素 
rend() 指向list末尾的逆向迭代器 
reverse() 把list的元素倒转 
size() 返回list中的元素个数 
sort() 给list排序 
splice() 合并两个list 
swap() 交换两个list 
unique() 删除list中重复的元素
```

list的增、删、改、查、反转、打印操作代码如下：

```c++
//借助迭代器来对list顺序链表进行操作 
//list任意位置插入值，下标从1开始
void Insert(list<int> &t,int pos,int x)
{
	 list<int>::iterator it=t.begin();
	 //advance() 函数用于将迭代器前进（或者后退）指定长度的距离
	 advance(it,pos-1);
	 t.insert(it,x);
}
//list任意位置删除值，下标从1开始
void Delete(list<int> &t,int pos)
{
	list<int>::iterator it=t.begin();
	advance(it,pos-1);
	t.erase(it);
}
//list中查找值，若查找不到返回-1，下标从1开始
int Find(list<int> &t,int x)
{
	//algorithm中的find()函数
	//返回值是目标元素的下标，找不到时返回值为迭代器结尾
	list<int>::iterator it=find(t.begin(),t.end(),x);
	if(it!=t.end())
	{
		//distance(p, q)：计算两个迭代器之间的距离，即迭代器 p 经过多少次 + +
		//操作后和迭代器 q 相等。如果调用时 p 已经指向 q 的后面，则这个函数会陷入死循环。
		//返回下标从0开始 
		return distance(t.begin(),it)+1;
	 } 
	 else return -1;
}
//list中更新值，下标从1开始
void Update(list<int> &t,int pos,int x)
{
	list<int>::iterator it=t.begin();
	advance(it,pos-1);
	*it=x;
}
//反转list中的元素
void Reverse(list<int> &t)
{
	t.reverse();
}
//通过迭代器去遍历输出
void PrintLinkList(list<int> &iList)
{
    list<int>::iterator it;
    int flag = 0;
    for(it=iList.begin();it!=iList.end();it++)
    {
        if(flag)
            cout<<" ";
        flag = 1;
        cout<<*it;
    }
    cout<<endl;
}
```
