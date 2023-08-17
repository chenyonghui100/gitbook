---
description: 函数中的参数传递
---

# 指针作为函数的参数传递

#### 一道很有意思的题目

```go
package main

import "fmt"

func main() {
	s := "我是一串字符串"
	fmt.Printf("main 内存地址：%p\n", &s)
	hello(&s)
}

func hello(s *string) {
	fmt.Printf("hello 内存地址：%p\n", &s)
}
```

输出打印结果【每次输出结果是不一样的】：

```shell
$ go run main.go
main 内存地址：0xc000010240
hello 内存地址：0xc00000e030
```

这里可以看到，输出的内存地址是不一样的。

在go 函数中，所有的参数传递都是值传递。

也就是说，我们在以 \&s 作为参数的时候，其实发生了两个步骤：

1. 产生了一个指针类型的变量，这个变量的值是 \&s
2. 将这个变量作为函数 hello 的参数进行传递。

所以： 我们在hello 中如果打印 s 的值，就会得到main函数中相同的打印结果。
