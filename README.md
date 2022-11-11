# leetcode 每日一题

## 1704. 判断字符串的两半是否相似
给你一个偶数长度的字符串 s 。将其拆分成长度相同的两半，前一半为 a ，后一半为 b 。

两个字符串 相似 的前提是它们都含有相同数目的元音（'a'，'e'，'i'，'o'，'u'，'A'，'E'，'I'，'O'，'U'）。注意，s 可能同时含有大写和小写字母。

如果 a 和 b 相似，返回 true ；否则，返回 false 。
```
// includes
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
// indexOf
var halvesAreAlike = function(s) {
    const vowelArray = ['a', 'e', 'i', 'o', 'u'];
    const length = s.length;
    const a = s.slice(0, length / 2);
    const b = s.slice(length / 2, length);
    let aLength = 0;
    let bLength = 0;
    for (let i = 0; i < a.length; i++) {
        if (vowelArray.indexOf(a[i].toLowerCase()) >= 0) {
            ++aLength;
        }
    }
    for (let i = 0; i < b.length; i++) {
        if (vowelArray.indexOf(b[i].toLowerCase()) >= 0) {
            ++bLength;
        }
    }
    return aLength === bLength;
};
```

