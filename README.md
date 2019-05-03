![](https://avatars3.githubusercontent.com/u/33419517)

# [Mysql]（https://github.com/fangguizhen/Notes/blob/master/Mysql.md）

# Golang知识点

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

## 持续更新中..

## 问题反馈
* 邮件:(1297394526@qq.com)
* QQ:1297394526
* Github: [@fangguizhen](https://github.com/fangguizhen)
