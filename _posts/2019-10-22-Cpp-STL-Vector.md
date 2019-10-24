---
layout: article
keys: 20191023
tags: C++ STL Vector
title: C++ STL Vector
---

#   Vector

## c++ 语言对应库：vector.h

## 方法 - 构造、析构、赋值

### 构造函数

可以从多方面去构造`vector`。

⚠️如果指定来vector的大小，如果没有扩张或者缩减过，那么`vector.size() == vector.capacity()`

-- case：

```cpp
// constructing vectors
#include <iostream>
#include <vector>

int main ()
{
  // constructors used in the same order as described above:
  std::vector<int> first;                                // empty vector of ints
  std::vector<int> second (4,100);                       // four ints with value 100
  std::vector<int> third (second.begin(),second.end());  // iterating through second
  std::vector<int> fourth (third);                       // a copy of third

  // the iterator constructor can also be used to construct from arrays:
  int myints[] = {16,2,77,29};
  std::vector<int> fifth (myints, myints + sizeof(myints) / sizeof(int) );

  std::cout << "The contents of fifth are:";
  for (std::vector<int>::iterator it = fifth.begin(); it != fifth.end(); ++it)
    std::cout << ' ' << *it;
  std::cout << '\n';

  return 0;
}
```

输出：

```
The contents of fifth are: 16 2 77 29
```

注释：

第15行中传入的参数有两个，第一个是**首地址**，第二个是**尾地址**。这两个地址中间的内存存放的数据应该是连续的。

⚠️注意：通过地址来构造的时候，包含关系是***前闭后开***。`[首地址, 尾地址)`



### 析构函数

可以不用关注。



### operator=

重载赋值运算符。将`vector a`赋值给另外一个`vector b`。

⚠️两个`vector`必须类型一致，分配方式一致。可以将长vector赋值给短vector，也可以将短vector赋值给长vector，同时其`vector.size()`会变得一致。

-- case：

```cpp
// vector assignment
#include <iostream>
#include <vector>

int main ()
{
  std::vector<int> foo (3,0);
  std::vector<int> bar (5,0);

  bar = foo;
  foo = std::vector<int>();

  std::cout << "Size of foo: " << int(foo.size()) << '\n';
  std::cout << "Size of bar: " << int(bar.size()) << '\n';
  return 0;
}
```

输出：

```
Size of foo: 0
Size of bar: 3
```



## 方法 - Capacity

### vector.size()

返回vector中有效元素所占用的长度。

-- case：

```cpp
// vector::size
#include <iostream>
#include <vector>

int main ()
{
  std::vector<int> myints;
  std::cout << "0. size: " << myints.size() << '\n';

  for (int i=0; i<10; i++) myints.push_back(i);
  std::cout << "1. size: " << myints.size() << '\n';

  myints.insert (myints.end(),10,100); // 在vector结尾处插入10个int元素100
  std::cout << "2. size: " << myints.size() << '\n';

  myints.pop_back();
  std::cout << "3. size: " << myints.size() << '\n';

  return 0;
}
```

输出：

```
0. size: 0
1. size: 10
2. size: 20
3. size: 19
```



### vector.max_size()

vector可以存放的最大的元素个数，一般取决于系统。

猜测的大小计算方式：`2^系统位数 / sizeof(T)`，无论是多少一般不用关注。

-- case：

```cpp
// comparing size, capacity and max_size
#include <iostream>
#include <vector>

int main ()
{
  std::vector<int> myvector;

  // set some content in the vector:
  for (int i=0; i<100; i++) myvector.push_back(i);

  std::cout << "size: " << myvector.size() << "\n";
  std::cout << "capacity: " << myvector.capacity() << "\n"; // 一般是1、2、4、8、16、32、64...
  std::cout << "max_size: " << myvector.max_size() << "\n";
  return 0;
}
```

输出：

```
size: 100
capacity: 128
max_size: 4611686018427387903
```



### vector.resize()

重新调整vector大小。

用法注释：`vector.resize(T, value)`，两个参数，有以下两种场景：

     		1. `T >= vector.size()`，则扩充`T - vector.size()`个元素，并以`value`填充，如果没有`value`，则以0或者初始化值填充。
     		2. `T < vector.size()`，则截取vector的前`T`个元素，其中`value`不起作用。

-- case：

