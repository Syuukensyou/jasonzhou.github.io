---
layout: article
keys: 20191023
tags: C++ STL Vector
title: C++ STL Vector
---

#   Vector

## c++ 语言对应库：vector.h

## 方法

### 构造函数

可以从多方面去构造`vector`。

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

  		1. `T >= vector.size()`，则扩充`T - vector.size()`个原素，并以`value`填充，如果没有`value`，则以0或者初始化值填充。
         		1. `T < vector.size()`，则截取vector的前`T`个元素，其中`value`不起作用。

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



###vector.reverse()

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





### operator[]

指定位置访问元素。