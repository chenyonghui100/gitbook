---
description: sync
---

# Sync

golang 提供了他sync 包以供使用,常见以下几种。

* sync.atomic​
* sync.Map   : 并发安全的 map
* sync.Mutex​ : 互斥锁
* sync.RWMutex​​  : 读写锁
* sync.once  :在高并发的场景下只执行一次
* sync.pool
* sync.WaitGroup​  : 任务计数器

### sync.waitGroup 任务计数器

```go
func Test_sync_WaitGroup(t *testing.T) {
	var wg sync.WaitGroup
	wg.Add(10)
	// 开 N 个后台打印线程
	for i := 0; i < 10; i++ {
		go func(i int) {       //go 关键字开启协程为方法或者函数调用
			fmt.Println(i)
			wg.Done()
		}(i)
	}
	// 等待 N 个后台线程完成
	wg.Wait()
}
```

### sync.Mutex​
