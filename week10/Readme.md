# Week 10

## 1

贼(`Thief`)偷窃行人，每个行人都有随身带的钱，贼偷窃一个行人，他的钱就增加相应的钱包里的钱数。请给出`Thief`类和`Walker`类。

## 2

警察局有多名警察，每个警察抓获一名贼，警察局的声望就增加 1 点，该警察的奖金就增加 100 元，贼的金钱减为 0。请实现相关的警察局、警察、贼的类。

用例可为：警察局`S`有警察`p1`,`p2`,`p3`,贼有`t1`,`t2`,`t3`,`t4`, `p1`抓获`t2`,`t3`,`p2`抓获`t4`, `p3`没抓获任何贼。
`t1`的初始金钱为`500`，`t2`的初始金钱为`800`，`t3`的初始金钱为`300`，`t4`的初始金钱为`1000`，`S`的初始声望为`100`，警察的初始奖金为`0`。
输出最终`S`的声望，每个警察的奖金数。

## 3

在[2](#2)的基础上，再加上[1](#1)中的类，试一试。

## 4

用简单双向关联和关联类的形式分别实现男人(`Man`)和女人(`Woman`)间的一对一关系。

> 一个未婚男人可以和一个未婚女人结婚
> 一个已婚男人可以其妻子离婚
> 一个未婚女人可以和一个未婚男人结婚
> 一个已婚女人可以其丈夫离婚
> 一个已婚男人可以“知道”其妻子
> 一个已婚女人可以“知道”其丈夫

## 5

现有类 A 和类 B:

```cpp
class A {
public:
  A(int num):mData(num){}
  ~A( ) {}
  int GetData( ) const    { return mData; }
  void SetData(int data)  { mData = data; }
private:
  int mData;
};
```

```cpp
class B {
public:
  B(int num=0):pa(new A(num)) {  }
  ~B( ) {delete pa;}
  B(const B& rhs) {
    pa=new A(*rhs.pa);
  }
  B& operator=(const B& rhs)  {
    if ( this!=&rhs )  {
       delete pa;
       pa=new A(*rhs.pa);
    }
    return *this;
  }
  A* operator->( ) const {return pa;}
  void GetData() const   { return pa->GetData();}
  void SetData(int data) { pa->SetData(data); }
private:
   A* pa;
};
```

1. 现需要以引用计数的方法，重新实现类 B，要求类`A`不得做任何修改。
2. 请在`1`的基础上，以`Copy On Write`的方式修改类`B`的实现，使得`B`类对象可以访问成员 B 类的成员`SetData(int);`
   (即可以修改`B`类对象中`pa`指针指向的`A`类对象的数据成员。也就是说，使用`B`类对象时，对于以只读方式访问`A`类的成员，使用引用计数；对于以写方式访问`A`类的成员，要先进行深赋值，然后再写数据。)

## 6

若程序中需要频繁地用`new`创建、用`delete`销毁 A 类对象，请在`A`类中通过重载`operator new`和`operator delete`，实时统计创建`A`类对象的个数、销毁的个数、累计分配的字节数、还在使用的字节数。
若还可能使用`new[]`,`delete[]`,如何也能完成上边的统计工作。

## 7

通过分析二元运算符的交换律，以及左操作数的限制，理解

> 为什么重载 `+`，一般用自由函数形式？
> 为什么重载 `+=`，一般用成员函数形式？
> 为什么重载 `=`，必须用成员函数形式？
> 为什么重载 `<<`，必须用自由函数形式？

## 8

实现课堂上讲解的分页器类(`Paginate`),不用实现输入指定页的部分。
如：
对于如下主函数：

```cpp
int main()
{
  Paginate pager(1,13);
  for(int i=1;i<=13;++i) {
    //i当前页，13总页数
    pager.setPage(i,13).show();
  }
  cout<<"start move...."<<endl;
  pager.setPage(5,13).show();
  pager.next().show();
  pager.prev().show();

  //直接翻5页
  pager.nextN().show();
  pager.next().show();
  pager.prevN().show();
  return 0;
}
```

其输出为：(+表示是当前页)  
上页 1+ 2 3 4 5 … 13 下页  
上页 1 2+ 3 4 5 … 13 下页  
上页 1 2 3+ 4 5 … 13 下页  
上页 1 2 3 4+ 5 … 13 下页  
上页 1 2 3 4 5+ … 13 下页  
上页 1 … 6+ 7 8 9 10 … 13 下页  
上页 1 … 6 7+ 8 9 10 … 13 下页  
上页 1 … 6 7 8+ 9 10 … 13 下页  
上页 1 … 6 7 8 9+ 10 … 13 下页  
上页 1 … 6 7 8 9 10+ … 13 下页  
上页 1 … 9 10 11+ 12 13 下页  
上页 1 … 9 10 11 12+ 13 下页  
上页 1 … 9 10 11 12 13+ 下页  
start move....  
上页 1 2 3 4 5+ … 13 下页  
上页 1 … 6+ 7 8 9 10 … 13 下页  
上页 1 2 3 4 5+ … 13 下页  
上页 1 … 6 7 8 9 10+ … 13 下页  
上页 1 … 9 10 11+ 12 13 下页  
上页 1 … 6+ 7 8 9 10 … 13 下页
