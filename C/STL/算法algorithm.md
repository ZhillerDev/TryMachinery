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

### 排列组合函数

STL 算法模块中的排列组合函数包括 next_permutation()、prev_permutation()和 next_combination()。

简要解释：每次执行一次排列组合函数，就会为指定迭代器区间的数组或向量进行一次排列

<br>

#### next_permutation()

next_permutation()函数用于生成下一个排列。它接受两个迭代器，分别指向要生成排列的起始和终止位置。函数会将指定范围内的元素重新排列，生成它们的下一个排列（按字典序排列）。如果当前排列已经是最后一个排列，则函数返回 false，否则返回 true。下面是一个使用 next_permutation()函数的例子：

```c++
#include <iostream>
#include <algorithm>
#include <vector>

int main() {
    // 初始化一个向量
    std::vector<int> nums = {1, 2, 3};
    // 生成全排列
    do {
        // 输出当前排列
        for (int num : nums) {
            std::cout << num << " ";
        }
        std::cout << std::endl;
    } while (std::next_permutation(nums.begin(), nums.end()));
    return 0;
}

// 输出结果
1 2 3
1 3 2
2 1 3
2 3 1
3 1 2
3 2 1
```

<br>

#### prev_permutation()

prev_permutation()函数与 next_permutation()函数相反，它用于生成上一个排列。下面是一个使用 prev_permutation()函数的例子：

```c++
#include <iostream>
#include <algorithm>
#include <vector>

int main() {
    // 初始化一个向量
    std::vector<int> nums = {3, 2, 1};
    // 生成全排列
    do {
        // 输出当前排列
        for (int num : nums) {
            std::cout << num << " ";
        }
        std::cout << std::endl;
    } while (std::prev_permutation(nums.begin(), nums.end()));
    return 0;
}

// 输出结果
3 2 1
3 1 2
2 3 1
2 1 3
1 3 2
1 2 3

```

<br>

#### next_combination()

next_combination()函数用于生成下一个组合。它接受两个迭代器，分别指向要生成组合的起始和终止位置，以及一个用于指示组合长度的整数 n。函数会将指定范围内的元素组合成长度为 n 的组合，并生成下一个组合。如果当前组合已经是最后一个组合，则函数返回 false，否则返回 true。下面是一个使用 next_combination()函数的例子：

```c++
#include <iostream>
#include <algorithm>
#include <vector>

int main() {
    // 初始化一个向量
    std::vector<int> nums = {1, 2, 3, 4};
    // 指定组合长度
    int n = 2;
    // 初始化组合
    std::vector<int> combination(n);
    std::iota(combination.begin(), combination.end(), 0);
    // 生成所有组合
    do {
        // 输出当前组合
        for (int index : combination) {
            std::cout << nums[index] << " ";
        }
        std::cout << std::endl;
    } while (std::next_combination(nums.begin(), nums.begin() + n, nums.end()));
    return 0;
}

// 输出结果
1 2
1 3
1 4
2 3
2 4
3 4
```

<br>

### 左右旋翻转 rotate

STL 中的 rotate 函数用于将指定范围内的元素左旋或右旋。  
它接受三个参数：旋转范围的起始位置、旋转范围的结束位置和旋转位置。  
旋转位置是一个迭代器，指向旋转后的第一个元素。

rotate 函数的作用是将指定范围内的元素旋转到容器的另一端，可以实现循环移位、字符串旋转等操作。这个函数非常高效，时间复杂度为 O(N)，其中 N 是旋转范围的大小。

下面是一个使用 rotate 函数的例子，演示如何将一个向量中的元素左旋两个位置：

```c++
#include <iostream>
#include <algorithm>
#include <vector>

int main() {
    std::vector<int> nums = {1, 2, 3, 4, 5};
    // 将前两个元素旋转到向量的末尾
    std::rotate(nums.begin(), nums.begin() + 2, nums.end());
    // 输出旋转后的向量
    for (int num : nums) {
        std::cout << num <<" ";
    }
    std::cout << std::endl;
    return 0;
}

// 运行结果
3 4 5 1 2
```

