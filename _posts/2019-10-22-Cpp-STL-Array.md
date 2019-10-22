---
layout: article
keys: 20191023
tags: C++ STL Array
title: C++ STL Array
---

#   数组 array

## c 语言

### 对应库：utarray.h

## c++ 语言

### 对应库：array.h

### 方法

#### array::at(size_t)

返回值是一个引用，具体可以参考源码.

如果越界，直接报异常：`out_of_range`

##### Case:

```cpp
// array::at
#include <iostream>
#include <array>

int main ()
{
  std::array<int,10> myarray;

  // assign some values:
  for (int i=0; i<10; i++) myarray.at(i) = i+1;

  // print content:
  std::cout << "myarray contains:";
  for (int i=0; i<10; i++)
    std::cout << ' ' << myarray.at(i);
  std::cout << '\n';

  return 0;
}
```

输出：

```
myarray contains: 1 2 3 4 5 6 7 8 9 10
```

注：**如果不用`at`的方法去访问，也可以直接用`[]`去访问和修改元素的值。**



#### array::front()、array::back()

返回数组最后的元素。如果数组是空，则会异常`undefined behavior`

##### Case:

```cpp
// array::back
#include <iostream>
#include <array>

int main ()
{
  std::array<int,3> myarray = {5, 19, 77};

  std::cout << "front is: " << myarray.front() << std::endl;   // 5
  std::cout << "back is: " << myarray.back() << std::endl;     // 77

  myarray.front() = 100;
  myarray.back() = 50;

  std::cout << "myarray now contains:";
  for ( int& x : myarray ) std::cout << ' ' << x;
  std::cout << '\n';

  return 0;
}
```

输出：

```cpp
front is: 5
back is: 77
myarray now contains: 100 19 50
```



#### array.fill(val)

无论数组中是否有值，都以`val`去填充整个数组。

##### Case:

```cpp
// array::fill example
#include <iostream>
#include <array>

int main () {
  std::array<int,6> myarray;

  myarray.fill(5);

  std::cout << "myarray contains:";
  for ( int& x : myarray) { std::cout << ' ' << x; }

  std::cout << '\n';

  return 0;
}
```

输出：

```
myarray contains: 5 5 5 5 5 5
```



#### array.swap()

交换两个数组，要求：元素类型一致，大小一致。如果不一致，报错。

##### Case:

```cpp
// swap arrays
#include <iostream>
#include <array>

int main ()
{
  std::array<int,5> first = {10, 20, 30, 40, 50};
  std::array<int,5> second = {11, 22, 33, 44, 55};

  first.swap (second);

  std::cout << "first:";
  for (int& x : first) std::cout << ' ' << x;
  std::cout << '\n';

  std::cout << "second:";
  for (int& x : second) std::cout << ' ' << x;
  std::cout << '\n';

  return 0;
}
```

输出：

```
first: 11 22 33 44 55
second: 10 20 30 40 50
```





#### array.size()、array.max_size()、sizeof(array)

对于`array`来说，`array.size() == array.max_size()`，返回的是数组所能够容纳的元素个数。

`sizeof(array)`是数组所有元素所占用的内存空间，字节表示。	

##### Case:

```cpp
// array::size
// array::max_size
// sizeof(array)
#include <iostream>
#include <array>

int main ()
{
  std::array<int,3> myarray = {5, 19, 77};

  //myarray.fill(0);

  std::cout << "myarray now contains:";
  for ( int& x : myarray ) std::cout << ' ' << x;
  std::cout << '\n';

  std::cout << "size: " << myarray.size() << std::endl;
  std::cout << "max_size: "<< myarray.max_size() << std::endl;
  std::cout << "sizeof: " << sizeof(myarray) << std::endl;

  return 0;       
}
```

输出：

```
myarray now contains: 5 19 77
size: 3
max_size: 3
sizeof: 24
```



#### array.empty()

如果数组的长度为0则返回true，否则false。

##### Case:

```cpp
// array::empty
#include <iostream>
#include <array>

int main ()
{
  std::array<int,0> first;
  std::array<int,5> second;
  std::cout << "first " << (first.empty() ? "is empty" : "is not empty") << '\n';
  std::cout << "second " << (second.empty() ? "is empty" : "is not empty") << '\n';
  return 0;
}
```

输出：

```
first is empty
second is not empty
```



#### operator[]

返回数组中的元素

##### Case:

```cpp
// array::operator[]
#include <iostream>
#include <array>

int main ()
{
  std::array<int,10> myarray;
  unsigned int i;

  // assign some values:
  for (i=0; i<10; i++) myarray[i]=i;

  // print content
  std::cout << "myarray contains:";
  for (i=0; i<10; i++)
    std::cout << ' ' << myarray[i];
  std::cout << '\n';

  return 0;
}
```

输出：

```
myarray contains: 0 1 2 3 4 5 6 7 8 9
```



#### array.data()

返回数组的指针。

##### Case:

```cpp
// array::data
#include <iostream>
#include <cstring>
#include <array>

int main ()
{
  const char* cstr = "Test string";
  std::array<char,12> charray;

  std::memcpy (charray.data(),cstr,12);

  std::cout << charray.data() << '\n';

  return 0;
}
```

输出：（如果cstr比较长，则只能拷贝到数组的size）

```
Test string
```





#### array.begin()、array.end()

迭代器。可以对元素进行访问、修改。

##### Case:

```cpp
// array::begin example
#include <iostream>
#include <array>

int main ()
{
  std::array<int,5> myarray = { 2, 16, 77, 34, 50 };

  std::cout << "myarray contains:";
  for ( auto it = myarray.begin(); it != myarray.end(); ++it )
    std::cout << ' ' << *it;
  std::cout << '\n';

  return 0;
}
```

输出：

```
myarray contains: 2 16 77 34 50
```



#### array.cbegin()、array.cend()

迭代器。只能对元素进行访问，**不能修改**。

##### Case:

```cpp
// array::cbegin example
#include <iostream>
#include <array>

int main ()
{
  std::array<int,5> myarray = { 2, 16, 77, 34, 50 };

  std::cout << "myarray contains:";

  for ( auto it = myarray.cbegin(); it != myarray.cend(); ++it )
    std::cout << ' ' << *it;   // cannot modify *it

  std::cout << '\n';

  return 0;
}
```

输出：

```
myarray contains: 2 16 77 34 50
```



#### array.rbegin()、array.rend()

反向迭代器。可以访问和修改。

#### array.crbegin()、array.crend()

反向静态迭代器。只能访问，不能对元素进行修改。





##### std::get<size_t>(array)

获取数组中第size_t（0开始计数）的元素，可以访问和修改。

##### Case:

```cpp
// arrays as tuples
#include <iostream>
#include <array>
#include <tuple>

int main ()
{
  std::array<int,3> myarray = {10, 20, 30};
  std::tuple<int,int,int> mytuple (10, 20, 30);

  std::tuple_element<0,decltype(myarray)>::type myelement;  // int myelement

  myelement = std::get<2>(myarray);
  std::get<2>(myarray) = std::get<0>(myarray);
  std::get<0>(myarray) = myelement;

  std::cout << "first element in myarray: " << std::get<0>(myarray) << "\n";
  std::cout << "first element in mytuple: " << std::get<0>(mytuple) << "\n";

  return 0;
}
```

还有其他的，暂且可以不用关注。