+++
date = '2026-01-27T10:10:31+08:00'
title = 'LeetCode 数组 Part 1'
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

# 数组 Part 1

## 数组基础 （一维数组、二维数组）

- [0066. 加一 - 力扣](https://leetcode.cn/problems/plus-one/)

    ```java
    package org.lc;
    
    /**
     * 创建时间: 2026/01/27 10:46
     * 作者: Ain
     * 链接：https://leetcode.cn/problems/plus-one/description/
     * 题目：66. 加一
     * 难度：easy
     * Thinking：2min
     * Coding：5min
     * 思路：设置进位标志位，
     *      循环结束前进位为假时，直接返回，
     *      循环结束后，判断进位，为假返回结果；为真返回1000……00
     */
    public class Lc0066 {
        public int[] plusOne(int[] digits) {
            var carry = true;
            if (digits.length == 0) return new int[]{1};
            for (int i = digits.length - 1; i >= 0; i--) {
                if (carry) {
                    if (digits[i] == 9) {
                        digits[i] = 0;
                    } else {
                        carry = false;
                        digits[i] += 1;
                    }
                } else {
                    return digits;
                }
            }
            if (carry) {
                int[] res = new int[digits.length + 1];
                res[0] = 1;
                for (int i = 1; i < res.length; i++) {
                    res[i] = 0;
                }
                return res;
            } else {
                return digits;
            }
        }
    }
    ```

- [0724. 寻找数组的中心下标](https://leetcode.cn/problems/find-pivot-index/)

    ```java
    package org.lc;
    
    import java.util.Arrays;
    
    /**
     * 创建时间: 2026/01/27 10:59
     * 作者: Ain
     * 链接：https://leetcode.cn/problems/find-pivot-index/description/
     * 题目：724. 寻找数组的中心下标
     * 难度：easy
     * Thinking:_1_min
     * Coding:_3_min
     * 思路：分别记录左右两侧的和，注意leftSum、rightSum加减和比较的时机即可
     */
    public class Lc0724 {
        public int pivotIndex(int[] nums) {
    //        var right = Arrays.stream(nums).sum(); 耗时？
            var right = 0;
            for (int num : nums) {
                right += num;
            }
            var left = 0;
            for (int i = 0; i < nums.length; i++) {
                right -= nums[i];
                if (left == right) return i;
                else left += nums[i];
            }
            return -1;
        }
    }
    
    ```

-   [0189. 轮转数组 - 力扣](https://leetcode.cn/problems/rotate-array/)

    ```java
    package org.lc;
    
    /**
     * 创建时间: 2026/01/27 11:35
     * 作者: Ain
     * 链接：https://leetcode.cn/problems/rotate-array/description/
     * 题目：189. 轮转数组
     * 难度：medium
     * Thinking：_4_min
     * Coding:_7_min
     * 思路：copy一份要移动的后半部分，移动完前部分，拼接后部分
     */
    public class Lc0189 {
        public void rotate(int[] nums, int k) {
            var n = nums.length;
            var kk = k % n;
            int[] res = new int[kk];
            for (int i = 0; i < kk; i++) {
                res[i] = nums[n - kk + i];
            }
            for (int i = n - 1; i >= 0; i--) {
                if (i >= kk) {
                    nums[i] = nums[i - kk];
                } else {
                    nums[i] = res[i];
                }
            }
        }
    }
    ```

    ***进阶思路***

    可以先把整个数组翻转一下，这样后半段元素就到了前边，前半段元素就到了后边，只不过元素顺序是反着的。

    再从 *k*位置分隔开，将 [0...k−1][0...*k*−1] 区间上的元素和 [k...n−1][*k*...*n*−1] 区间上的元素再翻转一下，就得到了最终结果。

    具体步骤：

    1.   将数组 [0,n−1][0,*n*−1] 位置上的元素全部翻转。

    2.   将数组 [0,k−1][0,*k*−1] 位置上的元素进行翻转。

    3.   将数组 [k,n−1][*k*,*n*−1] 位置上的元素进行翻转。

    ```java
    package org.lc;
    
    /**
     * 创建时间: 2026/01/27 12:08
     * 作者: Ain
     * 链接：https://leetcode.cn/problems/rotate-array/description/
     * 题目：189. 轮转数组
     * 难度：medium
     * Thinking：_1_min
     * Coding:_3_min
     * 思路：可以先把整个数组翻转一下，这样后半段元素就到了前边，前半段元素就到了后边，只不过元素顺序是反着的。
     * 再从 *k* 位置分隔开，将 [0...k−1][0...*k*−1] 区间上的元素和 [k...n−1][*k*...*n*−1] 区间上的元素再翻转一下，就得到了最终结果。
     */
    public class Lc0189_ {
        public void rotate(int[] nums, int k) {
            int kk = k % nums.length;
            rotate(nums, 0, nums.length - 1);
            rotate(nums, 0, kk - 1);
            rotate(nums, kk, nums.length - 1);
        }
    
        private void rotate(int[] arr, int left, int right) {
            var tmp = 0;
            var l = left;
            var r = right;
            while (l < r) {
                tmp = arr[l];
                arr[l] = arr[r];
                arr[r] = tmp;
                l++;
                r--;
            }
        }
    }
    ```

    ***思考***

    解题时，可以用直接思路，而不是直接去算，比如反转数组时，与其计算下标，不如直接用left、right两个指针去做。

-   [0048. 旋转图像 - 力扣](https://leetcode.cn/problems/rotate-image/)

    ```java
    package org.lc;
    
    /**
     * 创建时间: 2026/01/28 10:11
     * 作者: Ain
     * 链接：https://leetcode.cn/problems/rotate-image/description/
     * 题目：48. 旋转图像
     * 难度：medium
     * Thinking：_6_min
     * Coding:_10_min
     * 思路：计算(i,j)下标旋转后的位置
     */
    public class Lc0048 {
        public void rotate(int[][] matrix) {
            int tmp = 0;
            int n = matrix.length;
            for (int i = 0; i < n / 2; i++) {
                for (int j = i; j < n - 1 - i; j++) {
                    int i0 = i;
                    int j0 = j;
                    int i1 = j0;
                    int j1 = n - 1 - i0;
                    int i2 = j1;
                    int j2 = n - 1 - i1;
                    int i3 = j2;
                    int j3 = n - 1 - i2;
                    tmp = matrix[i0][j0];
                    matrix[i0][j0] = matrix[i3][j3];
                    matrix[i3][j3] = matrix[i2][j2];
                    matrix[i2][j2] = matrix[i1][j1];
                    matrix[i1][j1] = tmp;
                }
            }
        }
    }
    ```

    **思路 2：原地翻转**

    通过观察可以得出：原矩阵可以通过一次「水平翻转」+「主对角线翻转」得到旋转后的二维矩阵。

    这种思路没有想到


-   [0054. 螺旋矩阵 - 力扣](https://leetcode.cn/problems/spiral-matrix/)

    ```java
    package org.lc;
    
    import java.util.ArrayList;
    import java.util.List;
    
    /**
     * 创建时间: 2026/01/28 10:29
     * 作者: Ain
     * 链接：https://leetcode.cn/problems/spiral-matrix/description/
     * 题目：54. 螺旋矩阵
     * 难度：medium
     * Thinking：_5_min
     * Coding:_6_min
     * 思路：边界指针+状态机转换
     */
    public class Lc0054 {
        public List<Integer> spiralOrder(int[][] matrix) {
            int cntState = 0;
            int l = 0;
            int t = 0;
            int r = matrix[0].length - 1;
            int b = matrix.length - 1;
            List<Integer> res = new ArrayList<>();
            while (l <= r && t <= b) {
                switch (cntState) {
                    case 0: {
                        for (int j = l; j <= r; j++) {
                            res.add(matrix[t][j]);
                        }
                        t++;
                        break;
                    }
                    case 1: {
                        for (int i = t; i <= b; i++) {
                            res.add(matrix[i][r]);
                        }
                        r--;
                        break;
                    }
                    case 2: {
                        for (int j = r; j >= l; j--) {
                            res.add(matrix[b][j]);
                        }
                        b--;
                        break;
                    }
                    case 3: {
                        for (int i = b; i >= t; i--) {
                            res.add(matrix[i][l]);
                        }
                        l++;
                        break;
                    }
                }
                cntState = (cntState + 1) % 4;
            }
            return res;
        }
    }
    ```

-   [0498. 对角线遍历 - 力扣](https://leetcode.cn/problems/diagonal-traverse/)

    ```java
    package org.lc;
    
    /**
     * 创建时间: 2026/01/28 10:50
     * 作者: Ain
     * 链接：https://leetcode.cn/problems/diagonal-traverse/description/
     * 题目：498. 对角线遍历
     * 难度：medium
     * Thinking：_10_min
     * Coding:_20_min
     * 思路：两个状态，一个是向右上，一个向左下，注意边界条件即可。
     * 卡点：卡在了边界分析上。
     */
    public class Lc0498 {
        public int[] findDiagonalOrder(int[][] mat) {
            int m = mat.length;
            int n = mat[0].length;
            int[] ans = new int[m * n];
            boolean isRightUp = true;
            int loop = m + n - 2;
            int i = 0, j = 0, l = 0;
            for (int k = 0; k <= loop; k++) {
                if (isRightUp) {
                    while (i >= 0 && i <= m - 1 && j >= 0 && j <= n - 1) {
                        ans[l] = mat[i][j];
                        i--;
                        j++;
                        l++;
                    }
                    if (j > n - 1) {
                        j = n - 1;
                        i = i + 2;
                    } else {
                        i = 0;
                    }
                } else {
                    while (i >= 0 && i <= m - 1 && j >= 0 && j <= n - 1) {
                        ans[l] = mat[i][j];
                        l++;
                        i++;
                        j--;
                    }
                    if (i > m - 1) {
                        i = m - 1;
                        j = j + 2;
                    } else {
                        j = 0;
                    }
                }
                isRightUp = !isRightUp;
            }
            return ans;
        }
    }
    ```

-   [485. 最大连续 1 的个数](https://leetcode.cn/problems/max-consecutive-ones/)

    ```java
    package org.lc;
    
    /**
     * 创建时间: 2026/01/29 10:06
     * 作者: Ain
     * 链接：https://leetcode.cn/problems/max-consecutive-ones/description/
     * 题目：485. 最大连续 1 的个数
     * 难度：easy
     * Thinking：_1_min
     * Coding:_2_min
     * 思路：记录当前最值，和已知最值比较更新
     * 卡点：
     */
    public class Lc0485 {
        public int findMaxConsecutiveOnes(int[] nums) {
            int ans = 0;
            int tmp = -1;
            for (int i = 0; i < nums.length; i++) {
                if (nums[i] == 1) {
                    if (tmp == -1) {
                        tmp = 1;
                    } else {
                        tmp++;
                    }
                } else {
                    if (tmp > 0) {
                        ans = Math.max(ans, tmp);
                        tmp = -1;
                    }
                }
            }
            if (tmp > 0) {
                ans = Math.max(ans, tmp);
            }
            return ans;
        }
    }
    ```

-   [238. 除了自身以外数组的乘积](https://leetcode.cn/problems/product-of-array-except-self/)

    ```java
    package org.lc;
    
    /**
     * 创建时间: 2026/01/29 10:15
     * 作者: Ain
     * 链接：https://leetcode.cn/problems/product-of-array-except-self/
     * 题目：238. 除了自身以外数组的乘积
     * 难度：medium
     * Thinking：_∞_min
     * Coding:_4_min
     * 思路：左遍历一次，ans先存放前缀积，右遍历一次时把后缀积乘上去
     * 卡点：没想到反着遍历一次后缀和
     */
    public class Lc0238 {
        public int[] productExceptSelf(int[] nums) {
            int[] ans = new int[nums.length];
            ans[0] = 1;
            for (int i = 1; i < nums.length; i++) {
                ans[i] = ans[i - 1] * nums[i - 1];
            }
            int tmp = 1;
            for (int i = nums.length - 2; i >= 0; i--) {
                tmp = tmp * nums[i + 1];
                ans[i] = ans[i] * tmp;
            }
            return ans;
        }
    }
    ```

-   [73. 矩阵置零](https://leetcode.cn/problems/set-matrix-zeroes/)

    ```java
    package org.lc;
    
    /**
     * 创建时间: 2026/01/29 10:39
     * 作者: Ain
     * 链接：https://leetcode.cn/problems/set-matrix-zeroes/description/
     * 题目：73. 矩阵置零
     * 难度：medium
     * Thinking：_4_min
     * Coding:__min
     * 思路：用m和n数组分别记录值为0的坐标，然后根据坐标刷新数组。但是空间复杂度时O(m+n)，没有达到O(1)。
     * 卡点：没想到怎么用常数阶空间复杂度解题。
     */
    public class Lc0073 {
        public void setZeroes(int[][] matrix) {
            int m = matrix.length;
            int n = matrix[0].length;
            int[] x = new int[m];
            int[] y = new int[n];
    
            for (int i = 0; i < m; i++) {
                for (int j = 0; j < n; j++) {
                    if (matrix[i][j] == 0) {
                        x[i] = 1;
                        y[j] = 1;
                    }
                }
            }
    
            for (int i = 0; i < m; i++) {
                if (x[i] == 1) {
                    for (int j = 0; j < n; j++) {
                        matrix[i][j] = 0;
                    }
                }
            }
    
            for (int j = 0; j < n; j++) {
                if (y[j] == 1) {
                    for (int i = 0; i < m; i++) {
                        matrix[i][j] = 0;
                    }
                }
            }
        }
    }
    ```

    ***进阶思路***

    直观上可以使用两个数组来标记行和列出现 0 的情况，但这样空间复杂度就是 *O(m+n)* 了，不符合题意。

    考虑使用数组原本的元素进行记录出现 0 的情况。

    1.  设定两个变量 ***flag_row0***、***flag_col0*** 来标记第一行、第一列是否出现了 0。
    2.  接下来我们使用数组第一行、第一列来标记 0 的情况。
    3.  对数组除第一行、第一列之外的每个元素进行遍历，如果某个元素出现 0 了，则使用数组的第一行、第一列对应位置来存储 0 的标记。
    4.  再对数组除第一行、第一列之外的每个元素进行遍历，通过对第一行、第一列的标记 0 情况，进行置为 0 的操作。
    5.  最后再根据 ***flag_row0***、***flag_col0***的标记情况，对第一行、第一列进行置为 0 的操作。

    ***总结***：<u>说实话没想到可以直接在原数组上操作。</u>

-   [59. 螺旋矩阵 II](https://leetcode.cn/problems/spiral-matrix-ii/)

    ```java
    package org.lc;
    
    /**
     * 创建时间: 2026/01/29 11:02
     * 作者: Ain
     * 链接：https://leetcode.cn/problems/spiral-matrix-ii/description/
     * 题目：59. 螺旋矩阵 II
     * 难度：medium
     * Thinking：_6_min
     * Coding:_6_min
     * 思路：思路同[54. 螺旋矩阵]，边界指针 + 状态机
     * 卡点：
     */
    public class Lc0059 {
        public int[][] generateMatrix(int n) {
            int[][] ans = new int[n][n];
            int t = 0, l = 0, r = n - 1, b = n - 1;
            int k = 1;
            int cntStat = 0;
            while (t <= b && l <= r) {
                switch (cntStat) {
                    case 0: {
                        for (int j = l; j <= r; j++) {
                            ans[t][j] = k;
                            k++;
                        }
                        t++;
                        break;
                    }
                    case 1: {
                        for (int i = t; i <= b; i++) {
                            ans[i][r] = k;
                            k++;
                        }
                        r--;
                        break;
                    }
                    case 2: {
                        for (int j = r; j >= l; j--) {
                            ans[b][j] = k;
                            k++;
                        }
                        b--;
                        break;
                    }
                    case 3: {
                        for (int i = b; i >= t; i--) {
                            ans[i][l] = k;
                            k++;
                        }
                        l++;
                        break;
                    }
                }
                cntStat = (cntStat + 1) % 4;
            }
            return ans;
        }
    }
    ```

-   [289. 生命游戏](https://leetcode.cn/problems/game-of-life/)

    ```java
    package org.lc;
    
    /**
     * 创建时间: 2026/01/29 11:35
     * 作者: Ain
     * 链接：https://leetcode.cn/problems/game-of-life/description/
     * 题目：289. 生命游戏
     * 难度：medium
     * Thinking：_5_min
     * Coding:_7_min
     * 思路：使用O(m*n)空间复杂度，很容易解决；
     * 进阶思路是将原数组两次刷新，因需要记录原状态和新状态，故更新值。
     * 第一次刷新：  2表示原值是0，且不需要更新成1；4表示原值是0，且需要更新成1；
     * 3表示原值是1，且不需要更新成0；5表示原值是1，且需要更新成0。
     * 第二次刷新：  将元素2和5，更新成0；3和4更新成1。
     * 卡点：
     */
    public class Lc0289 {
        public void gameOfLife(int[][] board) {
            int m = board.length;
            int n = board[0].length;
            for (int i = 0; i < m; i++) {
                for (int j = 0; j < n; j++) {
                    board[i][j] = calculate(board, i, j, m, n);
                }
            }
    
            for (int i = 0; i < m; i++) {
                for (int j = 0; j < n; j++) {
                    switch (board[i][j]) {
                        case 2:
                        case 5: {
                            board[i][j] = 0;
                            break;
                        }
                        case 3:
                        case 4: {
                            board[i][j] = 1;
                            break;
                        }
                    }
                }
            }
        }
    
        private int calculate(int[][] board, final int i, final int j, final int m, final int n) {
            int ans = 0;
            if (i - 1 >= 0 && j - 1 >= 0 && board[i - 1][j - 1] % 2 == 1) ans++;
            if (i - 1 >= 0 && board[i - 1][j] % 2 == 1) ans++;
            if (i - 1 >= 0 && j + 1 < n && board[i - 1][j + 1] % 2 == 1) ans++;
    
            if (j - 1 >= 0 && board[i][j - 1] % 2 == 1) ans++;
            if (j + 1 < n && board[i][j + 1] % 2 == 1) ans++;
    
            if (i + 1 < m && j - 1 >= 0 && board[i + 1][j - 1] % 2 == 1) ans++;
            if (i + 1 < m && board[i + 1][j] % 2 == 1) ans++;
            if (i + 1 < m && j + 1 < n && board[i + 1][j + 1] % 2 == 1) ans++;
    
            if (ans < 2) {
                if (board[i][j] == 0) return 2;
                else return 5;
            } else if (ans > 3) {
                if (board[i][j] == 0) return 2;
                else return 5;
            } else if (ans == 3) {
                if (board[i][j] == 0) return 4;
                else return 3;
            } else {
                if (board[i][j] == 0) return 2;
                else return 3;
            }
        }
    }
    ```

## 数组排序

### 冒泡排序

```java
    /**
     * 冒泡排序
     */
    public void bubbleSort(int[] nums) {
        for (int i = 0; i < nums.length; i++) {
            boolean tmp = false;
            for (int j = 0; j < nums.length - i - 1; j++) {
                if (nums[j] > nums[j + 1]) {
                    int t = nums[j];
                    nums[j] = nums[j + 1];
                    nums[j + 1] = t;
                    tmp = true;
                }
            }
            if (!tmp) break;
        }
    }
```

### 选择排序

```java
    /**
     * 选择排序
     */
    public void selectionSort(int[] nums) {
        for (int i = 0; i < nums.length; i++) {
            int cntMin = nums[i];
            int cntIndex = i;
            for (int j = i; j < nums.length; j++) {
                if (cntMin > nums[j]) {
                    cntMin = nums[j];
                    cntIndex = j;
                }
            }
            var tmp = nums[i];
            nums[i] = nums[cntIndex];
            nums[cntIndex] = tmp;
        }
    }
```

### 插入排序

```java
    /**
     * 插入排序
     */
    public void insertionSort(int[] nums) {
        for (int i = 1; i < nums.length; i++) {
            for (int j = i; j > 0; j--) {
                if (nums[j] < nums[j - 1]) {
                    var tmp = nums[j];
                    nums[j] = nums[j - 1];
                    nums[j - 1] = tmp;
                }
            }
        }
    }
```

### 希尔排序

```java
    /**
     * 希尔排序
     */
    public void shellSort(int[] nums) {
        int gap = nums.length / 2;
        while (gap > 0) {
            for (int i = gap; i < nums.length; i++) {
                for (int j = i; j > 0; j -= gap) {
                    if (j - gap >= 0 && nums[j] < nums[j - gap]) {
                        var tmp = nums[j];
                        nums[j] = nums[j - gap];
                        nums[j - gap] = tmp;
                    }
                }
            }
            gap = gap / 2;
        }
    }
```

### 归并排序

``` java
    /**
     * 归并排序
     */
    public int[] mergeSort(int[] nums, int l, int r) {
        if (l >= r) return new int[]{nums[l]};
        int mid = (l + r) / 2;
        var left = mergeSort(nums, l, mid);
        var right = mergeSort(nums, mid + 1, r);
        return merge(left, right);
    }

    private int[] merge(int[] left, int[] right) {
        var lSize = left.length;
        var rSize = right.length;
        var ans = new int[lSize + rSize];
        var l = 0;
        var r = 0;
        var i = 0;
        while (l < lSize && r < rSize) {
            if (left[l] <= right[r]) {
                ans[i] = left[l];
                i++;
                l++;
            } else {
                ans[i] = right[r];
                i++;
                r++;
            }
        }

        if (l >= lSize) {
            while (r < rSize) {
                ans[i] = right[r];
                i++;
                r++;
            }
        } else {
            while (l < lSize) {
                ans[i] = left[l];
                i++;
                l++;
            }
        }
        return ans;
    }
```

### 快速排序

```java
    /**
     * 快速排序
     */
    public void quickSort(int[] nums, int l, int r) {
        if (l >= r) return;
        var p = partition(nums, l, r);
        quickSort(nums, l, p - 1);
        quickSort(nums, p + 1, r);
    }

    private int partition(int[] nums, int l, int r) {
        var key = nums[l];
        while (l < r) {
            while (key <= nums[r] && l < r) r--;
            nums[l] = nums[r];
            while (key >= nums[l] && l < r) l++;
            nums[r] = nums[l];
        }
        nums[l] = key;
        return l;
    }
```

### 堆排序

```java
    /**
     * 堆排序
     */
    public void maxHeapSort(int[] nums, int l, int r) {
        buildMaxHeap(nums, l, r);
        for (int i = 0; i <= r; i++) {
            var tmp = nums[0];
            nums[0] = nums[r - i];
            nums[r - i] = tmp;
            shiftMaxHeap(nums, l, r - i - 1);
        }
    }

    public void buildMaxHeap(int[] nums, int l, int r) {
        int target = (l + r) / 2;
        for (; target >= 0; target--) {
            shiftMaxHeap(nums, target, r);
        }
    }

    public void shiftMaxHeap(int[] nums, int l, int r) {
        var i = l;
        while (i < r) {
            var k = 2 * (i + 1) - 1;
            if (k + 1 <= r && nums[k] < nums[k + 1]) k = k + 1;
            if (k <= r && nums[i] < nums[k]) {
                var tmp = nums[i];
                nums[i] = nums[k];
                nums[k] = tmp;
                i = k;
            } else {
                break;
            }
        }
    }
```

### 计数排序

```java
    /**
     * 计数排序
     **/
    public void countingSort(int[] nums) {
        var min = nums[0];
        var max = nums[0];
        for (int i = 0; i < nums.length; i++) {
            min = Math.min(min, nums[i]);
            max = Math.max(max, nums[i]);
        }
        var range = max - min;
        int[] count = new int[range + 1];
        for (int num : nums) {
            count[num - min] += 1;
        }
        int k = 0;
        for (int i = 0; i < count.length; i++) {
            while (count[i] > 0) {
                nums[k] = i + min;
                k++;
                count[i]--;
            }
        }
    }
```

### 桶排序

```java
```

























-   
-   
-   
-   
-   

