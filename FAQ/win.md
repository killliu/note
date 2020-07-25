### GIF 工具
https://github.com/NickeManarin/ScreenToGif
下载链接：[百度云](https://pan.baidu.com/s/1Mj5nNfBFHkDHIaFVgrnanA) 提取码：7w92
### images-compress
https://github.com/semiromid/compress-images
下载链接：[百度云](https://pan.baidu.com/s/1nk51kV0zLuwQAbtRhPORTw) 提取码：xgkp
### vmware hyper-v not compatible
管理员打开cmd
```shell
bcdedit /copy {default} /d "Windows 10 Without Hyper-V" 
bcdedit /set {xxxxx} hypervisorlaunchtype off
```
执行完第一条命令会得到一串id，把id替换到第二个命令中的xxxxx即可。
然后运行msconfig，在引导的设置里把超时时间设置到3~5秒以上即可。
### 安装http服务
全局安装
npm install -g http-server 
启动服务
http_folder -> cmd -> http-server

### 软链接
可以理解为文件夹的快捷图标
```shell
set cur_path="%cd"
echo cur_path
rd /q /s your_target_folder_path 	# 删除目标文件夹（如果存在）
mklink /J your_target_folder_path from_folder_path
pause
```
### win10 文件夹中打开cmd
open_cmd_here.reg
```shell
Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\Directory\Background\shell\OpenCmdHere]
@="----- 在此处打开cmd -----"
"Extended"=""

[HKEY_CLASSES_ROOT\Directory\Background\shell\OpenCmdHere\command]
@="cmd.exe -noexit -command Set-Location -literalPath '%V'" 
```
双击如上文件
在任意文件夹中 按住shift键 + 鼠标右键 -> 即可看到 "----- 在此处打开cmd -----" 命令
### 清除垃圾文件
将如下代码写入文档存为 .bat 文件
```shell
@echo off
echo 正在清除系统垃圾文件，请稍等......
del /f /s /q  %systemdrive%\*.tmp
del /f /s /q  %systemdrive%\*._mp
del /f /s /q  %systemdrive%\*.log
del /f /s /q  %systemdrive%\*.gid
del /f /s /q  %systemdrive%\*.chk
del /f /s /q  %systemdrive%\*.old
del /f /s /q  %systemdrive%\recycled\*.*
del /f /s /q  %windir%\*.bak
del /f /s /q  %windir%\prefetch\*.*
rd /s /q %windir%\temp & md  %windir%\temp
del /f /q  %userprofile%\cookies\*.*
del /f /q  %userprofile%\recent\*.*
del /f /s /q  "%userprofile%\Local Settings\Temporary Internet Files\*.*"
del /f /s /q  "%userprofile%\Local Settings\Temp\*.*"
del /f /s /q  "%userprofile%\recent\*.*"
echo 清除系统垃圾完成！
echo. & pause
```
### 强制删除文件
将如下代码写文档，保存为 .bat 批处理文件
```shell
DEL /F /A /Q \\?\%1
RD /S /Q \\?\%1
```
将要删除的文件拖到该批处理文件的图标上即可

### 修改文件名

1. 删除当前目录下所有文件的前 deletenum 个字段

```shell
@echo off
setlocal enabledelayedexpansion
 
set /p deletenum=deletenum:
for /r %%i in (.) do (
    for /f "delims=" %%a in (' dir /b "%%i\*.flv" 2^>nul ') do (
		set "t=%%~na"
        ren "%%i\%%a" "!t:~%deletenum%!%%~xa"
    )
)
pause
```

2. 删除当前目录下所有文件的指定字段

```shell
@echo off
Setlocal Enabledelayedexpansion

set /p "str=your_delete_word:"

for /f "delims=" %%i in ('dir /b *.*') do (
	set "var=%%i" & ren "%%i" "!var:%str%=!"
)
```

