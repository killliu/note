环境：mac

文件：protobuf-2.5.0.zip

​      protoc-gen-lua-master.zip

下载：[百度云](https://pan.baidu.com/s/16PSBZhIZywd8Q-oNzym-dg) 密码：wucu

```shell
# 编译安装 protobuf-2.5.0 
./configure
make
make check
make install
# 安装 python 
brew install python
cd ../protobuf-2.5.0		# 回到 protobuf-2.5.0 目录
python setup.py install 
cd /usr/local/bin && sudo ln -s /你的路径/protoc-gen-lua/plugin/protoc-gen-lua
protoc --lua_out=./ client.proto 
```