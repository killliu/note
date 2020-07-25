### 位操作

```c#
private uint GetVersionUInt(string s)
{
    string[] ss = s.Split('.');
    return (uint.Parse(ss[0]) << 24) + (uint.Parse(ss[1]) << 16) + (uint.Parse(ss[2]) << 8) + uint.Parse(ss[3]); } private string GetVersionStr(uint i) { return string.Format("{0}.{1}.{2}.{3}", (i>> 24), ((i & 16777215) >> 16), ((i & 65535) >> 8), (i & 255));
}
```

<<>> 移位

`左移`n位相当于<font color="red">乘以</font>2的n次幂： *x*<<*n*=*x*∗2*n* `右移`n位相当于<font color="red">除以</font>2的n次幂： *x*>>*n*=*x*2*n*

`& | ^ 按位与、或、异或`

`&` 按位与处理两个长度相同的二进制数，两个相应的二进位都为1，则该位为1，否则该位为0 
`|` 按位或处理两个长度相同的二进制数，两个相应的二进位中只要有一个为1则该位为1，否则该位为0 
`^` 按位异或处理两个长度相同的二进制数，两个相应的二进位不同为1，否则该位为0 
eg. x & 1 == 0 ? `偶数` : `奇数` 
eg. 取第n位的bit值 ： r = x & (1 << n) 
eg. 设第n位的bit值为1 ： r = x | (1 << n) 
eg. 设第n位的bit值为0 ： r = x & ~(1 << n) 
eg. 将第n位的值取反 ： r = x ^ (1 << n)