### test module

Xxx_test.go

> go test -v

```go
// 测试入口
func TestMain(m *testing.M) {
	ClearTable()
	m.Run()         // 测试分支
	ClearTable()
}

func ClearTable() {
	dbConn.Exec("truncate users")
}

func TestUsers(t *testing.T) {
	t.Run("AddUser", testAddUsers)
	t.Run("GetUser", testGetUsers)
}
func testAddUsers(t *testing.T) {
	error := AddUser("kl", "123")
	if error != nil {
		t.Errorf("Error of add user: %v", error)
	}
}
func testGetUsers(t *testing.T) {
	pwd, error := GetUser("kl")
	if pwd != "123" || error != nil {
		t.Errorf("Error of get user: %v", error)
	}
}
```

### benchmark

一般以Benchmark开头

case一般会跑b.N次，每次执行都会如此

在执行过程中会根据实际case的执行时间是否会稳定增加b.N的次数以达到稳态

> go test -bench=.

```go
func BenchmarkAll(b *testing.B){
    for n:=0; n<b.N; n++{
        Print1to20()
        // BenchEver(n) // 非稳态！1：执行1次，2：执行2次，n：执行n次
    }                   // b.N 会一直增长以试图达到稳态
}

func Print1to20{
    res := 0
    for i := 1; i <= 20; i++{
        res += i
    }
    return res
}

func BenchEver(n int)int{ // 执行n次, 每次都递减到 0
    for n > 0 {
        n--
    }
    return n
}
```

