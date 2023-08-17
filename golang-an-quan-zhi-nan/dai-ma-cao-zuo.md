---
description: 开发安全指南
---

# 代码操作



### 1.  切片必须校验长度

> 在对slice进行操作时，必须判断长度是否合法，防止程序panic

```go
func Test_arr(t *testing.T) {    //未校验，程序可能会 panic   out of range
    var arr = []int{1,2,3,4,5}
    t.Log(arr[:6])   //左闭右开
}
```

应改为：

```go
func Test_arr(t *testing.T) {  
    var arr = []int{1,2,3,4,5}
    if len(arr) >= 6{
        t.Log(arr[:6])   //左闭右开
    } 
}
```

### 2. nil 指针判断

> 进行指针操作时，必须判断该指针是否为nil，防止程序panic

```go
type Packet struct {
    PackeyType
    PackeyVersion uint8
    Data
}

type Data struct {
    Stat uint8
    Len uint8
    Buf [8]byte
}
...
unmarshal 之后，直接调用 Packet.Data.Len  ，其实这个时候的 Data 有可能是 nil, 调用就会 painc
```

### 整数安全

> 在进行数字运算操作时，需要做好长度限制，防止外部输入运算导致异常：
>
> 1. &#x20;确保无符号整数运算时不会反转&#x20;
> 2. 确保有符号整数运算时不会出现溢出&#x20;
> 3. 确保整型转换时不会出现截断错误 （比如 int64 转 int32）
> 4. 确保整型转换时不会出现符号错误&#x20;
>
> 以下场景必须严格进行长度限制：&#x20;
>
> 1. 作为数组索引&#x20;
> 2. 作为对象的长度或者大小&#x20;
> 3. 作为数组的边界（如作为循环计数器）

```go
//bad
func Test_arr(t *testing.T) {
    var a uint8
    a = 254
    t.Log(a+2)   // 这里会输出 0   无符号整型会反转
    
    var a1 int64
    a1 = 12654896598
    t.Log(int16(a1))    // 数据截断， 产生错误数据
}

```

[Go 开发安全指南](https://github.com/Tencent/secguide/blob/main/Go%E5%AE%89%E5%85%A8%E6%8C%87%E5%8D%97.md)