<br>

**番外：rotate 函数的底层实现原理**

STL 中的 rotate 函数底层实现原理比较巧妙，它采用了三次翻转的方法来实现元素旋转。

具体来说，假设要将向量 nums 中的前 k 个元素旋转到向量末尾，rotate 函数的实现原理如下：

1. 将前 k 个元素翻转，得到子序列 A。

2. 将剩余的 n-k 个元素翻转，得到子序列 B。

3. 将整个序列 A+B 翻转，得到旋转后的序列。

具体来说，假设要将前 2 个元素旋转到末尾，即将序列 1 2 3 4 5 6 7 8 旋转为 3 45 6 7 8 1 2，那么 rotate 函数的实现过程如下：

1. 将前 2 个元素翻转，得到子序列 A：2 1。

2. 将剩余的 6 个元素翻转，得到子序列 B：8 7 6 5 4 3。

3. 将整个序列 A+B 翻转，得到旋转后的序列：3 4 5 6 7 8 1 2。

可以看到，通过三次翻转操作，我们成功将前 2 个元素旋转到了末尾。这种实现方法的时间复杂度为 O(N)，其中 N 是序列的长度。

需要注意的是，rotate 函数只是将元素旋转到指定位置，并没有改变元素的顺序。如果需要改变元素的顺序，可以使用其他 STL 算法，如 reverse 函数。

<br>

### 交换范围 swap_ranges

STL 中的 swap_ranges 函数用于交换两个容器中指定范围的元素，它接受三个参数：源容器的起始迭代器、目标容器的起始迭代器和指定范围的结束迭代器。

swap_ranges 函数的作用是将源容器和目标容器中指定范围的元素一一交换，可以用于交换两个容器中的部分元素，或将容器中的元素复制到另一个容器中。这个函数非常高效，时间复杂度为 O(N)，其中 N 是交换范围的大小。

以下是一个使用 swap_ranges 函数的例子，演示如何将一个向量中的前三个元素和另一个向量中的后三个元素进行交换：

```c++
#include <iostream>
#include <algorithm>
#include <vector>

int main() {
    std::vector<int> nums1 = {1, 2, 3, 4, 5};
    std::vector<int> nums2 = {6, 7, 8, 9, 10};
    // 将nums1中前三个元素和nums2中后三个元素进行交换
    std::swap_ranges(nums1.begin(), nums1.begin() + 3, nums2.end() - 3);
    // 输出交换后的向量
    for (int num : nums1) {
        std::cout << num <<" ";
    }
    std::cout << std::endl;
    for (int num : nums2) {
        std::cout << num <<" ";
    }
    std::cout << std::endl;
    return 0;
}

// 运行结果
4 5 3 9 10
6 7 8 1 2
```

<br>

swap_ranges 的底层实现十分简单，就是申请一个额外的空间 temp 存储欲交换的元素，然后对对应的容器进行替换即可

这是 swap_ranges 函数的 template 实现

```cpp
template <class ForwardIt1, class ForwardIt2>
ForwardIt2 swap_ranges(ForwardIt1 first1, ForwardIt1 last1, ForwardIt2 first2) {
    for (; first1 != last1; ++first1, ++first2) {
        std::iter_swap(first1, first2);
    }
    return first2;
}
```

<br>

### 合并 merge

STL 中的 merge 函数用于将两个已排序的容器合并成一个有序的容器。  
它接受五个参数：源容器 1 的起始迭代器、源容器 1 的结束迭代器、源容器 2 的起始迭代器、源容器 2 的结束迭代器和目标容器的起始迭代器。

以下是一个使用 merge 函数的例子，演示如何将两个已排序的向量合并成一个新的有序向量：

