### 基本类型
标识已划分具体大小内存的数据

1. 布尔类型： `bool`
2. 数值类型：有符号整型、无符号整型，浮点数
   
有符号整型：`int` 、`int8`、 `int16`、 `int32`、 `int64` 

无符号整型：`unit`、 `unit8` 、`unit16` 、`unit32` 、 `unit64` 

浮点数：`float32`、 `float64` 


### 数组

```
var a [3]int                    // 定义长度为3的int型数组, 元素全部为0
var b = [...]int{1, 2, 3}       // 定义长度为3的int型数组, 元素为 1, 2, 3
var c = [...]int{2: 3, 1: 2}    // 定义长度为3的int型数组, 元素为 0, 2, 3
var d = [...]int{1, 2, 4: 5, 6} // 定义长度为6的int型数组, 元素为 1, 2, 0, 0, 5, 6
```

```
var a = [...]int{1, 2, 3} // a 是一个数组
var b = &a                // b 是指向数组的指针
var c = a          
```


### 切片


### 配置

`GOPATH` 环境变量，用来指定工作目录的位置。


### package

`package` 可以与 `folder` 不同名


### Go Mod 使用

文档：https://github.com/golang/go/wiki/Modules

#### 配置Mod

可以在`/etc/profile` 下配置

`export GO111MODULE=on` 开启模块支持

- `GO111MODULE=off` 无模块支持，go 会从 GOPATH 和 vendor 文件夹寻找包
- `GO111MODULE=on` 模块支持，go 会忽略 GOPATH 和 vendor 文件夹，只根据 go.mod 下载依赖
- `GO111MODULE=auto` 在 GOPATH/src 外面且根目录有 go.mod 文件时，开启模块支持。

> 在使用模块时候，`GOPATH` 作用如下：
>
> 1. 下载的依赖存放在: `$GOPATH/pkg/mod`
> 2. `go install` 结果存放在 `$GOPATH/bin` 
> 3. Mod Cache 路径在 `$GOPATH/pkg/mod/cache` 

#### 指令

1. `go mod init`  初始化创建文件 `go.mod`

2. `go mod vendor` 命令可以在项目中创建`vendor` 文件夹将依赖包拷贝过来

3. `go mod download` 命令用于将依赖包缓存到本地 Cache

4. `go mod tidy` 增加缺失的包，移除没用的包

5. `go mod verify` 确认依赖关系

6. `go mod why` 解释为什么需要包和模块

7. `go mod graph` 把模块之间的依赖图显示出来

8. `go list -m -json all` 显示所有 import 库信息

在 `$GOPATH` 之外使用 go modules，如果是在现有的项目中可以直接使用`go mod init`，现有项目会根据`go remote` 自动识别 module 名，但新项目中则会报 `go: cannot determine module path for source directory`，所以需要：`go mod init xxx` xxx 为module名
