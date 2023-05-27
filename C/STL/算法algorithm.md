## 算法 Algorithm

以下是 STL 算法模块中全部的函数:

1. 序列排序函数:sort
2. 排序变体:stable_sort/partial_sort/partial_sort_copy/nth_element
3. 交换:swap
4. 复制:copy/copy_if/copy_n/iter_swap
5. 求最小最大值:min/max
6. 求和:accumulate
7. 查找:find/find_if/find_if_not/adjacent_find
   8: 查找变体:count/count_if/search/search_n
8. 二分查找算法:binary_search/lower_bound/upper_bound
9. 修改:transform/replace/replace_if/fill/fill_n
10. 删除:remove/remove_if/unique
11. 排列组合:next_permutation/prev_permutation
12. 圆周上分步进:rotate
13. 交换范围:swap_ranges
14. 合并:merge/inplace_merge
15. 布尔操作:all_of/any_of/none_of
16. 遍历:for_each
17. STL 针对集合的算法:
    set_union/set_intersection/set_difference/set_symmetric_difference

以上是 STL 算法模块中全部的函数,用法与 STL 容器一样模板化。

<br>

### 头文件

```c
#include<algorithm>
#include<functional>
```

<br>

### 排序 sort

sort() 是 STL 中一个非常重要和常用的算法,它可以对任何 STL 容器中的元素进行排序。

使用 sort()进行排序需要包含头文件`#include <algorithm>`

sort()有以下几个重要特征:

- 默认按升序排列元素
- 可以指定自定义比较函数,实现降序或其他顺序
- 时间复杂度为 O(nlogn)
- 算法操作会改变原有容器中的元素顺序

sort()函数的基本用法:

```cpp
sort(iterator beg, iterator end);
sort(iterator beg, iterator end, comp);
```

- beg 和 end 指向序列的起始和结束位置
- comp 为比较函数(或者函数对象),它决定排序的顺序。若不指定,则按默认升序排列。

例子:

```cpp
vector<int> v = {5, 3, 1, 2, 4};
sort(v.begin(), v.end());
// v 是 {1, 2, 3, 4, 5}

sort(v.begin(), v.end(), greater<int>());
// v 是 {5, 4, 3, 2, 1}
```

<br>

### 排序变体函数

> 变体 sort 和原版 sort 使用的方式完全一致，只是拥有了额外的特性而已

以下的 sort 变体函数和原版 sort 的主要区别在于:

- stable_sort():完全排序且稳定
- partial_sort():部分排序
- partial_sort_copy():复制并且部分排序
- nth_element():仅将第 n 小元素置于正确位置

<br>

### 交换 swap

swap() 可以交换两个元素的值。

使用 swap() 需要包含头文件:`#include <algorithm>`

swap()的基本形式为:

```cpp
swap(a, b);
```

它会交换 a 和 b 两个变量的值。a 和 b 可以是任意类型的变量。

例如:

```cpp
int a = 10;
int b = 20;
swap(a, b);
// a 是 20,b 是 10

string s1 = "hello";
string s2 = "world";
swap(s1, s2);
// s1 是 "world",s2 是 "hello"
```

swap()也可以用于交换两个迭代器指向的元素:

```cpp
iter_swap(a, b);
```

例如:

```cpp
vector<int> v = {1, 2, 3};

swap(v[0], v[2]); // 交换元素 1 和 3

iter_swap(v.begin(), v.begin()+2); // 也可使用迭代器交换
```

> 需要注意的是,swap()函数仅交换两个变量的值,并不复制他们。

<br>

### 复制 copy

copy() 用于将一个序列复制到另一个序列。

copy() 的基本形式是:

```cpp
copy(input_iterator begin, input_iterator end,
     output_iterator dest);
```

- begin 和 end 指定源序列的迭代器范围
- dest 指向目标序列的位置,结果被复制到 dest 之后

例如:

```cpp
vector<int> src {1, 2, 3};
vector<int> dest(src.size());

copy(src.begin(), src.end(), dest.begin());
// dest 为 {1, 2, 3}
```

copy() 的优点是:

- 可以复制任意类型的序列
- 效率高,避免了创建临时对象

copy() 常用于:

- 从一个序列复制到另一个序列
- 复制序列的部分元素
- 将数组复制到容器中

copy()还有几个变体函数:

- copy_if(): 复制谓词值为 true 的元素
- copy_n(): 复制 n 个元素
- move():移动而不是复制

<br>

### 最值

很简单的两个函数，min 和 max，他们均接收两个数值，并在执行后返回较小或者较大的那个值

min() 和 max() 的基本形式分别是:

```cpp
min(a, b);
max(a, b);
```

经典例子：

```cpp
int a = 5;
int b = 10;

cout << min(a, b) << endl; // 输出 5
cout << max(a, b) << endl;  // 输出 10

string s1 = "abc";
string s2 = "def";

cout << max(s1, s2) << endl; // 输出 def
cout << min(s1, s2) << endl; // 输出 abc
```

它们也可以作用于两个迭代器,得到迭代器指向区间最小和最大值:

```cpp
vector<int> v = {1, 2, 3, 4, 5};

cout << *min_element(v.begin(), v.end()) << endl; // 输出 1
cout << *max_element(v.begin(), v.end()) << endl; // 输出 5
```

<br>

### 求和 accumulate

accumulate() 的基本形式为:

```cpp
accumulate(iterator beg, iterator end, init_value);
```

- beg 和 end 指向容器的迭代器范围
- init_value 表示被求和的数值的类型，如果你要求和整数，此处必须填一个整数（0、1、2 随意）；如果你要求和浮点数，同理此处填任意一个浮点数