```c++
#include <iostream>
#include <algorithm>
#include <vector>

int main() {
    std::vector<int> nums1 = {1, 3, 5, 7, 9};
    std::vector<int> nums2 = {2, 4, 6, 8, 10};
    std::vector<int> result(10);
    // 将nums1和nums2合并到result中
    std::merge(nums1.begin(), nums1.end(), nums2.begin(), nums2.end(), result.begin());
    // 输出合并后的向量
    for (int num : result) {
        std::cout << num << " ";
    }
   std::cout << std::endl;
    return 0;
}

// 运行结果：
1 2 3 4 5 6 7 8 9 10
```

<br>

`inplace_merge` 函数用于将一个已排序的容器中指定范围的两个相邻子序列合并成一个有序序列  
它接受三个参数：容器的起始迭代器、指定范围的起始迭代器和指定范围的结束迭代器。inplace_merge 函数的作用是将容器中指定范围的两个相邻子序列合并为一个有序序列，并保持容器的有序性。

以下是一个使用 inplace_merge 函数的例子，演示如何将一个已排序的向量中的两个相邻子序列合并为一个有序序列：

```c++
#include <iostream>
#include <algorithm>
#include <vector>

int main() {
    std::vector<int> nums = {1, 3, 5, 2, 4, 6};
    // 将nums中前三个元素和后三个元素合并
    std::inplace_merge(nums.begin(), nums.begin() + 3, nums.end());
    // 输出合并后的向量
    for (int num : nums) {
        std::cout << num << " ";
    }
    std::cout << std::endl;
    return 0;
}

// 运行结果：
1 2 3 4 5 6
```

<br>

merge_backward 函数与 merge 函数类似，不同之处在于它是从后向前合并两个已排序的容器  
它接受五个参数：源容器 1 的起始迭代器、源容器 1 的结束迭代器、源容器 2 的起始迭代器、源容器 2 的结束迭代器和目标容器的结束迭代器。

以下是一个使用 merge_backward 函数的例子，演示如何将两个已排序的向量合并成一个新的有序向量：

```c++
#include <iostream>
#include <algorithm>
#include <vector>

int main() {
    std::vector<int> nums1 = {1, 3, 5, 7, 9};
    std::vector<int> nums2 = {2, 4, 6, 8, 10};
    std::vector<int> result(10);
    // 将nums1和nums2合并到result中
    std::merge_backward(nums1.begin(), nums1.end(), nums2.begin(), nums2.end(), result.end());
    // 输出合并后的向量
    for (int num : result) {
        std::cout << num << " ";
    }
    std::cout << std::endl;
    return 0;
}

// 运行结果
1 2 3 4 5 6 7 8 9 10
```

<br>

#### merge 底层实现

merge 函数的底层实现原理是基于归并排序的思想。

具体来说，merge 函数会将两个已排序的容器中的元素按照从小到大的顺序合并到一个新的容器中，并保持新容器的有序性。

merge 函数的实现过程如下：

1. 创建一个新的目标容器 result，大小为源容器 1 和源容器 2 的大小之和；

2. 定义三个迭代器：源容器 1 的起始迭代器 first1、源容器 1 的结束迭代器 last1 和源容器 2 的起始迭代器 first2；

3. 使用 while 循环，比较 first1 和 first2 指向的元素大小，将较小的元素插入到 result 中，并将对应的迭代器向前移动；

4. 如果其中一个容器中的元素已经全部插入到 result 中，那么直接将另一个容器中剩余的元素插入到 result 中；

5. 返回 result 的结束迭代器。

以下是一个简化的 merge 函数的示例代码，用于演示其底层实现原理：

```c++
template <typename InputIt1, typename InputIt2, typename OutputIt>
OutputIt merge(InputIt1 first1, InputIt1 last1,
                InputIt2 first2, InputIt2 last2,
                OutputIt result) {
    while (first1 != last1 && first2 != last2) {
        if (*first1 < *first2) {
            *result = *first1;
            ++first1;
        } else {
            *result = *first2;
            ++first2;
        }
        ++result;
    }
    return std::copy(first1, last1, std::copy(first2, last2, result));
}
```

