# Day1 第一章 数组 part01

> 资料来源地址：[代码随想录 (programmercarl.com)](https://programmercarl.com/数组理论基础.html)

## 数组理论基础

### Java

**Java**中一维数组在内存空间上是连续的，但是二维数组在内存空间上不连续。



## 典型例题

### LeetCode704.二分查找

> ==二分==

一般情况下，**使用二分法的前提：**

1. 数组为有序数组
2. 数组中无重复元素

> [!important]
>
> **循环不变量**：对于`left, right`的区间定义，`[left, right)`或者`[left, right]`
>
> 根据区间定义判断循环条件`left < right`或`left <= right`，也要根据区间定义判断循环中对`left, right`赋值时与`middle`的关系。
>
> 个人偏向左闭右开。

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0;
        int right = nums.size(); // 定义target在左闭右开的区间里，即：[left, right)
        while (left < right) { // 因为left == right的时候，在[left, right)是无效的空间，所以使用 <
            int middle = left + ((right - left) >> 1);
            if (nums[middle] > target) {
                right = middle; // target 在左区间，在[left, middle)中
            } else if (nums[middle] < target) {
                left = middle + 1; // target 在右区间，在[middle + 1, right)中
            } else { // nums[middle] == target
                return middle; // 数组中找到目标值，直接返回下标
            }
        }
        // 未找到目标值
        return -1;
    }
};
```

普通循环（迭代）情况下时空复杂度：

- 时间复杂度：$O(\log{n})$
- 空间复杂度：$O(1)$

> 关于`middle = left + (right - left) / 2`，防止溢出



###  LeetCode27.移除元素

> ==双指针（快慢指针）==

> [!important]
>
> **双指针法（快慢指针法）在数组和链表的操作中是非常常见的，很多考察数组、链表、字符串等操作的面试题，都使用双指针法。**

双指针法（快慢指针法）： **通过一个快指针和慢指针在一个for循环下完成两个for循环的工作。**

- 快指针：寻找新数组的元素 ，新数组就是不含有目标元素的数组
- 慢指针：指向更新 新数组下标的位置

```c++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int slowIndex = 0;
        for (int fastIndex = 0; fastIndex < nums.size(); fastIndex++) {
            if (val != nums[fastIndex]) {
                nums[slowIndex++] = nums[fastIndex];
            }
        }
        return slowIndex;
    }
};
```

时空复杂度：

- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$



### LeetCode977.有序数组的平方

> ==双指针==

数组其实是有序的，只不过负数平方之后可能成为最大数了。

那么数组平方的最大值就在数组的两端，不是最左边就是最右边，不可能是中间。

此时可以考虑双指针法了，`i`指向起始位置，`j`指向终止位置。

定义一个新数组`result`，和$A$数组一样的大小，让`k`指向`result`数组终止位置。

```c++
class Solution {
public:
    vector<int> sortedSquares(vector<int>& A) {
        int k = A.size() - 1;
        vector<int> result(A.size(), 0);
        for (int i = 0, j = A.size() - 1; i <= j;) { // 注意这里要i <= j，因为最后要处理两个元素
            if (A[i] * A[i] < A[j] * A[j])  {
                result[k--] = A[j] * A[j];
                j--;
            }
            else {
                result[k--] = A[i] * A[i];
                i++;
            }
        }
        return result;
    }
};
```

时间复杂度：$O(n)$





