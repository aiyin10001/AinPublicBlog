+++
date = '2026-01-30T18:22:52+08:00'
title = 'LeetCode 数组part2'
readingTime = true
description = '掉入数组的泥潭……'
categories = [
    "LeetCode",
]
tags = [
    "LeetCode",
]
image="tu.png"
+++

# 数组 Part 2 之 二分查找

>   **二分查找算法（Binary Search Algorithm）**，又称折半查找或对数查找，是一种在有序数组中高效定位目标元素的方法。其核心思想是每次将查找区间缩小一半，从而快速锁定目标位置。

## 二分查找思想

二分查找算法体现了经典的 **「减而治之」** 思想。

所谓 **「减」**，就是每一步都通过条件判断，排除掉一部分一定不包含目标元素的区间，从而缩小问题规模；**「治」**，则是在缩小后的区间内继续解决剩下的子问题。也就是说，二分查找的核心在于：**每次查找都排除掉不可能存在目标的区间，仅在可能存在目标的区间内继续查找**。

通过不断缩小查找区间，问题规模逐步减小。由于区间有限，经过有限次迭代，最终要么找到目标元素，要么确定目标不存在于数组中。  

## 两种二分查找问题解决思路

### **直接法**

**二分查找** 是一种在 **有序数组** 中高效查找目标元素的算法，其核心思想是 **每次将查找区间缩小一半**，从而快速定位目标位置。

1.  初始化左右边界，令` left=0`，`right=len(nums)−1`，即查找区间为 [left,right]（左闭右闭）。
2.  在每轮循环中，计算中间位置 `mid`，比较 `nums[mid]`与目标值 `target`：
    1.  如果 `nums[mid]==target`，直接返回 `mid`。
    2.  如果 `nums[mid]<target`，则目标值只可能在右半区间，将 left更新为 `mid+1`。
    3.  如果 `nums[mid]>target`，则目标值只可能在左半区间，将 right 更新为 `mid−1`。
3.  当 `left>right`时，说明查找区间已为空，目标值不存在，返回 −1。

```java
   /**
     * 标准二分查找算法
     */
    public int BinarySearch(int[] nums, int key) {
        var l = 0;
        var h = nums.length - 1;
        while (l <= h) {
            var mid = (h + l) / 2;
            if (nums[mid] == key) {
                return mid;
            } else if (nums[mid] < key) {
                l = mid + 1;
            } else {
                h = mid - 1;
            }
        }
        return -1;
    }
```

**细节**

-   这种思路是在一旦循环体中找到元素就直接返回。
-   循环可以继续的条件是 `left <= right`。
-   如果一旦退出循环，则说明这个区间内一定不存在目标元素。

### **排除法**

>   **排除法思想**：每轮循环都优先排除掉一定不包含目标元素的区间，仅在可能存在目标的区间内继续查找。

1.  初始化左右边界` left=0`，`right=len(nums)−1`，查找区间为 `[left,right]`（左闭右闭）。
2.  每次计算中间位置 `mid`，比较 `nums[mid]`与 `target`，优先排除掉目标元素一定不存在的区间。
3.  在剩余的区间内继续查找，重复上述过程。
4.  当区间收缩到只剩一个元素时，判断该元素是否为目标值。

基于排除法，可以实现两种常见写法：

>   **一个小心得**：*<u>如果是`low = mid + 1;`就要向下取整，如果是`high = mid - 1;` 就要向上取整。这样才能保证在出现与key相等值时候，无死循环。</u>*

```java
    public int binarySearch_1(int[] nums, final int key) {
        var low = 0;
        var high = nums.length - 1;
        while (low < high) {
            // 特别注意：为防止死循环，此处必须向下取整
            var mid = low + (high - low) / 2;
            if (nums[mid] < key) {
                low = mid + 1;
            } else {
                high = mid;
            }
        }
        return nums[low] == key ? low : -1;
    }
```

```java
    public int binarySearch_2(final int[] nums, final int key) {
        var low = 0;
        var high = nums.length - 1;
        while (low < high) {
            // 特别注意：为防止死循环，此处必须向上取整
            var mid = low + (high - low + 1) / 2;
            if (nums[mid] > key) high = mid - 1;
            else low = mid;
        }
        return nums[low] == key ? low : -1;
    }
```

