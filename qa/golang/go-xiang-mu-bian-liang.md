# go 项目变量

在 main 中定义的某个变量，可能在项目很多地方调用， 如何处理？

解决方案： 新建一个 global 文件，存储公共变量。 在 main 中对其赋值。如下：

```go
package main
import 	"global"
func init() {
    flag.StringVar(&global.Env, "env", "dev", "config path, eg: -conf config.yaml")
}
```

这样在项目中所有地方，都可以去调用 global.env

需要注意的是，因为不是常量，对这个值的修改需要特别谨慎。
