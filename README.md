# leetcode 每日一题

## 1704. 判断字符串的两半是否相似（中）
### 问题
给你一个偶数长度的字符串 s 。将其拆分成长度相同的两半，前一半为 a ，后一半为 b 。

两个字符串 相似 的前提是它们都含有相同数目的元音（'a'，'e'，'i'，'o'，'u'，'A'，'E'，'I'，'O'，'U'）。注意，s 可能同时含有大写和小写字母。

如果 a 和 b 相似，返回 true ；否则，返回 false 。
### 解答
```
// 自解（推荐）
var halvesAreAlike = function(s) {
    const vowelArray = ['a', 'e', 'i', 'o', 'u'];
    const length = s.length;
    const a = s.slice(0, length / 2);
    const b = s.slice(length / 2, length);
    let aLength = 0;
    let bLength = 0;
    a.toLowerCase().split('').forEach((item) => {
        if (vowelArray.includes(item)) ++aLength;
    });
    b.toLowerCase().split('').forEach((item) => {
        if (vowelArray.includes(item)) ++bLength;
    });
    return aLength === bLength;
};
```
```
// 官方
var halvesAreAlike = function(s) {
    const vowelArray = ['a', 'e', 'i', 'o', 'u', 'A', 'E', 'I', 'O', 'U'];
    const length = s.length;
    const a = s.slice(0, length / 2);
    const b = s.slice(length / 2, length);
    let aLength = 0;
    let bLength = 0;
    for (let i = 0; i < a.length; i++) {
        if (vowelArray.indexOf(a[i]) >= 0) {
            ++aLength;
        }
    }
    for (let i = 0; i < b.length; i++) {
        if (vowelArray.indexOf(b[i]) >= 0) {
            ++bLength;
        }
    }
    return aLength === bLength;
};
```
## 791. 自定义字符串排序（中）
### 问题
给定两个字符串 order 和 s 。order 的所有字母都是 唯一 的，并且以前按照一些自定义的顺序排序。

对 s 的字符进行置换，使其与排序的 order 相匹配。更具体地说，如果在 order 中的字符 x 出现字符 y 之前，那么在排列后的字符串中， x 也应该出现在 y 之前。

返回 满足这个性质的 s 的任意一种排列。
### 解答
```
// 自解（推荐）
var customSortString = function(order, s) {
    let sToArray = s.split('');
    let orderArray = [];
    for (let i = 0; i < order.length; i++) {
        orderArray = orderArray.concat(
            sToArray.reduce((prev, cur) => {
                if (cur === order[i]) {
                    prev.push(order[i]);
                }
                return prev;
            }, [])
        );
        // 将匹配元素从 sToArray 中删除
        sToArray = sToArray.filter((item) => item !== order[i]);
    }
    return orderArray.concat(sToArray).join('');
};
```
```
// 官方
var customSortString = function(order, s) {
    const val = new Array(26).fill(0);
    for (let i = 0; i < order.length; ++i) {
        // charCodeAt() 方法返回 0 到 65535 之间的整数，表示给定索引处的 UTF-16 代码单元
        // a~z：77～122；A~Z：65~90
        val[order[i].charCodeAt() - 'a'.charCodeAt()] = i + 1;
    }
    const arr = new Array(s.length).fill(0).map((_, i) => s[i]);
    arr.sort((c0, c1) => val[c0.charCodeAt() - 'a'.charCodeAt()] - val[c1.charCodeAt() - 'a'.charCodeAt()])
    let ans = '';
    for (let i = 0; i < s.length; ++i) {
        ans += arr[i];
    }
    return ans;
};
```
## 805. 数组的均值分割（难）
### 问题
给定你一个整数数组 nums

我们要将 nums 数组中的每个元素移动到 A 数组 或者 B 数组中，使得 A 数组和 B 数组不为空，并且 average(A) == average(B) 。

如果可以完成则返回true ， 否则返回 false  。

注意：对于数组 arr ,  average(arr) 是 arr 的所有元素的和除以 arr 长度。

提示:

1 <= nums.length <= 30
0 <= nums[i] <= 104

