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
