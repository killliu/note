# centos7

### 安装   

```shell
yum install epel-release
yum install golang -y
go version
```

### 设置环境变量

> vi /etc/profile

  追加如下代码：

```shell
# golang evn config
export GO111MODULE=on
export GOPROXY=https://goproxy.io
```

  重置配置

> source /etc/profile

 <font color="darkgray"> (注：现在的 go 版本已经不需要设置 GOROOT 了，如果自己设置错了，使用 unset GOROOT 取消手动配置，使用默认的 GOROOT )</font>

### 测试

```shell
cd ~
mkdir test
cd test
vi go.go
package main
import "fmt"
func main(){
	fmt.Println("hello go!")
}
go run ./go.go
```