```cpp
// resizing vector
#include <iostream>
#include <vector>

int main ()
{
  std::vector<int> myvector;

  // set some initial content:
  for (int i=1;i<10;i++) myvector.push_back(i);

  myvector.resize(5);
  myvector.resize(8,100);
  myvector.resize(12);

  std::cout << "myvector contains:";
  for (int i=0;i<myvector.size();i++)
    std::cout << ' ' << myvector[i];
  std::cout << '\n';

  return 0;
}
```

输出：

```
myvector contains: 1 2 3 4 5 100 100 100 0 0 0 0
```

注释：

line 10：vector中存放来10个元素，`vector.size() = 10`。

line 12：因为`5 < vector.size()`，故将vector的元素截取前面5个。

line 13：因为`8 > vector.size()`，故将vector的元素个数扩充`8 - 5`个，并以`100`填充。

line 14：因为`12 > vector.size()`，故将vector的元素个数扩充`12 - 8`个，由于没有指定以什么元素进行填充，故以0填充。



### vector.capacity()

vector所占用的内存，一般都是1、2、4、8、16、32、64、128...这种成倍增加的内存大小。

有一个关系：`vector.size() <= vector.capacity() <= vector.max_size()`



### vector.empty()

如果`vector.size()==0`，则返回true。否则为false。

-- case：

```cpp
// vector::empty
#include <iostream>
#include <vector>

int main ()
{
  std::vector<int> myvector;
  int sum (0);

  for (int i=1;i<=10;i++) myvector.push_back(i);

  while (!myvector.empty())
  {
     sum += myvector.back();
     myvector.pop_back();
  }

  std::cout << "total: " << sum << '\n';

  return 0;
}
```

输出：

```
total: 55
```



### vector.reverse()

指定vector分配内存为多少。

-- case：

```cpp
// vector::reserve
#include <iostream>
#include <vector>

int main ()
{
  std::vector<int>::size_type sz;

  std::vector<int> foo;
  sz = foo.capacity();
  std::cout << "making foo grow:\n";
  for (int i=0; i<100; ++i) {
    foo.push_back(i);
    if (sz!=foo.capacity()) {
      sz = foo.capacity();
      std::cout << "capacity changed: " << sz << '\n';
    }
  }

  std::vector<int> bar;
  sz = bar.capacity();
  bar.reserve(100);   // this is the only difference with foo above
  std::cout << "making bar grow:\n";
  for (int i=0; i<400; ++i) {
    bar.push_back(i);
    if (sz!=bar.capacity()) {
      sz = bar.capacity();
      std::cout << "capacity changed: " << sz << '\n';
    }
  }
  return 0;
}
```

输出：

```
making foo grow:
capacity changed: 1
capacity changed: 2
capacity changed: 4
capacity changed: 8
capacity changed: 16
capacity changed: 32
capacity changed: 64
capacity changed: 128
making bar grow:
capacity changed: 100
capacity changed: 200
capacity changed: 400
```



### vector.shrink_to_fit()

将capacity缩减至size。

-- case：

```cpp
// vector::shrink_to_fit
#include <iostream>
#include <vector>

int main ()
{
  std::vector<int> myvector (100);
  std::cout << "1. capacity of myvector: " << myvector.capacity() << '\n';

  myvector.resize(10);
  std::cout << "2. capacity of myvector: " << myvector.capacity() << '\n';

  myvector.shrink_to_fit();
  std::cout << "3. capacity of myvector: " << myvector.capacity() << '\n';

  return 0;
}
```

输出：

```
1. capacity of myvector: 100
2. capacity of myvector: 100
3. capacity of myvector: 10
```



## 方法 - Access

### operator[]

指定位置访问元素。

⚠️不检查边界，反而会继续运行。

-- case：

```cpp
// vector::operator[]
#include <iostream>
#include <vector>

int main ()
{
  std::vector<int> myvector (10);   // 10 zero-initialized elements

  std::vector<int>::size_type sz = myvector.size();

  // assign some values:
  for (unsigned i=0; i<sz; i++) myvector[i]=i;

  // reverse vector using operator[]:
  for (unsigned i=0; i<sz/2; i++)
  {
    int temp;
    temp = myvector[sz-1-i];
    myvector[sz-1-i]=myvector[i];
    myvector[i]=temp;
  }

  std::cout << "myvector contains:";
  for (unsigned i=0; i<sz; i++)
    std::cout << ' ' << myvector[i];
  std::cout << '\n';
    
  std::cout << "exception?" << myvector[10] << std::endl; // 越界测试，不会抛出异常

  return 0;
}
```

