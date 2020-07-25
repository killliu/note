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

### shell color

echo -e “\033[30m 黑色字 \033[0m” 
echo -e “\033[31m <font color="red">红色字</font>> \033[0m” 
echo -e “\033[32m <font color="green">绿色字</font> \033[0m” 
echo -e “\033[33m <font color="yellow">黄色字</font> \033[0m” 
echo -e “\033[34m <font color="blue">蓝色字</font> \033[0m” 
echo -e “\033[35m <font color="purple">紫色字</font> \033[0m” 
echo -e “\033[36m <font color="cyan">天蓝字</font> \033[0m” 
echo -e “\033[37m 白色字 \033[0m”

echo -e “\033[40;37m <font style="background:black" color="white">黑底白字</font> \033[0m” 
echo -e “\033[41;37m <font style="background:red" color="white">红底白字</font> \033[0m” 
echo -e “\033[42;37m <font style="background:green" color="white">绿底白字</font> \033[0m” 
echo -e “\033[43;37m <font style="background:yellow" color="white">黄底白字</font> \033[0m” 
echo -e “\033[44;37m <font style="background:blue" color="white">蓝底白字</font> \033[0m” 
echo -e “\033[45;37m <font style="background:purple" color="white">紫底白字</font> \033[0m” 
echo -e “\033[46;37m <font style="background:cyan" color="white">天蓝底白字</font> \033[0m” 
echo -e “\033[47;30m <font style="background:white" color="black">白底黑字</font> \033[0m”

\33[0m 				关闭所有属性 
\33[1m 				设置高亮度 
\33[4m 				下划线 
\33[5m 				闪烁 
\33[7m 				反显 
\33[8m 				消隐 
\33[30m — \33[37m 	  设置前景色 
\33[40m — \33[47m 	  设置背景色 
\33[nA 				 光标上移n行 
\33[nB 				 光标下移n行 
\33[nC 				 光标右移n行 
\33[nD 				 光标左移n行 
\33[y;xH			 设置光标位置 
\33[2J 				  清屏 
\33[K 				 清除从光标到行尾的内容 
\33[s 				 保存光标位置 
\33[u 				 恢复光标位置 
\33[?25l 			   隐藏光标 
\33[?25h 			  显示光标