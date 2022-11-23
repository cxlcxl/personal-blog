---
title: "进制转换"
date: 2022-11-23T14:31:06+08:00
draft: false
description: "进制转换"
categories: ["进制转换"]
tags: ["进制转换", "二进制", "十进制", "八进制"]
ShowToc: true
ShowBreadCrumbs: '' #原创文章，如需转载请注明文章作者和出处。谢谢！
---
&emsp;&emsp;世界上有 10 种人，懂二进制和不懂二进制的，实际上二进制只有 0 和 1，逢 2 进 1，所以 10 在二进制中并不是十进制的 10 而是十进制的 2。

&emsp;&emsp;进制就是逢几进一，n 进制就是逢 n 进一。计算机只能识别二进制，人类最习惯使用的是十进制，而为了实际需要，又建立了八进制和十六进制。八进制就是逢八进一，十六进制就是逢十六进一。

### 二进制转十进制

例二进制数：`10010110`，可转换如下
```
  1       0       0       1       0       1       1       0         // 二进制
1*2^7 + 0*2^6 + 0*2^5 + 1*2^4 + 0*2^3 + 1*2^2 + 1*2^1 + 0*2^0
128   + 0     + 0     + 16    + 0     + 4     + 2     + 0     = 150 // 十进制
```

----
### 八进制转十进制
八进制：只有 0~7，逢 8 进 1

例八进制数：`256320`，可转换如下
```
  2       5       6       3       2       0           // 八进制
2*8^5 + 5*8^4 + 6*8^3 + 3*8^2 + 2*8^1 + 0*8^0
65536 + 20480 + 3072  + 192   + 16    + 0     = 89296 // 十进制
```

----
### 十六进制转十进制
十六进制稍微特殊，但是转换与上面两种相同：只有 0~9,a,b,c,d,e,f，逢 16 进 1

例十六进制数：`30af`，可转换如下
```
  3        0         a         f            // 十六进制
3*16^3 + 0*16^2 + 10*16^1 + 15*16^0 
12288  + 0      + 160     + 15      = 12463 // 十进制
```

> 转十进制总结：都是 `位上的数字 * 进制 ^ 权重`


----
### 十进制转二进制

例十进制数：`562`，可如下做除非换算

```txt
// 562 为被除数，除数为 2，知道商为 0 停止，余数反向从下往上相连既是转换结果：
2 | 562
  -----
 2 | 281   ---------- 0
    -----
  2 | 140   --------- 1
    -----
   2 | 70    -------- 0
     -----
    2 | 35    ------- 0
      -----
     2 | 17    ------ 1
       -----
      2 | 8     ----- 1
        -----
       2 | 4     ---- 0
         -----
        2 | 2     --- 0
          -----
         2 | 1     -- 0
           -----
            0      -- 1
// 结果：1000110010
```

> 十进制转八进制，十进制转十六进制同转二进制一个方式，而非十进制间的转换就可以通过十进制中转即可。