**细节**

-   循环条件采用 `left < right`，这样循环结束时必然有 `left == right`，无需再区分返回 `left`还是 `right`，只需判断 `nums[left] `是否为目标值即可。
-   在循环体内，先比较目标值与中间元素的大小，优先排除目标值不可能存在的区间，然后在剩余区间继续查找。
-   排除目标值不可能存在的区间后，`else` 分支通常直接取剩余的另一半区间，无需额外判断。例如，如果排除 [left,mid]，则剩余区间为 [mid+1,right]；如果排除 [mid,right]，则剩余区间为 [left,mid−1]。
-   为避免死循环，当区间被划分为 [left,mid−1]和 [mid,right]时，**mid 需要向上取整**，即 `mid = left + (right - left + 1) // 2`。因为当区间只剩两个元素（right=left+1）时，如果 mid向下取整，`left = mid` 会导致区间不变，陷入死循环。
    -   例如 left=5，right=6，如果 mid=5，执行 left=mid后区间仍为 [5,6]，无法收缩，导致死循环。
    -   如果 mid 向上取整，mid=6，执行 left=mid后区间变为 [6,6]，循环得以终止。
-   边界设置可记忆为：只要出现 `left = mid`，就要让 mid向上取整。具体配对如下：
    -   `left = mid + 1`、`right = mid` 搭配 `mid = left + (right - left) // 2`。
    -   `right = mid - 1`、`left = mid` 搭配 `mid = left + (right - left + 1) // 2`。

### 适用范围

-   **直接法**：因为判断语句是 `left <= right`，有时候要考虑返回是 left 还是 right。循环体内有 3 个分支，并且一定有一个分支用于退出循环或者直接返回。这种思路适合解决简单题目。即要查找的元素性质简单，数组中都是非重复元素，且 `==`、`>`、`<` 的情况非常好写的时候。
-   **排除法**：更加符合二分查找算法的减治思想。每次排除目标元素一定不存在的区间，达到减少问题规模的效果。然后在可能存在的区间内继续查找目标元素。这种思路适合解决复杂题目。比如查找一个数组里可能不存在的元素，找边界问题，可以使用这种思路。

## 总结

1.   二分查找的细节问题包括区间开闭、mid取值、循环条件和搜索范围的选择。

2.   **区间开闭**：建议使用左闭右闭区间，这样逻辑更简单，减少出错可能。

3.   **mid取值**：通常使用 `mid = left + (right - left) / 2`，防止整型溢出。在某些情况下，如 `left = mid` 时，需向上取整，避免死循环。

4.   **循环条件**：

     -   `left <= right`：适用于直接法，循环结束时如果未找到目标，直接返回 −1。

     -   `left < right`：适用于排除法，循环结束时需额外判断 `nums[left]` 是否为目标值。

5.   **搜索范围选择**：
     -   直接法：`left = mid + 1` 或 `right = mid - 1`，明确缩小范围。
     -   排除法：根据情况选择 `left = mid + 1` 或 `right = mid`，以及 `right = mid - 1` 或 `left = mid`，确保每次排除无效区间。

6.   **两种思路**：
     -   **直接法**：简单直接，适合查找明确存在的元素。
     -   **排除法**：更通用，适合复杂问题，如边界查找或不确定元素是否存在的情况。

## 二分查找题目

### 二分下标题目

#### Easy

