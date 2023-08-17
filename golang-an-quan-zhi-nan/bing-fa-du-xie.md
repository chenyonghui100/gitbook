---
description: golang 中的并发读写
---

# 并发读写

go 语言的优势就在于并发， 那么如果对于 某一个变量进行并发读写会有什么后果？

### 基本数据类型

不会报错，对于 int,string,bool 等这些基本类型， 如果是值追加可以，覆盖可能会导致结果不受控。

### 数组类型

不会报错，在不超过数组长度的情况下，对数组并发读写不会报错。

### 切片类型

不会报错，但是如果使用 append 的时候， 会导致结果不符合预期。

```go
func TestConSlice(t *testing.T){
	testArr := make([]int64,30)
	var i int64
	for i =1;i<3000;i++ {
		i := i
		go func() {
			testArr = append(testArr,i)
		}()
	}
	fmt.Println(len(testArr))   // 输出结果小于 3000
}
```

### map 类型

Golang默认map是不支持并发写入操作。

```go
func TestConMap(t *testing.T){
	testMap := make(map[int64]string,30)
	var i int64
	for i =1;i<30;i++ {
		i := i
		go func() {
			testMap[i] = fmt.Sprintf("user%d",i)
		}()
	}

}
// 程序报错 fatal error: concurrent map writes
```

要对map做并发写入，则需要使用`互斥锁`来实现，实现`并发读、同步写`。在使用官方的`sync`包，有两种方案，第一种是`sync.RWMutex（使用锁的缺点是会降低性能）`，第二种是`sync.map（用空间换时间）`