输出：

```
myvector contains: 9 8 7 6 5 4 3 2 1 0
exception?3146976
```



### vector.at()

访问特定位置元素。

⚠️检查边界。如果越界，抛出异常：`out_of_range`

-- case：

```cpp
// vector::at
#include <iostream>
#include <vector>

int main ()
{
  std::vector<int> myvector (10);   // 10 zero-initialized ints

  // assign some values:
  for (unsigned i=0; i<myvector.size(); i++)
    myvector.at(i)=i;

  std::cout << "myvector contains:";
  for (unsigned i=0; i<myvector.size(); i++)
    std::cout << ' ' << myvector.at(i);
  std::cout << '\n';

  return 0;
}
```

输出：

```
myvector contains: 0 1 2 3 4 5 6 7 8 9
```



### verctor.front()、vector.back()

访问vector的第一个元素和最后一个有效元素。

⚠️如果使用此中连个方法访问空vector，则会造成未定义的行为，异常。

-- case：

```cpp
// vector::front
#include <iostream>
#include <vector>

int main ()
{
  std::vector<int> myvector;

  myvector.push_back(78);
  myvector.push_back(16);

  // now front equals 78, and back 16

  myvector.front() -= myvector.back();

  std::cout << "myvector.front() is now " << myvector.front() << '\n';

  return 0;
}
```

输出：

```
myvector.front() is now 62
```





### vector.data()

直接获取vector的指针。同`array.data()`方法一致。

-- case：

```cpp
// vector::data
#include <iostream>
#include <vector>

int main ()
{
  std::vector<int> myvector (5);

  int* p = myvector.data();

  *p = 10;
  ++p;
  *p = 20;
  p[2] = 100; // ⚠️当前指针p的位置向后offset sizeof(int)

  std::cout << "myvector contains:";
  for (unsigned i=0; i<myvector.size(); ++i)
    std::cout << ' ' << myvector[i];
  std::cout << '\n';

  return 0;
}
```

输出：

```
myvector contains: 10 20 0 100 0
```



## 方法 - Modifiers

### vector.assign()

修改元素并赋值。

⚠️已有元素会被覆盖。如果用size比原有的`vector.size()`长的覆盖vector，则覆盖后的`vector.size() = vector.capacity() = size`

-- case：

```cpp
// vector assign
#include <iostream>
#include <vector>

int main ()
{
  std::vector<int> first;
  std::vector<int> second;
  std::vector<int> third;

  first.assign (7,100);             // 7 ints with a value of 100

  std::vector<int>::iterator it;
  it=first.begin()+1;

  second.assign (it,first.end()-1); // the 5 central values of first

  int myints[] = {1776,7,4};
  third.assign (myints,myints+3);   // assigning from array.

  std::cout << "Size of first: " << int (first.size()) << '\n';
  std::cout << "Size of second: " << int (second.size()) << '\n';
  std::cout << "Size of third: " << int (third.size()) << '\n';
  return 0;
}
```

输出：

```
Size of first: 7
Size of second: 5
Size of third: 3
```



### vector.push_back()、vector.pop_back()

插入到最后。

***删除***最后，无法获取删除的元素。-- 如果需要获取可以使用`vector.back()`



### vector.insert()

在某个iterator的前面添加几个元素。

-- case：

```cpp
// inserting into a vector
#include <iostream>
#include <vector>

int main ()
{
  std::vector<int> myvector (3,100);
  std::vector<int>::iterator it;

  it = myvector.begin();
  it = myvector.insert ( it , 200 ); // 注意it重新赋值了！！！！！ 在it前插入元素200
  std::cout << "1 myvector contains:";
  for (std::vector<int>::iterator i=myvector.begin(); i<myvector.end(); i++)
    std::cout << ' ' << *i;
  std::cout << '\n';

  myvector.insert (it,2,300); // 在it前面插入2个元素300
  std::cout << "2 myvector contains:";
  for (std::vector<int>::iterator i=myvector.begin(); i<myvector.end(); i++)
    std::cout << ' ' << *i;
  std::cout << '\n';

  // "it" no longer valid, get a new one:
  it = myvector.begin();

  std::vector<int> anothervector (2,400);
  myvector.insert (it+2,anothervector.begin(),anothervector.end()); // 在it+2前面插入元素，元素范围是一个vector的iterator表示，需要有begin()和end()
  std::cout << "3 myvector contains:";
  for (std::vector<int>::iterator i=myvector.begin(); i<myvector.end(); i++)
    std::cout << ' ' << *i;
  std::cout << '\n';

  int myarray [] = { 501,502,503 };
  myvector.insert (myvector.begin(), myarray, myarray+3); // 在vector.begin()前插入元素，元素范围是一个数组的指针，需要有两个指针

  std::cout << "4 myvector contains:";
  for (it=myvector.begin(); it<myvector.end(); it++)
    std::cout << ' ' << *it;
  std::cout << '\n';

  return 0;
}
```

