---
description: 慎用 append
---

# 使用slice 作为参数进行传递的坑

Slice, map,channel 本身都是指针类型，所以使用这三个作为参数时要考虑要修改本身的值。

这里有一个特殊的：Slice

在使用 slice 作为参数传递时，如果在函数中进行了原切片的修改操作，在原函数中时能体现出来，但是append 操作未能体现。比如以下代码：

```go
func TestSlice(t *testing.T)  {
	// 传递slice
	slice1 := make([]string,3)
	slice1[0]="one"
	slice1[1]="two"
	hello(slice1)
	t.Log(slice1)
}

func hello(slices []string)  {
	slices[1]="three"
	slices = append(slices,"four")
}
```

执行该测试函数，得到下面的结果:

```shell
=== RUN   TestSlice
    pointer_test.go:30: [one three ]
--- PASS: TestSlice (0.00s)
```

这里我们可以得到以下几点结论：

1. 切片在 make 的时候，会使用数据类型的零值来占位，比如 make(\[]string,3) 这个时候的切片的值就是三个空字符串。
2. 在使用切片作为参数时，函数内的修改会影响切片本身。 比如代码中修改 slices\[1]="three" 说明切片本身是一个指针类型。
3. 在使用切片作为参数时，函数内的append 操作没有在原始切片中体现。

探索一下结论三出现的原因：

先看一下切片的定义:

```go
type slice struct {
    array unsafe.Pointer
    len   int
    cap   int
}
```

* `array`：一个指针，指向底层存储数据的数组
* `len`：切片的长度，在代码中我们可以使用`len()`函数获取这个值
* `cap`：切片的容量，**即在不扩容的情况下，最多能容纳多少元素**。在代码中我们可以使用`cap()`函数获取这个值

可以看到，切片的 array 指向一个底层数组，len 和 cap 都是int类型而不是指针，也就是说，无论函数内如何修改，原始切片的指针地址，len 和 cap 的值是不会发生变化的。 所以要慎用 append ，最好的方式就是return 后重新赋值。

```go
func TestSlice(t *testing.T)  {
	// 传递slice
	slice1 := make([]string,3)
	slice1[0]="one"
	slice1[1]="two"
	slice1 = hello(slice1)
	t.Log(slice1)
}

func hello(slices []string) []string {
	slices[1]="three"
	slices = append(slices,"four")
	return slices
}
```

#### 总结：

1. 如果函数内部没有更新操作，直接传递即可。
2. 如果有更新操作，并且不希望破坏原来的值，使用 copy() 后进行参数传递。

```go
slice2 := make([]string,3)
copy(slice2,slice1)
```

3\. 如果有更新操作，并且希望得到更新后的值，return 后重新赋值。