```cpp
vector<int> v = {1, 2, 3};
int sum = accumulate(v.begin(), v.end(), 0);
cout << sum ;   // 输出 6

// 也可以不指定 init_value
int sum = accumulate(v.begin(), v.end());
cout << sum;  // 也输出 6
```

accumulate() 还有两个变体:

- accumulate( beg, end, init_value, binary_op) ,指定累加操作
- partial_sum( beg, end, dest) ,将部分和存储到 dest 中

其优点是:

- 可以作用于任何类型的序列
- 非常高效,时间复杂度 O(n)

<br>

### 查找 find

find() 和它的变体函数都是 STL 中的查找算法,用来在序列中查找元素。

## find()

find() 的基本形式为:

```cpp
find(iterator beg, iterator end, value);
```

- beg 和 end 指定序列的迭代器范围
- value 是要查找的目标值

如果找到,返回该元素的迭代器;否则返回 end 迭代器

时间复杂度是 O(n)

例子:

```cpp
vector<int> v = {1, 3, 5, 2, 4};
int value = 3;

auto it = find(v.begin(), v.end(), value);
if (it != v.end()) {
    cout << "Element found at position " << it - v.begin();
} else {
    cout << "Element not found";
}
// 输出 Element found at position 1
```

find() 有几个常用的变体函数:

- find_if(): 使用谓词 predicate 来查找元素
- find_if_not(): 使用谓词查找非 predicate 为 true 的元素
- search():在一个序列中查找一个子序列
- search_n(): 查找 n 个连续的与给定值相等的元素

这些变体函数也是 O(n)的时间复杂度。

例如:

```cpp
find_if(v.begin(), v.end(), [](int val){ return val % 2 == 0; });
// 查找第一个偶数元素

find_if_not(v.begin(), v.end(), isodd);
// 查找第一个奇数元素
```

<br>

### 二分搜索 binary_search

binary_search() 是 STL 中的一个二分查找算法,只能作用于有序序列。

它的基本形式为:

```cpp
binary_search (iterator beg, iterator end, value);
```

- beg 和 end 指定序列迭代器范围
- value 是要查找的值

binary_search() 的作用是:

- 利用二分查找算法,高效地查找序列中是否含有值等于 value 的元素
- 如果找到返回 true,否则返回 false

时间复杂度是 O(logn)。

比如:

```cpp
vector<int> v = {1, 2, 3, 4, 5};
sort(v.begin(), v.end());

if (binary_search(v.begin(), v.end(), 3)) {
    cout << "Found" << endl;
} else {
    cout << "Not found" << endl;
}
// 输出 Found
```

binary_search() 还有两个变体:

- lower_bound(): 返回第一个不 smaller than value 的位置
- upper_bound(): 返回第一个 greater than value 的位置

这两个变体函数也是时间复杂度 O(logn)。

<br>

### 替换 replace

transform() 可以将一个序列中的元素映射到另一个序列,修改各个元素。

基本形式为:

```cpp
transform(first1, last1, first2, result);
transform(first1, last1, first2, result, binary_op);
```

- first1、last1 是原始序列的迭代器。
- first2 是目标序列的迭代器。
- result 指向结果序列。
- binary_op 是的二元操作函数。

例如:

```cpp
vector<int> v1 = {1, 2, 3};
vector<int> v2(v1.size());

transform(v1.begin(), v1.end(), v2.begin(), [](int x) {
   return x * x;
});
// v2 是 {1, 4, 9}
```

replace 基本形式:

```cpp
replace(first, last, old_value, new_value);
replace_if(first, last, pred, new_value);
```

- first、last 是序列迭代器。
- old_value 是要替换的值。
- new_value 是新值。
- pred 是谓词函数。

例如:

```cpp
replace(v1.begin(), v1.end(), 2, 0);
// 将所有 2 替换为 0

replace_if(v1.begin(), v1.end(), [](int x){ return x % 2 == 0; }, 0);
// 将所有偶数替换为 0
```

fill() 可以用一个指定值填充序列。

基本形式:

```cpp
fill(first, last, value);
fill_n(first, count, value);
```

例如:

```cpp
fill(v1.begin(), v1.end(), 0);
// 用 0 填充序列

fill_n(v2.begin(), 2, 5);
// 用 5 填充 v2 的前两个元素
```

<br>

### 移除 remove

remove() 将序列中满足某个条件的元素移除。

基本形式:

```cpp
remove(iterator beg, iterator end, value);
remove_if(iterator beg, iterator end, pred);
```

- beg 和 end 指定序列迭代器范围
- value 是要移除的值
- pred 是移除元素的谓词

例如:

```cpp
vector<int> v = {1, 2, 3, 2, 4};

// 移除所有值为2的元素
v.erase(remove(v.begin(), v.end(), 2), v.end());
// {1, 3, 4}

// 移除所有奇数元素
v.erase(remove_if(v.begin(), v.end(), [](int n){ return n % 2;}), v.end());
// {2, 4}
```

remove() 会保留原来的元素顺序。

<br>

unique() 将序列中相邻且重复的元素移除,保留一个。

基本形式:

```cpp
unique(iterator beg, iterator end);
unique(iterator beg, iterator end, pred);
```

- beg 和 end 指定序列迭代器范围
- pred 是判断两个元素是否相同的谓词

例如:

```cpp
vector<int> v = {1, 1, 2, 2, 3};

unique(v.begin(), v.end());
// {1, 2, 3}

unique(v.begin(), v.end(), [] (int x, int y){return abs(x - y) < 2;});
// {1, 2, 3}
```

<br>
