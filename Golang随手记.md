#### Golang随手记

##### 类型断言

```
func main() {
	var x interface{}
	x = 10
	value, ok := x.(int)
	fmt.Print(value, ",", ok)
}
```

类型断言的必要条件就是x是接口类型，非接口类型的x不能做类型断言

