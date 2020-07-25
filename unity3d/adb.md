将 sdk 的目录中的platform-tools目录配置到环境变量路径中
打开 cmd.exe
```shell
adb devices			 # 查看当前USB连接的设备
adb tcpip 5555		 # 关于手机 -> 状态消息 中可以查看到手机的 ip (eg. 192.168.5.11)
adb connect 192.168.5.11:5555
connected to 192.168.5.11:5555
```
<font color="red">error: more than one device/emlator</font>

> **adb -s your-device-or-emlator shell**

