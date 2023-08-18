# :open_book:什么是ORM

* 对象和数据库表中原本没有直接映射关系
* 为了解决这个问题有了ORM技术

# Gorm

### 安装命令

```wsl
go get -u gorm.io/gorm
go get -u gorm.io/driver/sqlite
```

- 检查是否安装

  - ```wsl
    go list -m gorm.io/gorm
    ```

### 模型定义

- 从数据库读取的数据会先保存到预先定义的模型对象，然后我们就可以从模型对象得到我们想要的数据。
- 插入数据到数据库也是先新建一个模型对象，然后把想要保存的数据先保存到模型对象，然后把模型对象保存到数据库。

1.  定义gorm model

   1. ```go
     //字段注释说明了gorm库把struct字段转换为表字段名长什么样子。
     type Food struct {
     	Id         int  //表字段名为：id
     	Name       string //表字段名为：name
     	Price      float64 //表字段名为：price
     	TypeId     int  //表字段名为：type_id
     	//字段定义后面使用两个反引号``包裹起来的字符串部分叫做标签定义，这个是golang的基础语法，不同的库会定义不同的标签，有不同的含义
     	CreateTime int64 `gorm:"column:createtime"`  //表字段名为：createtime
     }
     ```

     * 在golang中gorm模型定义是通过struct实现的，这样我们就可以通过gorm库实现struct类型和mysql表数据的映射。

   2. 结构体的嵌套

      GORM 定义一个 gorm.Model 结构体，其包括字段 ID、CreatedAt、UpdatedAt、DeletedAt。

      ```go
      // gorm.Model 的定义
      type Model struct {
        ID        uint           `gorm:"primaryKey"`
        CreatedAt time.Time
        UpdatedAt time.Time
        DeletedAt gorm.DeletedAt `gorm:"index"`
      }
      ```

      以将它嵌入到我们的结构体中，就以包含这几个字段，类似继承的效果。

   3. 例子：

      ```go
      type User struct {
        gorm.Model // 嵌入gorm.Model的字段
        Name string
      }
      ```

2. gorm模型标签

   * `gorm:"column:createtime"`这样的标签定义语法，定义struct字段的列名（表字段名）。
     gorm标签语法：**`gorm:"标签定义"`**

3. 自动更新时间

   * GORM 约定使用 CreatedAt、UpdatedAt 追踪创建/更新时间。如果定义了这种字段，GORM 在创建、更新时会自动填充当前时间。

   * 要使用不同名称的字段，您可以配置 autoCreateTime、autoUpdateTime 标签

   * 如果想要保存 UNIX（毫/纳）秒时间戳，而不是 time，只需简单地将 time.Time 修改为 int 即可。

     * 例子：

       ```go
       type User struct {
         CreatedAt time.Time // 默认创建时间字段， 在创建时，如果该字段值为零值，则使用当前时间填充
         UpdatedAt int       // 默认更新时间字段， 在创建时该字段值为零值或者在更新时，使用当前时间戳秒数填充
         Updated   int64 `gorm:"autoUpdateTime:nano"` // 自定义字段， 使用时间戳填纳秒数充更新时间
         Updated   int64 `gorm:"autoUpdateTime:milli"` //自定义字段， 使用时间戳毫秒数填充更新时间
         Created   int64 `gorm:"autoCreateTime"`      //自定义字段， 使用时间戳秒数填充创建时间
       ```

### 定义表名

* 可以通过定义struct类型的TableName函数实现定义模型的表名	

  ```GO
  func (p Product) TableName() string{
      return "product"
  }
  ```

  - 为结构体实现TableName的接口

  - 返回的字符串就是表名，这里是product


### 连接数据库

> 两个步骤
>
> 1. 配置DSN
> 2. 使用`gorm.Open`连接数据库

#### DSN（数据源名称）

- 格式

  `username:password@tcp(host:port)/Dbname?charset=utf8&parseTime=True&loc=Local`\

- 举例

  `root:123456@tcp(localhost:3306)/tizi365?charset=utf8&parseTime=True&loc=Local`

  ```go
  //mysql dsn格式
  //涉及参数:
  //username   数据库账号
  //password   数据库密码
  //host       数据库连接地址，可以是Ip或者域名
  //port       数据库端口
  //Dbname     数据库名
  //  charset=utf8 客户端字符集为utf8
  //  parseTime=true 支持把数据库datetime和date类型转换为golang的time.Time类型
  //  loc=Local 使用系统本地时区
  ```

#### 使用gorm.Open连接数据库

- 连接到数据库

  ```go
  db, err := gorm.Open(mysql.Open(dsn), &gorm.Config{})
  if err != nil {
  		panic("连接数据库失败, error=" + err.Error())
  	}
  ```

#### 连接池

* 待补充[GORM连接Mysql数据库 - 梯子教程网 (tizi365.com)](https://www.tizi365.com/archives/15.html)

### 插入数据crud

1. 创建数据

   ```go
   //定义一个用户，并初始化数据
   u := User{
   	Username:"tizi365",
   	Password:"123456",
   	CreateTime:time.Now().Unix(),
   }
   //插入一条用户数据
   db.Create(&u)
   
   //一般项目中我们会类似下面的写法，通过Error对象检测，插入数据有没有成功，如果没有错误那就是数据写入成功了。
   if err := db.Create(&u).Error; err != nil {
   	fmt.Println("插入失败", err)
   	return
   }
   ```

2. 查询数据

   > :heavy_check_mark:我们需要定义一个结构体，用来存放查询的结果

   ```go
   //定义接收查询结果的结构体变量
   food := Food{}
   //查询 
   db.Take(&food)
   ```

   * 把`Take`换成`First`/`Last`就是查询第一条/最后一条数据

   > :heavy_check_mark: 把`Take`换成`Find`就是查询多条数据

    Find函数返回的是一个数组, 我们需要定义一个数组，用来存放查询的结果

   ```go
   //因为Find返回的是数组，所以定义一个商品数组用来接收结果
   var foods []Food
   
   db.Find(&foods)
   ```

   > :heavy_check_mark: 查询错误处理

   通过db.Error属性判断查询结果是否出错, Error属性不等于nil表示有错误发生。

   ```
   例子：
   if err := db.Take(&food).Error; err != nil {
       fmt.Println("查询失败", err)
   }
   ```

3. 更新数据

4. 删除数据





## :thinking: 参考文档

1. [GORM查询数据 - 梯子教程网 (tizi365.com)](https://www.tizi365.com/archives/20.html)