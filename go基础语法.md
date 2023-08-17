# Go语言结构

## 1. 包声明

- 包名称和目录保持一致

- 名称使用**小写**

  - 拓展

    | 类型     | 命名                                                         |
    | -------- | ------------------------------------------------------------ |
    | 包       | 小写                                                         |
    | 文件     | 1. 小写<br />2. 用下划线分隔单词                             |
    | 结构体   | 驼峰命名法                                                   |
    | 接口     | 1. 小写+用下划线分隔单词<br />2. 单个函数的结构名要以“er”为结构体后缀 |
    | 变量     | 1. 驼峰命名法<br />2. 私有变量（小写）<br />    常量（全部大写） |
    | 错误处理 | 错误描述如果是英文，必须全部小写，且不要标点结尾             |
    | 单元测试 | 1. 测试文件命名规范`example_test.go`<br />2. 测试函数必须以`Test`开头，例如`TestExample` |

    

## 2. 引入包

* ```go
  import "fmt"
  ```

  * `fmt 包`实现了格式化 IO（输入/输出）的函数

## 3. 函数

* `fmt.Println` 比 `fmt.Print` 多输出一个换行符，方便输出多行内容时的阅读。

  * | 函数          | 输出             | 示例                                                  |
    | :------------ | :--------------- | :---------------------------------------------------- |
    | `fmt.Print`   | 不换行，不格式化 | `fmt.Print("Hello", "world")` 输出：Helloworld        |
    | `fmt.Printf`  | 不换行，格式化   | `fmt.Printf("%s %d", "Hello", 42)` 输出：Hello 42     |
    | `fmt.Println` | 换行，格式化     | `fmt.Println("Hello", "world")` 输出：<br>Hello world |

* `func main() `是程序开始执行的函数。main 函数是每一个可执行程序所必须包含的，一般来说都是在启动后第一个执行的函数

* |           |                       描述                       |                         输出                          |
  | --------- | :----------------------------------------------: | :---------------------------------------------------: |
  | `Sprintf` | 根据格式化参数生成格式化的字符串并返回该字符串。 | **var** target_url=fmt.Sprintf(url,stockcode,enddate) |
  | `Printf`  | 根据格式化参数生成格式化的字符串并写入标准输出。 |                fmt.Println(target_url)                |

- 在Go语言中,函数名如果是大写字母开头,表示该函数是可以被其他包访问的公开函数。
  - 看到一个Go语言的函数名是大写的,意味着这个函数是designed to be used by other packages（是一个公开的API接口）

- 定义函数

  ```go
  func function_name( [传入参数列表] ) [返回值类型] {
     函数体
  }
  ```

  * go函数可以返回多个值

    * `return y, x`

  * **方法**是一种特殊类型的函数

    * ```go
      package main
      
      import (
         "fmt"  
      )
      
      /* 定义结构体 */
      type Circle struct {
        radius float64
      }
      
      func main() {
        var c1 Circle
        c1.radius = 10.00
        fmt.Println("圆的面积 = ", c1.getArea())
      }
      
      //该 method 属于 Circle 类型对象中的方法
      func (c Circle) getArea() float64 {
        //c.radius 即为 Circle 类型对象中的属性
        return 3.14 * c.radius * c.radius
      }
      ```

      * `func (c Circle) getArea() float64 { ... }`：这是方法的定义格式。在函数名 `getArea` 之前，你看到了 `(c Circle)`，这个部分表示这个方法是关联到 `Circle` 这个类型的。因此，这个方法只能由 `Circle` 类型的变量调用。

* 函数闭包

  * Go 语言支持匿名函数，可作为闭包。匿名函数是一个"内联"语句或表达式。匿名函数的优越性在于可以直接使用函数内的变量，不必申明。

    * 匿名函数

      ```go
      func() {
          // 函数体
      }
      ```

    * **例子**

      ```go
      func getSequence() func() int {
         i:=0
         return func() int {
            i+=1
           return i  
         }
      }
      ```

      * 函数定义`getSenquence`，这个函数返回的是`func()`函数，这个函数返回值是`int`类型

## 4. 语法