需要注意的是，merge 函数的前提条件是源容器 1 和源容器 2 已经按照从小到大的顺序排序。如果源容器中存在相同的元素，merge 函数会将它们全部插入到 result 中，但不保证它们的顺序。

<br>

### 布尔操作 all_of

STL 中的 all_of 函数是一个布尔操作，用于判断容器中的所有元素是否都满足指定的条件。如果所有元素都满足条件，那么 all_of 函数返回 true；否则返回 false。

以下是 all_of 函数的定义：

```c++
template <typename InputIt, typename UnaryPredicate>
bool all_of(InputIt first, InputIt last, UnaryPredicate p);
```

其中，first 和 last 分别是容器的起始迭代器和结束迭代器，p 是一个一元谓词，用于判断元素是否满足条件。all_of 函数会对`[first, last)`范围内的每个元素调用 p 函数，如果所有元素都满足条件，那么 all_of 函数返回 true，否则返回 false。

以下是一个使用 all_of 函数的例子，演示如何判断一个向量中的所有元素是否都是正数：

```c++
#include <iostream>
#include <algorithm>
#include <vector>

int main() {
    std::vector<int> nums = {1, 2, 3, 4, 5};
    // 判断nums中的所有元素是否都是正数
    bool all_positive = std::all_of(nums.begin(), nums.end(), [](int num) {
        return num > 0;
    });
    if (all_positive) {
        std::cout << "All elements are positive." << std::endl;
    } else {
        std::cout << "Not all elements are positive." << std::endl;
    }
    return 0;
}

// 运行结果
// All elements are positive.
```

<br>

### 针对集合 set 的重要函数

STL 中提供了许多针对集合（set）的算法，这些算法可以用于对集合进行排序、查找、合并、求交等操作，大大简化了集合操作的实现。下面介绍一些常用的集合算法：

1. `set_union函数`：用于计算两个集合的并集。set_union 函数接受四个参数：源集合 1 的起始迭代器、源集合 1 的结束迭代器、源集合 2 的起始迭代器、目标集合的起始迭代器。set_union 函数的作用是将两个集合中的元素合并到目标集合中，并去除重复元素，返回目标集合的结束迭代器。

2. `set_intersection函数`：用于计算两个集合的交集。set_intersection 函数接受四个参数：源集合 1 的起始迭代器、源集合 1 的结束迭代器、源集合 2 的起始迭代器、目标集合的起始迭代器。set_intersection 函数的作用是将两个集合中的共有元素合并到目标集合中，并去除重复元素，返回目标集合的结束迭代器。

3. `set_difference函数`：用于计算两个集合的差集。set_difference 函数接受四个参数：源集合 1 的起始迭代器、源集合 1 的结束迭代器、源集合 2 的起始迭代器、目标集合的起始迭代器。set_difference 函数的作用是将源集合 1 中有但源集合 2 中没有的元素合并到目标集合中，并去除重复元素，返回目标集合的结束迭代器。

4. `set_symmetric_difference函数`：用于计算两个集合的对称差集。set_symmetric_difference 函数接受四个参数：源集合 1 的起始迭代器、源集合 1 的结束迭代器、源集合 2 的起始迭代器、目标集合的起始迭代器。set_symmetric_difference 函数的作用是将源集合 1 和源集合 2 中独有的元素合并到目标集合中，并去除重复元素，返回目标集合的结束迭代器。

5. `includes函数`：用于判断一个集合是否包含另一个集合。includes 函数接受四个参数：源集合 1 的起始迭代器、源集合 1 的结束迭代器、源集合 2 的起始迭代器、源集合 2 的结束迭代器。includes 函数的作用是判断源集合 2 是否是源集合 1 的子集，如果是，返回 true；否则返回 false。

6. set_union、set_intersection、set_difference 和 set_symmetric_difference 函数的实现均基于归并排序的思想，时间复杂度为 O(NlogN)，其中 N 为集合的大小。

<br>
