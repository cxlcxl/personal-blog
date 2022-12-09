---
title: "Fibonacci 斐波那契数列"
date: 2022-11-30T13:52:28+08:00
draft: false
description: "斐波那契数列"
categories: ["algorithm"]
showToc: false
---

&emsp;&emsp;斐波那契数（意大利语：Successione di Fibonacci），又译为菲波拿契数、菲波那西数、斐氏数、黄金分割数。所形成的数列称为斐波那契数列（意大利语：Successione di Fibonacci），又译为菲波拿契数列、菲波那西数列、斐氏数列、黄金分割数列。

在数学上，斐波那契数是以递归的方法来定义：
  - F<sub>0</sub> = 0
  - F<sub>1</sub> = 1
  - F<sub>n</sub> = F<sub>n-1</sub> + F<sub>n-2</sub>

&emsp;&emsp;用文字来说，就是斐波那契数列由0和1开始，之后的斐波那契数就是由之前的两数相加而得出。

&emsp;&emsp;1、 1、 2、 3、 5、 8、 13、 21、 34、 55、 89、 144、 233、 377、 610、 987......
特别指出：0不是第一项，而是第零项。

```go
func fibonacci(n int) int {
  if n < 3 {
    return 1
  }
  return fibonacci(n-1) + fibonacci(n-2)
}
# fibonacci(5)
```