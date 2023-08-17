# Go 命令工具

## Go Mod(依赖管理)

* 设置moduel `go env -w GO111MODULE=on`

## 1. Go mod init

* `go mod init github.com/Milefer7/toktik`
  * 根目录运行命令
  * 初始化模块。它会创建 `go.mod` 来记录的项目依赖和版本信息。
  * 后面的命令要根据你项目在GitHub的位置，具体看网页就好

## 2. Go get

* `go get [package]`
  * 其中，`[package]` 是要获取的Go模块或库的导入路径。
  * **用于从远程代码仓库下载并安装Go模块或库**
* `go get -u [package]`
  * `go get` 命令不再默认将获取的包安装到全局的 `GOPATH` 下，而是会将其安装到当前工作目录的 `go.mod` 文件中指定的依赖项路径下。如果你希望安装到全局的 `GOPATH`，可以使用 `-u` 参

## 3. Go mod download

* `go mod download `  
  * 将所有依赖项下载到本地缓存

* 总结来说，`go mod download` 主要用于显式下载依赖模块，而 `go get` 用于安装或更新包，如果没有参数则获取所有未安装的依赖项。
  * 大多数情况下，你不需要手动使用 `go mod download`，除非你希望在构建之前确保所有依赖都已下载，或者在特定的情况下需要手动控制依赖的下载过程。

## 4. Go mod tidy

* `go mod tidy`
  * 整理依赖。此命令会检查项目源码，移除不再使用的依赖，并增加需要的依赖.

## go build

* 生成一个`.exe`文件，可以用代码直接运行
* ![image-20230811164246051](C:\Users\hello\AppData\Roaming\Typora\typora-user-images\image-20230811164246051.png)

![image-20230811164304441](C:\Users\hello\AppData\Roaming\Typora\typora-user-images\image-20230811164304441.png)

