---
description: float 数据存储 精度损失
---

# 禁止使用 float 判断相等

几个很有意思的题目:

```go
var f1 float32 = 16777216.0
var f2 float32 = 16777217.0
fmt.Println(f1 == f2) //true
```

```go
var f1 float64 = 0.1
var f2 float64 = 0.2
fmt.Println(f1+f2 == 0.3) //false
```

```go
var f1 float32 = 0.1
var f2 float32 = 0.2
fmt.Println(f1+f2 == 0.3) //true
```

可以看到结果总是不是和预期的相符。

因为float 类型在存储的时候，小数部分有可能存在精度损失，导致结果不符合预期，所以应该禁止使用float 来判断相等。

一个简单的例子，在存储钱币的数值时，我们一般用分作为基本单位，而不是元。因为如果用元，会存在 2.12 元这种带有小数的值，在计算的时候就可能会出现误差。