输出：（加*仅表示此次插入的元素，并不代表输出）

```
1 myvector contains: 200* 100 100 100
2 myvector contains: 300* 300* 200 100 100 100
3 myvector contains: 300 300 400* 400* 200 100 100 100
4 myvector contains: 501* 502* 503* 300 300 400 400 200 100 100 100
```



### vector.erase()

抹去指定位置或者指定区间元素。

-- case：

```cpp
// erasing from vector
#include <iostream>
#include <vector>

int main ()
{
  std::vector<int> myvector;

  // set some values (from 1 to 10)
  for (int i=1; i<=10; i++) myvector.push_back(i);
  std::cout << "1 myvector contains:";
  for (unsigned i=0; i<myvector.size(); ++i)
    std::cout << ' ' << myvector[i];
  std::cout << '\n';

  // erase the 6th element
  myvector.erase (myvector.begin()+5);
  std::cout << "2 myvector contains:";
  for (unsigned i=0; i<myvector.size(); ++i)
    std::cout << ' ' << myvector[i];
  std::cout << '\n';

  // erase the first 3 elements:
  myvector.erase (myvector.begin(),myvector.begin()+3);
  std::cout << "3 myvector contains:";
  for (unsigned i=0; i<myvector.size(); ++i)
    std::cout << ' ' << myvector[i];
  std::cout << '\n';

  return 0;
}
```

输出：（加*代表下次删除的元素，并非正式输出）

```
1 myvector contains: 1 2 3 4 5 6* 7 8 9 10
2 myvector contains: 1* 2* 3* 4 5 7 8 9 10
3 myvector contains: 4 5 7 8 9 10
```



### vector.swap()

交换，以vector为入参。要求：T和分配实例一致。

猜测：只是交换指针。已经分配的iterator还是指向原有的。

-- case：

```cpp
// swap vectors
#include <iostream>
#include <vector>

int main ()
{
  std::vector<int> foo (3,100);   // three ints with a value of 100
  std::vector<int> bar (5,200);   // five ints with a value of 200

  foo.swap(bar);

  std::cout << "foo contains:";
  for (unsigned i=0; i<foo.size(); i++)
    std::cout << ' ' << foo[i];
  std::cout << '\n' << foo.capacity() << std::endl;

  std::cout << "bar contains:";
  for (unsigned i=0; i<bar.size(); i++)
    std::cout << ' ' << bar[i];
  std::cout << '\n' << bar.capacity() << std::endl;

  return 0;
}
```

输出：

```
foo contains: 200 200 200 200 200
5
bar contains: 100 100 100
3
```



### vector.clear()

清空vector，使`vector.size()`为0，但是不修改`vector.capacity()`

-- case：

```cpp
// clearing vectors
#include <iostream>
#include <vector>

int main ()
{
  std::vector<int> myvector;
  myvector.push_back (100);
  myvector.push_back (200);
  myvector.push_back (300);

  std::cout << "myvector contains:";
  for (unsigned i=0; i<myvector.size(); i++)
    std::cout << ' ' << myvector[i];
  std::cout << '\n';

  myvector.clear(); // 等同于 std::vector<int>().swap(myvector)
  myvector.push_back (1101);
  myvector.push_back (2202);

  std::cout << "myvector contains:";
  for (unsigned i=0; i<myvector.size(); i++)
    std::cout << ' ' << myvector[i];
  std::cout << '\n';

  return 0;
}
```

输出：

```
myvector contains: 100 200 300
myvector contains: 1101 2202
```

注意⚠️：

虽然line 17可以用替代形式，但是后面其实使用空vector来置空vector，但是size和capacity发生了变化。

### 