### 思路
- 折半搜索
![image](https://user-images.githubusercontent.com/42236890/201651039-0c3bc914-790d-47cc-8210-e3d11379a892.png)
- 动态规划
![image](https://user-images.githubusercontent.com/42236890/201653722-5c4dad28-a4ba-44a1-ad93-f96d31dc3331.png)
### 解答
```
// 官方：折半搜索
var splitArraySameAverage = function(nums) {
    if (nums.length === 1) {
        return false;
    }
    const n = nums.length, m = Math.floor(n / 2);
    let sum = 0;
    for (const num of nums) {
        sum += num;
    }
    for (let i = 0; i < n; i++) {
        nums[i] = nums[i] * n - sum;
    }

    const left = new Set();
    for (let i = 1; i < (1 << m); i++) {
        let tot = 0;
        for (let j = 0; j < m; j++) {
            if ((i & (1 << j)) !== 0) {
                tot += nums[j];
            }
        }
        if (tot === 0) {
            return true;
        }
        left.add(tot);
    }
    let rsum = 0;
    for (let i = m; i < n; i++) {
        rsum += nums[i];
    }
    for (let i = 1; i < (1 << (n - m)); i++) {
        let tot = 0;
        for (let j = m; j < n; j++) {
            if ((i & (1 << (j - m))) != 0) {
                tot += nums[j];
            }
        }
        if (tot === 0 || (rsum !== tot && left.has(-tot))) {
            return true;
        }
    }
    return false;
};
```
```
// 官方：动态规划（推荐）
var splitArraySameAverage = function(nums) {
    if (nums.length === 1) {
        return false;
    }
    const n = nums.length, m = Math.floor(n / 2);
    let sum = 0;
    for (const num of nums) {
        sum += num;
    }
    let isPossible = false;
    for (let i = 1; i <= m; i++) {
        if (sum * i % n === 0) {
            isPossible = true;
            break;
        }
    }  
    if (!isPossible) {
        return false;
    }
    const dp = new Array(m + 1).fill(0).map(() => new Set());
    dp[0].add(0);
    for (const num of nums) {
        for (let i = m; i >= 1; i--) {
            for (const x of dp[i - 1]) {
                let curr = x + num;
                if (curr * n === sum * i) {
                    return true;
                }
                dp[i].add(curr);
            } 
        }
    }
    return false;
};
```
## 1710. 卡车上的最大单元数（易）
### 问题
请你将一些箱子装在 一辆卡车 上。给你一个二维数组 boxTypes ，其中 boxTypes[i] = [numberOfBoxesi, numberOfUnitsPerBoxi] ：

numberOfBoxesi 是类型 i 的箱子的数量。
numberOfUnitsPerBoxi 是类型 i 每个箱子可以装载的单元数量。
整数 truckSize 表示卡车上可以装载 箱子 的 最大数量 。只要箱子数量不超过 truckSize ，你就可以选择任意箱子装到卡车上。

返回卡车可以装载 单元 的 最大 总数。
### 示例
输入：boxTypes = [[1,3],[2,2],[3,1]], truckSize = 4

输出：8

解释：箱子的情况如下：
- 1 个第一类的箱子，里面含 3 个单元。
- 2 个第二类的箱子，每个里面含 2 个单元。
- 3 个第三类的箱子，每个里面含 1 个单元。

可以选择第一类和第二类的所有箱子，以及第三类的一个箱子。

单元总数 = (1 * 3) + (2 * 2) + (1 * 1) = 8
### 解答
```
// 自解
var maximumUnits = function(boxTypes, truckSize) {
    let size = truckSize;
    boxTypes.sort((a, b) => b[1] - a[1]);
    let maxCount = 0;
    for(let i = 0; i < boxTypes.length; i++) {
        if (boxTypes[i][0] >= size && size > 0) {
            maxCount += boxTypes[i][1] * size;
            size = 0;
        } else if (size > 0) {
            size = size - boxTypes[i][0];
            maxCount += boxTypes[i][1] * boxTypes[i][0];
        }
    }
    return maxCount;
};
```
```
// 官方
var maximumUnits = function(boxTypes, truckSize) {
    boxTypes.sort((a, b) => b[1] - a[1]);
    let res = 0;
    for (const boxType of boxTypes) {
        let numberOfBoxes = boxType[0];
        let numberOfUnitsPerBox = boxType[1];
        if (numberOfBoxes < truckSize) {
            res += numberOfBoxes * numberOfUnitsPerBox;
            truckSize -= numberOfBoxes;
        } else {
            res += truckSize * numberOfUnitsPerBox;
            break;
        }
    }
    return res;
};
```
## 1662. 检查两个字符串数组是否相等（易）
### 问题
给你两个字符串数组 word1 和 word2 。如果两个数组表示的字符串相同，返回 true ；否则，返回 false 。

数组表示的字符串 是由数组中的所有元素 按顺序 连接形成的字符串。
### 解答
```
var arrayStringsAreEqual = function(word1, word2) {
    return word1.join('') === word2.join('');
};
```
## 1620. 网络信号最好的坐标
### 问题
给你一个数组 towers 和一个整数 radius 。

数组  towers  中包含一些网络信号塔，其中 towers[i] = [xi, yi, qi] 表示第 i 个网络信号塔的坐标是 (xi, yi) 且信号强度参数为 qi 。所有坐标都是在  X-Y 坐标系内的 整数 坐标。两个坐标之间的距离用 欧几里得距离 计算。

整数 radius 表示一个塔 能到达 的 最远距离 。如果一个坐标跟塔的距离在 radius 以内，那么该塔的信号可以到达该坐标。在这个范围以外信号会很微弱，所以 radius 以外的距离该塔是 不能到达的 。

如果第 i 个塔能到达 (x, y) ，那么该塔在此处的信号为 ⌊qi / (1 + d)⌋ ，其中 d 是塔跟此坐标的距离。一个坐标的 信号强度 是所有 能到达 该坐标的塔的信号强度之和。

请你返回数组 [cx, cy] ，表示 信号强度 最大的 整数 坐标点 (cx, cy) 。如果有多个坐标网络信号一样大，请你返回字典序最小的 非负 坐标。
### 注意
- 坐标 (x1, y1) 字典序比另一个坐标 (x2, y2) 小，需满足以下条件之一：
  - 要么 x1 < x2 ，
  - 要么 x1 == x2 且 y1 < y2 。
- ⌊val⌋ 表示小于等于 val 的最大整数（向下取整函数）。
### 示例
示例1  
输入：towers = [[1,2,5],[2,1,7],[3,1,9]], radius = 2  
输出：[2,1]  
解释：  
坐标 (2, 1) 信号强度之和为 13，三个塔的信号都可以达到该坐标处的塔（距离都小于2）
- 塔 (2, 1) 强度参数为 7 ，在该点强度为 ⌊7 / (1 + sqrt((2-2)^2+(1-1)^2⌋ = ⌊7⌋ = 7（其实不用计算，距离自身的强度直接取其qi值即可）
- 塔 (1, 2) 强度参数为 5 ，在该点强度为 ⌊5 / (1 + sqrt((2-1)^2+(1-2)^2)⌋ = ⌊2.07⌋ = 2
- 塔 (3, 1) 强度参数为 9 ，在该点强度为 ⌊9 / (1 + sqrt((2-3)^+(1-1)^2)⌋ = ⌊4.5⌋ = 4  
没有别的坐标有更大的信号强度。

示例2  
输入：towers = [[23,11,21]], radius = 9  
输出：[23,11]  
解释：由于仅存在一座信号塔，所以塔的位置信号强度最大。 

示例3  
输入：towers = [[1,2,13],[2,1,7],[0,1,9]], radius = 2  
输出：[1,2]  
解释：坐标 (1, 2) 的信号强度最大。
### 提示
- 1 <= towers.length <= 50
- towers[i].length == 3
- 0 <= xi, yi, qi <= 50
- 1 <= radius <= 50
### 解答

## 775. 全局倒置与局部倒置（中）
### 问题
给你一个长度为 n 的整数数组 nums ，表示由范围 [0, n - 1] 内所有整数组成的一个排列。

全局倒置 的数目等于满足下述条件不同下标对 (i, j) 的数目：
- 0 <= i < j < n
- nums[i] > nums[j]

局部倒置 的数目等于满足下述条件的下标 i 的数目：
- 0 <= i < n - 1
- nums[i] > nums[i + 1]

当数组 nums 中 全局倒置 的数量等于 局部倒置 的数量时，返回 true ；否则，返回 false 。
### 示例
示例1  
输入：nums = [1,0,2]  
输出：true  
解释：有 1 个全局倒置，和 1 个局部倒置。

示例2  
输入：nums = [1,2,0]  
输出：false  
解释：有 2 个全局倒置，和 1 个局部倒置。
### 提示
- n == nums.length
- 1 <= n <= 105
- 0 <= nums[i] < n
- nums 中的所有整数 互不相同
- nums 是范围 [0, n - 1] 内所有数字组成的一个排列
### 解答
```
// 自解（超出时间限制）
var isIdealPermutation = function(nums) {
    let partInvertCount = 0;
    let allInvertCount = 0;
    for (let i = 0; i < nums.length - 1; i++) {
        if (nums[i] > nums[i+1]) ++partInvertCount;
        for (let j = i + 1; j < nums.length; j++) {
            if (nums[i] > nums[j]) ++allInvertCount;
        }
    }
    return partInvertCount === allInvertCount;
};
```
