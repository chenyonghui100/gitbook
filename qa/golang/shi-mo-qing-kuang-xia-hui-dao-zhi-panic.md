# 什么情况下会导致 panic

1. 数组下标越界。（数组和切片下标不能超过 len 的值。即使 slice 没有超过 cap 也不行）
2. 类型断言失败。如下：

```go
var a interface{}
a = "sss"
j := a.(int)  //断言 a 是int类型
```

3\. 空指针异常 （切片，map 没有 make 直接使用等）

4\. 往关闭的 channel 进行写数据。

<table><thead><tr><th width="150">channel</th><th width="157">未初始化</th><th width="150">buffer=0</th><th width="150">close【buffer=0】</th><th>close【buffer ！= 0】</th></tr></thead><tbody><tr><td>写数据</td><td><p>阻塞</p><p>(睡眠-死锁状态)</p></td><td>阻塞</td><td>阻塞</td><td>panic: send on closed channel</td></tr><tr><td>读数据</td><td><p>阻塞</p><p>(睡眠-死锁状态)</p></td><td>阻塞</td><td>阻塞</td><td>先读取buffer，直到为空，再读会返回零值</td></tr></tbody></table>

5\. 程序错误。（死锁【channel 只发不收】，死循环）\