### 变量

* 声明变量

  * 如果声明后不使用，会报错 

    ```go
    var x int
    ```

  * 短变量声明：

    * **intVal := 1** 
    * 只能在函数体内，在外部是不能生效的

    ```go
    //相当于
    var intVal int 
    intVal = 1 
    ```

  * 匿名变量

    * ```go
      func GetNameAndAge()(string, int){
      	return "tom", 20		//这两个就是匿名变量
      }
      
      _, age := getNameAndAge()  //第一个参数也是匿名变量
      ```

* 变量类型

  * | 序号 | 类型和描述                                                   |
    | :--- | :----------------------------------------------------------- |
    | 1    | **布尔型** 布尔型的值只可以是常量 true 或者 false。一个简单的例子：var b bool = true。 |
    | 2    | **数字类型** 整型 int 和浮点型 float32、float64，Go 语言支持整型和浮点型数字，并且支持复数，其中位的运算采用补码。 |
    | 3    | **字符串类型:** 字符串就是一串固定长度的字符连接起来的字符序列。Go 的字符串是由单个字节连接起来的。Go 语言的字符串的字节使用 UTF-8 编码标识 Unicode 文本。 |
    | 4    | **派生类型:** 包括：(a) 指针类型（Pointer）(b) 数组类型(c) 结构化类型(struct)(d) Channel 类型(e) 函数类型(f) 切片类型(g) 接口类型（interface）(h) Map 类型 |


### 常量

- 定义常量变量

  * 单个定义：

    * `const b string = "abc"`

  * 多个定义：

    ```go
    const(
    	a = 100
        b = 200
    )
    ```

  * 定义后不可以再修改

  * 类型可以省略

- 特殊的`iota`

  - 可以被编译器修改的常量，默认值是0，每调用一次加1

  - ```go
    const(
    	a1 = iota
        a2 = iota
        a3 = iota
    )
    
    fmt.print("a1: %v\n", a1)
    fmt.print("a2: %v\n", a2)
    fmt.print("a3: %v\n", a3)
    
    //运行结果
    //a1: 0
    //a2: 1
    //a3: 2
    ```

  - 

### 数组

* ```go
  //var arrayName [size]dataType
  var balance [10]float32
  ```

* ```go
  //这样也可以
  numbers := [5]int{1, 2, 3, 4, 5}
  ```

* ```go
  //  将索引为 1 和 3 的元素初始化
  balance := [5]float32{1:2.0,3:7.0}
  ```

### 指针

* ```
  var ip *int        /* 指向整型*/
  var fp *float32    /* 指向浮点型 */
  ```

* 空指针
  * nil 指针也称为空指针。
  * 指针变量通常缩写为 ptr

### 结构体

* ```go
  type Books struct {
     title string
     author string
     subject string
     book_id int
  }
  
  /* 打印 Book1 信息 */
     fmt.Printf( "Book 1 title : %s\n", Book1.title)
     fmt.Printf( "Book 1 author : %s\n", Book1.author)
     fmt.Printf( "Book 1 subject : %s\n", Book1.subject)
     fmt.Printf( "Book 1 book_id : %d\n", Book1.book_id)
  ```

### 切片

* 切片是一个动态数组，它可以根据需要自动扩容。

  * ```go
    //数组
    arr := [4]int{0,1,2,3}
    //切片
    slice := arr[1:3]
    
    //或者
    slice := []int{0，1，2，3}
    ```

  * 长度是指包含的数字的多少，如`1:3`就是2个

  * 容量是指从开始到原数组的结尾中间包含的数字的多少

* `[]`是左闭右开区间

* `append`函数是用于向切片（slice）追加元素的函数

  * 如果容量后，就加到最后
  * 如果容量不够，就重新开辟一个空间，赋值原内容，再加上新内容

### range（范围）

* 格式

  ```go
  for key, value := range name(定义range的名字){
  	name[key] = value
  }
  ```

  * 对于如果不需要`key` 或`value`的话，就用`_`来代替

    ```go
    for _, value := range slide{
        fmt.Printf("value is: %d\n", value)
    }
    ```

