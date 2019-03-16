### 基本类型
标识已划分具体大小内存的数据

1. 布尔类型： `bool`
2. 数值类型：有符号整型、无符号整型，浮点数
   
有符号整型：`int` 、`int8`、 `int16`、 `int32`、 `int64` 

无符号整型：`unit`、 `unit8` 、`unit16` 、`unit32` 、 `unit64` 

浮点数：`float32`、 `float64` 

### 变量

#### 变量
1. 定义一个变量
`var a int`

2. 定义一个变量，并赋值
`var a int = 1`
或者
`a := 1` 但这个只能在函数内使用

#### 常量
使用 `const` 标识


### `switch`

> 1. `switch` 会自动break，除非使用 `fallthrough`
>     `fallthrough` 强制执行后面的case
> 2. `switch`后 可以没有表达式

```
func switch_test(a int) {
	switch a {
	case 1:
	case 2: fallthrough
	default:
	}
}
```

### `if`

> `if` 里可以定义局部变量
> 
> Tips: go 函数可以返回多返回值

```
func getResult() (result int, err error) {
	return 1, nil
}

func main()  {
	if result, err := getResult(); err == nil {
		fmt.Println(result)
	}
}
```

### 指针

> 区别在于：值传递、引用传递

```
// 引用传递
func swap(a, b *int) {

	*b, *a = *a, *b
}

func main()  {
	a, b := 1, 2
	swap(&a, &b)
}
```



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
var c = a                 // c 只是指向 a数值，但并不能改变数组的值
```


### 切片


### `Map`

> 1. map 使用哈希表，必须比较相等
> 2. 除了 slice，map，function 内建类型都可以作为key
> 3. struct类型 不包含上述字段，也可作为key


```
// [] 里是key， 后面的 value
m := map[string]string {
    "haha": "lala",
}

// 遍历比较方便(无序)
for k, v := range m {
    fmt.Println(k, v)
}

// 获得值
haha := m["haha"]

// 判断 key 是否存在
haha, isExist := m["hahah"]
```

### `rune`

编码采用：utf-8

> **UTF-8**: 是一种针对 Unicode 的可变长度字符编码，也是一种前缀码。
> 可以用来表示 Unicode 标准中的任何字符，且其编码中的第一个字节仍与 ASCII 码兼容。

go 底层存储与对外都使用 UTF-8编码

so，当要处理中文字符的时候就需要注意。

而 `rune` 相当与 go 的char

```
s := "你好啊，世界！"

for i, ch := range s {
	fmt.Printf("(%d, %c)", i, ch)
}

fmt.Println()

for i, ch := range []rune(s) {
    fmt.Printf("(%d, %c)", i, ch)
}
```

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


