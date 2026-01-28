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

## 数组基础

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

-   

-   

-   