-   [704. 二分查找](https://leetcode.cn/problems/binary-search/)

    ```java
    package org.lc.search;
    
    /**
     * 创建时间: 2026/02/03
     * 作者: Ain
     * 链接：https://leetcode.cn/problems/binary-search/description/
     * 题目：704. 二分查找
     * 难度：easy
     * Thinking：_1_min
     * Coding:_1_min
     * 思路：无他，二分查找
     * 卡点：
     */
    public class Lc0704 {
        public int search(int[] nums, int target) {
            var l = 0;
            var h = nums.length - 1;
            while (l <= h) {
                var mid = l + (h - l) / 2;
                if (nums[mid] == target) return mid;
                if (nums[mid] < target) l = mid + 1;
                else h = mid - 1;
            }
            return -1;
        }
    }
    ```

-   [35. 搜索插入位置](https://leetcode.cn/problems/search-insert-position/)

    ```java
    package org.lc.search;
    
    /**
     * 创建时间: 2026/02/03
     * 作者: Ain
     * 链接：https://leetcode.cn/problems/search-insert-position/description/
     * 题目：35. 搜索插入位置
     * 难度：easy
     * Thinking：_2_min
     * Coding:_1_min
     * 思路：
     * 卡点：
     */
    public class Lc0035 {
        public int searchInsert(int[] nums, int target) {
            var l = 0;
            var h = nums.length - 1;
            while (l <= h) {
                var mid = (h - l) / 2 + l;
                if (nums[mid] == target) return mid;
                if (nums[mid] < target) l = mid + 1;
                else h = mid - 1;
            }
            return l;
        }
    }
    ```

-   [278. 第一个错误的版本](https://leetcode.cn/problems/first-bad-version/)

    ```java
    package org.lc.search;
    
    /**
     * 创建时间: 2026/02/03
     * 作者: Ain
     * 链接：https://leetcode.cn/problems/first-bad-version/description/
     * 题目：278. 第一个错误的版本
     * 难度：easy
     * Thinking：_1_min
     * Coding:_2_min
     * 思路：二分查找，需要排除区间，判断位置
     * 卡点：注意思考取整方向
     */
    public class Lc0278 {
        public int firstBadVersion(int n) {
            var l = 1;
            var h = n;
            while (l < h) {
                var mid = l + (h - l) / 2;
                if (isBadVersion(mid)) h = mid;
                else l = mid + 1;
            }
            return l;
        }
    
        boolean isBadVersion(int version) {
            return false;
        }
    }
    ```

#### Medium

-   [34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/)

    ```java
    package org.lc.search;
    
    /**
     * 创建时间: 2026/02/03
     * 作者: Ain
     * 链接：https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/description/
     * 题目：34. 在排序数组中查找元素的第一个和最后一个位置
     * 难度：medium
     * Thinking：_1_min
     * Coding:_5_min
     * 思路：查找两次，一次找最左边，一次找最右边。
     * 卡点：
     */
    public class Lc0034 {
        public int[] searchRange(int[] nums, int target) {
            int[] ans = {-1, -1};
            if (nums.length < 1) return ans;
            var l = 0;
            var h = nums.length - 1;
            while (l < h) {
                var mid = l + (h - l + 1) / 2;
                if (nums[mid] <= target) l = mid;
                else h = mid - 1;
            }
            if (nums[l] != target) return ans;
            ans[1] = l;
            l = 0;
            h = nums.length - 1;
            while (l < h) {
                var mid = l + (h - l) / 2;
                if (nums[mid] >= target) h = mid;
                else l = mid + 1;
            }
            ans[0] = l;
            return ans;
        }
    }
    ```

-   [167. 两数之和 II - 输入有序数组](https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted/)

    ```java
    package org.lc.search;
    
    /**
     * 创建时间: 2026/02/03
     * 作者: Ain
     * 链接：https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted/description/
     * 题目：167. 两数之和 II - 输入有序数组
     * 难度：medium
     * Thinking：_1_min
     * Coding:__min
     * 思路：两个思路，一个是双指针，一个是查找，先按照二分查找来做。
     * 卡点：要注意到可能出现重复元素，需要寻找最右侧符合要求到数。
     */
    public class Lc0167 {
        public int[] twoSum(int[] numbers, int target) {
            for (int i = 0; i < numbers.length; i++) {
                var index = search(numbers, target - numbers[i]);
                if (index != -1) {
                    return new int[]{i + 1, index + 1};
                }
            }
            return null;
        }
    
        private int search(int[] nums, int key) {
            var l = 0;
            var h = nums.length - 1;
            while (l < h) {
                var mid = l + (h - l + 1) / 2;
                if (nums[mid] > key) h = mid - 1;
                else l = mid;
            }
            return nums[l] == key ? l : -1;
        }
    }
    ```

-   [153. 寻找旋转排序数组中的最小值](https://leetcode.cn/problems/find-minimum-in-rotated-sorted-array/)

    ```java
    package org.lc.search;
    
    /**
     * 创建时间: 2026/02/04
     * 作者: Ain
     * 链接：https://leetcode.cn/problems/find-minimum-in-rotated-sorted-array/description/
     * 题目：153. 寻找旋转排序数组中的最小值
     * 难度：medium
     * Thinking：_4_min
     * Coding:_8_min
     * 思路：递归二分查找
     * 卡点：
     */
    public class Lc0153 {
        public int findMin(int[] nums) {
            return nums.length == 1 ? nums[0] : nums[findMin(nums, 0, nums.length - 1)];
        }
    
        private int findMin(int[] nums, int l, int h) {
            if (l > h) return -1;
            var mid = l + (h - l) / 2;
            var len = nums.length;
            var pre = nums[(mid - 1 + len) % len];
            var next = nums[(mid + 1) % len];
            if (nums[mid] > pre && nums[mid] > next) return (mid + 1) % len;
            if (nums[mid] < pre && nums[mid] < next) return mid;
            var left = findMin(nums, l, mid - 1);
            return left != -1 ? left : findMin(nums, mid + 1, h);
        }
    }
    ```

-   [33. 搜索旋转排序数组](https://leetcode.cn/problems/search-in-rotated-sorted-array/)

    ```java
    package org.lc.search;
    
    /**
     * 创建时间: 2026/02/04
     * 作者: Ain
     * 链接：https://leetcode.cn/problems/search-in-rotated-sorted-array/description/
     * 题目：33. 搜索旋转排序数组
     * 难度：medium
     * Thinking：_5_min
     * Coding:_7_min
     * 思路：1. 找最小值下标；
     * 1.1 排除法二分查找找下标。
     * 2. 分段查找。
     * 卡点：
     */
    public class Lc0033 {
        public int search(int[] nums, int target) {
            var min = findMinIndex(nums);
            var left = find(nums, 0, min - 1, target);
            return left != -1 ? left : find(nums, min, nums.length - 1, target);
        }
    
        private int findMinIndex(int[] nums) {
            int l = 0;
            int h = nums.length - 1;
            while (l < h) {
                var mid = l + (h - l) / 2;
                if (nums[mid] > nums[h]) l = mid + 1;
                else h = mid;
            }
            return l;
        }
    
        private int find(int[] nums, int l, int h, int key) {
            while (l <= h) {
                var mid = l + (h - l) / 2;
                if (nums[mid] == key) return mid;
                if (nums[mid] < key) l = mid + 1;
                else h = mid - 1;
            }
            return -1;
        }
    }
    ```

    ***思考***

    <u>二分查找关注点之一，与[mid]比较的元素，即比较器写法。</u>

    ```java
    package org.lc.search;
    
    /**
     * 创建时间: 2026/02/04
     * 作者: Ain
     * 链接：https://leetcode.cn/problems/search-in-rotated-sorted-array/description/
     * 题目：33. 搜索旋转排序数组
     * 难度：medium
     */
    public class Lc0033_ {
        public int search(int[] nums, int target) {
            int l = 0;
            int h = nums.length - 1;
            while (l <= h) {
                var mid = l + (h - l) / 2;
                if (nums[mid] == target) return mid;
                if (nums[mid] >= nums[l]) {
                    if (target < nums[mid] && nums[l] <= target) {
                        h = mid - 1;
                    } else {
                        l = mid + 1;
                    }
                } else {
                    if (target > nums[mid] && target <= nums[h]) {
                        l = mid + 1;
                    } else {
                        h = mid - 1;
                    }
                }
            }
            return -1;
        }
    }
    ```

-   [81. 搜索旋转排序数组 II](https://leetcode.cn/problems/search-in-rotated-sorted-array-ii/)

    ```java
    package org.lc.search;
    
    /**
     * 创建时间: 2026/02/04
     * 作者: Ain
     * 链接：https://leetcode.cn/problems/search-in-rotated-sorted-array-ii/description/
     * 题目：81. 搜索旋转排序数组 II
     * 难度：medium
     * Thinking：_12_min
     * Coding:_7_min
     * 思路：同https://leetcode.cn/problems/find-minimum-in-rotated-sorted-array/，如果是[mid] == [l]，l++。
     * 卡点：
     */
    public class Lc0081 {
        public boolean search(int[] nums, int target) {
            int l = 0;
            int h = nums.length - 1;
            while (l <= h) {
                var mid = l + (h - l) / 2;
                if (nums[mid] == target) return true;
                if (nums[mid] > nums[l]) {
                    if (target >= nums[l] && target < nums[mid]) h = mid - 1;
                    else l = mid;
                } else if (nums[mid] < nums[l]) {
                    if (target > nums[mid] && target <= nums[h]) l = mid + 1;
                    else h = mid;
                } else {
                    l = l + 1;
                }
            }
            return false;
        }
    }
    ```

-   [162. 寻找峰值](https://leetcode.cn/problems/find-peak-element/)

    ```java
    package org.lc.search;
    
    /**
     * 创建时间: 2026/02/04
     * 作者: Ain
     * 链接：https://leetcode.cn/problems/find-peak-element/description/
     * 题目：162. 寻找峰值
     * 难度：medium
     * Thinking：_2_min
     * Coding:__min
     * 思路：二分查找，如果是↗️则向右寻找位置，如果是↘️则向左寻找位置
     * 卡点：
     */
    public class Lc0162 {
        public int findPeakElement(int[] nums) {
            var l = 0;
            var h = nums.length - 1;
            while (l < h) {
                var mid = l + (h - l + 1) / 2;
                if (mid > 0 && nums[mid - 1] < nums[mid]) l = mid;
                else h = mid - 1;
            }
            return l;
        }
    }
    ```

-   [0852. 山脉数组的峰顶索引](https://leetcode.cn/problems/peak-index-in-a-mountain-array/)

    ```java
    package org.lc.search;
    
    /**
     * 创建时间: 2026/02/04
     * 作者: Ain
     * 链接：https://leetcode.cn/problems/peak-index-in-a-mountain-array/description/
     * 题目：0852. 山脉数组的峰顶索引
     * 难度：medium
     * Thinking：_1_min
     * Coding:_2_min
     * 思路：同[0162. 寻找峰值](https://leetcode.cn/problems/find-peak-element/)
     * 卡点：
     */
    public class Lc0852 {
        public int peakIndexInMountainArray(int[] arr) {
            int l = 0;
            int h = arr.length - 1;
            while (l < h) {
                var mid = l + (h - l + 1) / 2;
                if (mid > 0 && arr[mid - 1] < arr[mid]) l = mid;
                else h = mid - 1;
            }
            return l;
        }
    }
    ```

-   [0074. 搜索二维矩阵](https://leetcode.cn/problems/search-a-2d-matrix/)

    ```java
    package org.lc.search;
    
    /**
     * 创建时间: 2026/02/04
     * 作者: Ain
     * 链接：https://leetcode.cn/problems/search-a-2d-matrix/description/
     * 题目：0074. 搜索二维矩阵
     * 难度：medium
     * Thinking：_5_min
     * Coding:_4_min
     * 思路：找到二维数组下标ij和一维数组下标k的对应关系，按照一维数组解题即可。
     * 卡点：
     */
    public class Lc0074 {
        public boolean searchMatrix(int[][] matrix, int target) {
            int m = matrix.length, n = matrix[0].length, r = m * n - 1, l = 0;
            while (l <= r) {
                var mid = l + (r - l) / 2;
                int i = mid / n;
                int j = mid % n;
                if (matrix[i][j] == target) return true;
                if (matrix[i][j] < target) l = mid + 1;
                else r = mid - 1;
            }
            return false;
        }
    }
    ```

-   [0240. 搜索二维矩阵 II](https://leetcode.cn/problems/search-a-2d-matrix-ii/)

    ```java
    package org.lc.search;
    
    /**
     * 创建时间: 2026/02/04
     * 作者: Ain
     * 链接：https://leetcode.cn/problems/search-a-2d-matrix-ii/description/
     * 题目：0240. 搜索二维矩阵 II
     * 难度：medium
     * Thinking：_1_min
     * Coding:_2_min
     * 思路：遍历/按行二分/按列二分
     * 卡点：尴尬，看错题意了。
     */
    public class Lc0240 {
        public boolean searchMatrix(int[][] matrix, int target) {
            for (int[] ints : matrix) {
                for (int anInt : ints) {
                    if (anInt == target) return true;
                }
            }
            return false;
        }
    }
    ```

#### Hard

-   [154. 寻找旋转排序数组中的最小值 II](https://leetcode.cn/problems/find-minimum-in-rotated-sorted-array-ii/)

    ```java
    package org.lc.search;
    
    /**
     * 创建时间: 2026/02/04
     * 作者: Ain
     * 链接：https://leetcode.cn/problems/find-minimum-in-rotated-sorted-array-ii/description/
     * 题目：154. 寻找旋转排序数组中的最小值 II
     * 难度：hard
     * Thinking：_∞_min
     * Coding:__min
     * 思路：创建两个指针 `left`、`right`，分别指向数组首尾。然后计算出两个指针中间值 `mid`。将 `mid` 与右边界进行比较。
     * 1.  如果 `nums[mid]>nums[right]`，则最小值不可能在 `mid` 左侧，一定在 `mid`右侧，则将 `left`移动到 `mid+1 `位置，继续查找右侧区间。
     * 2.  如果 `nums[mid]<nums[right]`，则最小值一定在 `mid`左侧，令右边界 `right`为 `mid`，继续查找左侧区间。
     * 3.  如果 `nums[mid]==nums[right]`，无法判断在 `mid`的哪一侧，可以采用 ``right = right - 1`` 逐步缩小区域。
     * 卡点：
     */
    public class Lc0154 {
        public int findMin(int[] nums) {
            int l = 0;
            int r = nums.length - 1;
            while (l < r) {
                var mid = l + (r - l) / 2;
                if (nums[mid] > nums[r]) l = mid + 1;
                else if (nums[mid] < nums[r]) r = mid;
                else r--;
            }
            return nums[l];
        }
    }
    ```

    创建两个指针 `left`、`right`，分别指向数组首尾。然后计算出两个指针中间值 `mid`。将 `mid` 与右边界进行比较。

    1.  如果 `nums[mid]>nums[right]`，则最小值不可能在 `mid` 左侧，一定在 `mid`右侧，则将 `left`移动到 `mid+1 `位置，继续查找右侧区间。
    2.  如果 `nums[mid]<nums[right]`，则最小值一定在 `mid`左侧，令右边界 `right`为 `mid`，继续查找左侧区间。
    3.  如果 `nums[mid]==nums[right]`，无法判断在 `mid`的哪一侧，可以采用 ``right = right - 1`` 逐步缩小区域。

-   [1095. 山脉数组中查找目标值](https://leetcode.cn/problems/find-in-mountain-array/)

    ```java
    package org.lc.search;
    
    /**
     * 创建时间: 2026/02/05
     * 作者: Ain
     * 链接：https://leetcode.cn/problems/find-in-mountain-array/description/
     * 题目：1095. 山脉数组中查找目标值
     * 难度：Hard
     * Thinking：_6_min
     * Coding:_8_min
     * 思路：1. 先用二分查找找到峰值下标；
     * 2. 在左侧二分查找target；
     * 3. 在右侧二分查找target；
     * 卡点：注意右侧是倒序
     */
    public class Lc1095 {
        public int findInMountainArray(int target, MountainArray mountainArr) {
            var len = mountainArr.length();
            var l = 0;
            var r = len - 1;
            while (l < r) {
                var mid = l + (r - l + 1) / 2;
                if (mid > 0 && mountainArr.get(mid) > mountainArr.get(mid - 1)) l = mid;
                else r = mid - 1;
            }
            var top = l;
            var left = searchLeft(mountainArr, target, 0, top);
            return left != -1 ? left : searchRight(mountainArr, target, top, len - 1);
        }
    
        private int searchLeft(MountainArray mountainArr, int key, int l, int r) {
            while (l <= r) {
                var mid = l + (r - l) / 2;
                var midValue = mountainArr.get(mid);
                if (midValue == key) return mid;
                if (midValue < key) l = mid + 1;
                else r = mid - 1;
            }
            return -1;
        }
    
        private int searchRight(MountainArray mountainArr, int key, int l, int r) {
            while (l <= r) {
                var mid = l + (r - l) / 2;
                var midValue = mountainArr.get(mid);
                if (midValue == key) return mid;
                if (midValue < key) r = mid - 1;
                else l = mid + 1;
            }
            return -1;
        }
    }
    
    interface MountainArray {
        public int get(int index);
    
        public int length();
    }
    ```

-   [4. 寻找两个正序数组的中位数](https://leetcode.cn/problems/median-of-two-sorted-arrays/)

    ```java
    package org.lc.search;
    
    /**
     * 创建时间: 2026/02/05
     * 作者: Ain
     * 链接：https://leetcode.cn/problems/median-of-two-sorted-arrays/description/
     * 题目：4. 寻找两个正序数组的中位数
     * 难度：Hard
     * Thinking：_16_min
     * Coding:_∞_min
     * 思路：思路1: 合并有序数组，然后计算中位数，时间复杂度O(m+n),空间复杂度O(m+n);
     * 思路2: 二分查找，没想明白。
     * 卡点：
     */
    public class Lc0004 {
        public double findMedianSortedArrays(int[] nums1, int[] nums2) {
            int len1 = nums1.length, len2 = nums2.length;
            int[] nums = new int[len1 + len2];
            int i1 = 0, i2 = 0, i = 0;
            while (i1 < len1 && i2 < len2) {
                if (nums1[i1] <= nums2[i2]) {
                    nums[i] = nums1[i1];
                    i1++;
                } else {
                    nums[i] = nums2[i2];
                    i2++;
                }
                i++;
            }
    
            while (i1 < len1) {
                nums[i] = nums1[i1];
                i1++;
                i++;
            }
    
            while (i2 < len2) {
                nums[i] = nums2[i2];
                i2++;
                i++;
            }
            return ((double) nums[nums.length / 2] + nums[(nums.length - 1) / 2]) / 2d;
        }
    }
    ```

    ***其他思路***

    **中位数把数组分割成了左右两部分，并且左右两部分元素个数相等。**

    问题就可以进一步转换为：**如何从 nums1数组中取出前 m1 个元素，使得 nums1第 m1个元素或者 nums2 第 m2=k−m1 个元素为中位线位置**。

    建议直接去看 ***[题解 ](https://algo.itcharge.cn/solutions/0001-0099/median-of-two-sorted-arrays/#%E6%80%9D%E8%B7%AF-1-%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE)***或者 ***[官方题解](https://leetcode.cn/problems/median-of-two-sorted-arrays/solutions/258842/xun-zhao-liang-ge-you-xu-shu-zu-de-zhong-wei-s-114/)***。

    ```java
    package org.lc.search;
    
    /**
     * 创建时间: 2026/02/05
     * 作者: Ain
     * 链接：https://leetcode.cn/problems/median-of-two-sorted-arrays/description/
     * 题目：4. 寻找两个正序数组的中位数
     * 难度：Hard
     */
    public class Lc0004_ {
        public double findMedianSortedArrays(int[] nums1, int[] nums2) {
            if (nums1.length > nums2.length) {
                return findMedianSortedArrays(nums2, nums1);
            }
            var len1 = nums1.length;
            var len2 = nums2.length;
            var k = (len1 + len2 + 1) / 2;
    
            var l = 0;
            var r = len1;
    
            var c1 = 0;
            var c2 = 0;
            while (l <= r) {
                var m1 = l + (r - l) / 2;
                var m2 = k - m1;
    
                var nums1_i_1 = (m1 == 0 ? Integer.MIN_VALUE : nums1[m1 - 1]);
                var nums1_i = (m1 == len1 ? Integer.MAX_VALUE : nums1[m1]);
    
                var nums2_j_1 = (m2 == 0 ? Integer.MIN_VALUE : nums2[m2 - 1]);
                var nums2_j = (m2 == len2 ? Integer.MAX_VALUE : nums2[m2]);
    
                if (nums1_i_1 < nums2_j) {
                    c1 = Math.max(nums1_i_1, nums2_j_1);
                    c2 = Math.min(nums1_i, nums2_j);
                    l = m1 + 1;
                } else r = m1 - 1;
            }
    
            return ((len1 + len2) & 1) == 1 ? c1 : ((double) c1 + c2) / 2;
        }
    }
    ```

### 二分答案题目

#### Easy

-   [69. x 的平方根 ](https://leetcode.cn/problems/sqrtx/)

    ```java
    package org.lc.search.answerSearch;
    
    /**
     * 创建时间: 2026/02/05
     * 作者: Ain
     * 链接：https://leetcode.cn/problems/sqrtx/description/
     * 题目：69. x 的平方根
     * 难度：Easy
     * Thinking：_1_min
     * Coding:_2_min
     * 思路：在0～x-1上二分查找
     * 卡点：注意到了边界值溢出，没注意到mid^2溢出。
     */
    public class Lc0069 {
        public int mySqrt(int x) {
            if (x == 0) return 0;
            int l = 1;
            int r = x - 1;
            while (l < r) {
                var mid = l + (r - l + 1) / 2;
                if ((long) mid * mid > x) r = mid - 1;
                else l = mid;
            }
            return l;
        }
    }
    ```

-   [0367. 有效的完全平方数](https://leetcode.cn/problems/valid-perfect-square/)

    ```java
    package org.lc.search.answerSearch;
    
    /**
     * 创建时间: 2026/02/05
     * 作者: Ain
     * 链接：https://leetcode.cn/problems/valid-perfect-square/description/
     * 题目：367. 有效的完全平方数
     * 难度：Easy
     * Thinking：_1_min
     * Coding:_2_min
     * 思路：
     * 卡点：
     */
    public class Lc0367 {
        public boolean isPerfectSquare(int num) {
            long l = 1;
            long r = num;
            while (l <= r) {
                long mid = l + (r - l + 1) / 2;
                if (mid * mid == num) return true;
                if (mid * mid < num) l = mid + 1;
                else r = mid - 1;
            }
            return false;
        }
    }
    ```

#### Medium

-   [0287. 寻找重复数](https://leetcode.cn/problems/find-the-duplicate-number/)

    ```java
    package org.lc.search.answerSearch;
    
    /**
     * 创建时间: 2026/02/05
     * 作者: Ain
     * 链接：https://leetcode.cn/problems/find-the-duplicate-number/description/
     * 题目：0287. 寻找重复数
     * 难度：Medium
     * Thinking：_∞_min
     * Coding:__min
     * 思路：因为是1～(n+1)位置填1～n，如果mid位置，一侧位置<mid的数量少于mid，说明左侧松散，另一侧就拥挤，结果出现在另一侧，反之亦然。
     * 卡点：
     */
    public class Lc0287 {
        public int findDuplicate(int[] nums) {
            int l = 1;
            int r = nums.length - 1;
            while (l < r) {
                var mid = l + (r - l) / 2;
                if (lessThanKey(nums, mid) <= mid) l = mid + 1;
                else r = mid;
            }
            return l;
        }
    
        private int lessThanKey(int[] nums, int key) {
            int res = 0;
            for (int num : nums) {
                if (num <= key) res++;
            }
            return res;
        }
    }
    ```

-   [0050. Pow(x, n)](https://leetcode.cn/problems/powx-n/)

    ```java
    package org.lc.search.answerSearch;
    
    /**
     * 创建时间: 2026/02/05
     * 作者: Ain
     * 链接：https://leetcode.cn/problems/powx-n/description/
     * 题目：50. Pow(x, n)
     * 难度：Medium
     * Thinking：_∞_min
     * Coding:__min
     * 思路：分治思想
     * 卡点：
     */
    public class Lc0050 {
        public double myPow(double x, int n) {
            return myPow(x, (long) n);
        }
    
        /**
         * @param n 防止取反时溢出。
         * */
        private double myPow(double x, long n) {
            if (x == 0d) return 0;
            if (n < 0) return 1.0 / myPow(x, -n);
            double ans = 1;
            double tmp = x;
            while (n > 0) {
                if ((n & 1) == 1) {
                    ans *= tmp;
                }
                tmp *= tmp;
                n /= 2;
            }
            return ans;
        }
    }
    ```

-   [1300. 转变数组后最接近目标值的数组和](https://leetcode.cn/problems/sum-of-mutated-array-closest-to-target/)

    ```java
    ```

    

-   [400. 第 N 位数字](https://leetcode.cn/problems/nth-digit/)

    ```java
    ```

    

-   

### 复杂二分查找题目

#### Easy

-   





#### Medium





#### Hard









