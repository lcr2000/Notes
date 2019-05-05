![](https://avatars3.githubusercontent.com/u/33419517)

# [Mysql](https://github.com/fangguizhen/Notes/blob/master/Mysql.md)

# [数据结构与算法](https://github.com/fangguizhen/Notes/blob/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E4%B8%8E%E7%AE%97%E6%B3%95.md)

# [计算机网络](https://github.com/fangguizhen/Notes/blob/master/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C.md)

# [Golang知识点](https://github.com/fangguizhen/Notes/blob/master/Golang%E7%9F%A5%E8%AF%86%E7%82%B9.md)

## 1、Golang自增自减操作符

- go语言中的++、--操作符都是后置操作符，必须跟在操作数后面，并且它们没有返回值，所以不能用于表达式，它必须是单独的一条语句。

## 2、Golang中分为值类型和引用类型。

- 值类型分别有：int、float、bool、string、数组和结构体。
  - 值类型特点是：变量直接存储值，内存通常在栈中分配。
- 引用类型分别有：指针、slice切片、管道channel、接口interface、map、函数等。(nil只能赋值给这些变量）。
  - 引用类型特点是：变量存储的是一个地址，这个地址对应的空间里才是真正存储的值，内存通常在堆中分配。

## 3、无缓冲的channel与有缓冲的channel

- 无缓冲的channel是同步的，而有缓冲的channel是非同步的。
  比喻：

- 无缓冲的就是一个送信人去你家门口送信，你不在家，他不走，你一定要接下信，他才会走。
  - 无缓冲保证信能到你手上。
- 有缓冲的就是一个送信人去你家扔到你家信箱转身就走，除非信箱满了，他必须等信箱空下来。
  - 有缓冲的保证信能进你家邮箱。

## 4、Golang的25个关键词

|          |             |        |           |        |
| -------- | ----------- | ------ | --------- | ------ |
| break    | default     | func   | interface | select |
| case     | defer       | go     | map       | struct |
| chan     | else        | goto   | package   | switch |
| const    | fallthrough | if     | range     | type   |
| continue | for         | import | return    | var    |

## 5、init

- init函数可以在任何包中有0个或1个或多个。
- 首先初始化导入包的变量和常量，然后执行init函数，最后初始化本包的变量和常量，然后是init函数，最后是main函数。
- main函数只能在main包中有且只有一个。
- init函数和main函数都不能被显示调用。

## 6、bool类型不能转换

## 7、panic

- 当函数发生 panic 时，它会终止运行，在执行完所有的延迟函数后，程序控制返回到该函数的调用方。这样的过程会一直持续下去，直到当前协程的所有函数都返回退出，然后程序会打印出 panic 信息，接着打印出堆栈跟踪（Stack Trace），最后程序终止。

## 8、只有在延迟函数的内部，调用 recover 才有用

- 在延迟函数内调用 recover，可以取到 panic 的错误信息，并且停止 panic 续发事件（Panicking Sequence），程序运行恢复正常。如果在延迟函数的外部调用 recover，就不能停止 panic 续发事件。

## 9、goroutine快速入门

- 主线程是一个物理线程，直接作用在cpu上的，是重量级的，非常耗费CPU资源。
- 协程是从主线程开启的，是轻量级的线程，是逻辑态。对资源消耗相对小。

## 10、goroutine调度模型

MPG基本模式介绍：

- M：操作系统的主线程（是物理线程）

- P：协程执行需要的上下文

- G：协程

M关联了一个内核线程，通过调度器P（上下文）的调度，可以连接1个或者多个G,相当于把一个内核线程切分成了了N个用户线程。M和P是一对一关系（但是实际调度中关系多变），通过P调度N个G（P和G是一对多关系），实现内核线程和G的多对多关系（M:N）。
通过这个方式，一个内核线程就可以起N个Goroutine，同样硬件配置的机器可用的用户线程就成几何级增长，并发性大幅提高。

## 11、Go命名风格

- 1、整体上，Go 采用驼峰风格
- 2、对于常见的缩写，应该采用全小写或全大写，比如：ID，URL，不应该定义为：Id、Url
- 3、常量单词采用首字母大写的方式，如：FirstName。

## 12、如何在Go中打印变量的类型？

- 直接使用reflect的TypeOf方法就可以了

```javascript
//fmt.Println(reflect.TypeOf(var))
func main() {
	var name string
	name = "tom"
	fmt.Println(name)
	fmt.Println(reflect.TypeOf(name))
}
```

## 13、Go中变量的静态类型声明是什么？

- 静态类型(static type)是变量声明的时候的声明类型，在变量声明、new方法创建对象时或者结构体(struct)的元素的类型定义，参数类型等。


## 14、Go中变量的动态类型声明是什么？

- 接口(interface)类型的变量还有一个动态类型，它是运行时赋值给这个变量的具体的值的类型(当然值为nil的时候没有动态类型)。一个变量的动态类型在运行时可能改变，
  这主要依赖它的赋值。

```javascript
var x interface{}  // x 为零值 nil,静态类型为 interface{}
var v *T           // v 为零值 nil, 静态类型为 *T
x = 42             // x 的值为 42,动态类型为int, 静态类型为interface{}
x = v              // x 的值为 (*T)(nil)， 动态类型为 *T, 静态类型为 *T
```

## 15、能在Go中的单个声明中声明多种类型的变量吗？

- 只能在批量声明中声明多种类型的变量

  ```javascript
  var (
  	name string
  	age int
  )
  ```

- 在单个声明中

  ```javascript
  var name, job string
  ```

## 16、break语句的目的是什么？

- break语句用于在循环立即终止，程序控制继续下一个循环语句后面语句。
- break在switch语句中在执行一条case后跳出语句的作用。
- 如果你正在使用嵌套循环(即，一个循环在另一个循环中)，break语句将停止最内层循环的执行，并开始执行的下一行代码的程序段之后。

## 17、Go 语言 continue 语句

- Go 语言的 continue 语句 有点像 break 语句。但是 continue 不是跳出循环，而是跳过当前循环执行下一次循环语句。
- for 循环中，执行 continue 语句会触发for增量语句的执行。

## 18、变量声明和变量定义有什么区别？

- 我们声明的最终目的是为了提前使用，即在定义之前使用，如果不需要提前使用就没有单独声明的必要，变量是如此，函数也是如此，所以声明不会分配存储空间，只有定义时才会分配存储空间。

## 19、什么是指针？

| 概念 | 说明                                                         |
| ---- | ------------------------------------------------------------ |
| 变量 | 是一种占位符，用于引用计算机的内存地址。可理解为内存地址的标签 |
| 指针 | 表示内存地址，表示地址的指向。指针是一个指向另一个变量内存地址的值 |
| &    | 取地址符，例如：{指针}:=&{变量}                              |
| *    | 取值符，例如：{变量}:=*{指针}                                |


## 20、类型转换

- Go不会对数据进行隐式的类型转换，只能手动去执行转换操作
- 不是所有数据类型都能转换的（自定义类型）

## 21、类型断言

- 类型断言的本质，跟类型转换类似，都是类型之间进行转换。

  不同之处在于，类型断言实在接口之间进行。

  ```javascript
  func test6() {
      var i interface{} = "TT"
      j, b := i.(int)
      if b {
          fmt.Printf("%T->%d\n", j, j)
      } else {
          fmt.Println("类型不匹配")
      }
  }
  ```

## 22、goto语句的目的？

- 目的：跳转到指定的标签
- 说明：通过标签进行代码间的无条件跳转。
  goto 语句可以在快速跳出循环、避免重复退出上有一定的帮助
- 作用：
  - 1、使用goto退出多层循环
  - 2、统一错误处理
- 注意：goto不能跳到其他函数或者内层代码。

## 23、Go支持指针算术吗？

- 在很多 golang 程序中，虽然用到了指针，但是并不会对指针进行加减运算
  但是Go 还提供了一些底层的库 reflect 和 unsafe，它们可以让使用者把任意一个 Go 指针转成 uintptr 类型的值，然后再像 C++ 一样对指针做算术运算。

## 24、Go中slice的len()和cap()函数有什么区别？

- 对于make slice而言，有两个概念需要搞清楚：长度跟容量。
  - 容量表示底层数组的大小，长度是你可以使用的大小。
    容量的用处在哪？当你用 append扩展长度时，如果新的长度小于容量，不会更换底层数组，否则，go 会新申请一个底层数组，拷贝这边的值过去，把原来的数组丢掉。也就是说，容量的用途是：在数据拷贝和内存申请的消耗与内存占用之间提供一个权衡。
  - 而长度，则是为了帮助你限制切片可用成员的数量，提供边界查询的。所以用 make 申请好空间后，需要注意不要越界【越 len 】
  ```javascript
  func test(){
	var array =[]int{1,2,3,4,5}// len:5,capacity:5
	var newArray=array[1:3]// len:2,capacity:4   (已经使用了两个位置，所以还空两位置可以append)
	fmt.Printf("%p\n",array) //0xc420098000
	fmt.Printf("%p\n",newArray) //0xc420098008 可以看到newArray的地址指向的是array[1]的地址，即他们底层使用的还是一个数组
	fmt.Printf("%v\n",array) //[1 2 3 4 5]
	fmt.Printf("%v\n",newArray) //[2 3]

	newArray[1]=9 //更改后array、newArray都改变了
	fmt.Printf("%v\n",array) // [1 2 9 4 5]
	fmt.Printf("%v\n",newArray) // [2 9]

	newArray=append(newArray,11,12)//append 操作之后，array的len和capacity不变,newArray的len变为4，capacity：4。因为这是对newArray的操作
	fmt.Printf("%v\n",array) //[1 2 9 11 12] //注意对newArray做append操作之后，array[3],array[4]的值也发生了改变
	fmt.Printf("%v\n",newArray) //[2 9 11 12]

	newArray=append(newArray,13,14) // 因为newArray的len已经等于capacity，所以再次append就会超过capacity值，
	// 此时，append函数内部会创建一个新的底层数组（是一个扩容过的数组），并将array指向的底层数组拷贝过去，然后在追加新的值。
	fmt.Printf("%p\n",array) //0xc420098000
	fmt.Printf("%p\n",newArray) //0xc4200a0000
	fmt.Printf("%v\n",array) //[1 2 9 11 12]
	fmt.Printf("%v\n",newArray) //[2 9 11 12 13 14]  他两已经不再是指向同一个底层数组y了
	}


## 25、如何知道变量分配到堆还是栈

- Go的编译器很聪明，它还会做逃逸分析(escape analysis)，在编译原理中，分析指针动态范围的方法称之为逃逸分析。通俗来讲，当一个对象的指针被多个方法或线程引用时，我们称这个指针发生了逃逸。
- 更简单来说，逃逸分析决定一个变量是分配在堆上还是分配在栈上。
  - 编译器会根据变量是否被外部引用来决定是否逃逸：
    - 如果函数外部没有引用，则优先放到栈中；
    - 如果函数外部存在引用，则必定放到堆中；

注意：

- 堆上动态分配内存比栈上静态分配内存，开销大很多。
- 不要盲目使用变量的指针作为函数参数，虽然它会减少复制操作。但其实当参数为变量自身的时候，复制是在栈上完成的操作，开销远比变量逃逸后动态地在堆上分配内存少的多。
- 变量分配在栈上需要能在编译期确定它的作用域，否则会分配到堆上。

## 持续更新中..

## 问题反馈
* 邮件:1297394526@qq.com
* QQ:1297394526
* Github: [@fangguizhen](https://github.com/fangguizhen)