### map(集合)

* 是无序集合

* 通过key定位索引

  ```go
  // 创建一个空的Map , 集合类型是string, key类型是int， 容量为10
  m := make(map[string]int, 10)
  
  // 使用字面量创建 Map
  m := map[string]int{
      "apple": 1,
      "banana": 2,
      "orange": 3,
  }
  
  // 获取元素key：
  v1 := m["apple"]
  v2, ok := m["pear"]  // 如果键不存在，ok 的值为 false，v2 的值为该类型的零值
  
  
  // 修改元素：
  m["apple"] = 5
  
  
  // 获取 Map 的长度
  len := len(m)
  
  
  // 遍历 Map
  for k, v := range m {
      fmt.Printf("key=%s, value=%d\n", k, v)
  }
  
  // 删除元素：
  delete(m, "banana")
  ```


* 需要注意的是 ` { ` 不能单独放在一行，所以以下代码在运行时会产生错误

* Go 语言的字符串连接可以通过 **+** 实现

* 反引号（`）

  * 以下是一个使用反引号创建的多行字符串字面量的示例：

    ```go
    str := `
    这是一段
    多行文本，
    可以包含换行符和特殊字符。`
    ```

### 接口（interface）

* interface把所有具有共性的方法定义在一起，让其他类型只要实现了这些方法就是实现了这接口

* ```go
  /* 定义接口 */
  type interface_name interface {
     method_name1 [return_type]
     method_name2 [return_type]
     method_name3 [return_type]
     ...
     method_namen [return_type]
  }
  
  /* 定义结构体 */
  type struct_name struct {
     /* variables */
  }
  
  /* 实现接口方法 */
  func (struct_name_variable struct_name) method_name1() [return_type] {
     /* 方法实现 */
  }
  ...
  func (struct_name_variable struct_name) method_namen() [return_type] {
     /* 方法实现*/
  }
  ```

* ```go
  //例子
  package main
  
  import (
      "fmt"
  )
  
  type Phone interface {
      call()
  }
  
  type NokiaPhone struct {
  }
  
  func (nokiaPhone NokiaPhone) call() {
      fmt.Println("I am Nokia, I can call you!")
  }
  
  type IPhone struct {
  }
  
  func (iPhone IPhone) call() {
      fmt.Println("I am iPhone, I can call you!")
  }
  
  func main() {
      var phone Phone
  
      phone = new(NokiaPhone)
      phone.call()
  
      phone = new(IPhone)
      phone.call()
  
  }
  ```

### go并发

* 我们只需要通过 go 关键字来开启 goroutine 即可

  * ```go
    go 函数名( 参数列表 )
    ```

### 通道（channel）

* 声明一个通道很简单，我们使用chan关键字即可，通道在使用前必须先创建：

  ```go
  ch := make(chan int)
  
  //通道可以设置缓冲区，通过 make 的第二个参数指定缓冲区大小
  
  ch := make(chan int, 100)
  ```

  ```go
  ch <- v    // 把 v 发送到通道 ch
  v := <-ch  // 从 ch 接收数据
             // 并把值赋给 v
  ```

  

## 5. 语句&表达式

* 条件语句

  * ```go
    if a < 20 {
           /* 如果条件为 true 则执行以下语句 */
           fmt.Printf("a 小于 20\n" )
       }
    ```

  * ```
    if a < 20 {
           /* 如果条件为 true 则执行以下语句 */
           fmt.Printf("a 小于 20\n" );
       } else {
           /* 如果条件为 false 则执行以下语句 */
           fmt.Printf("a 不小于 20\n" );
       }
    ```

* case 语句

  * ```go
    switch marks {
          case 90: grade = "A"
          case 80: grade = "B"
          case 50,60,70 : grade = "C"
          default: grade = "D"  
       }
    ```

  * 变量 marks 可以是任何类型

* 循环语句

  * ```go
    for i := 0; i <= 10; i++ {
             sum += i
          }
    ```

## 6. 注释

* 单行注释
  * 或`#`
* 多行注释
  * 已以 `/*` 开头，并以 `*/ `结尾





p14开始继续