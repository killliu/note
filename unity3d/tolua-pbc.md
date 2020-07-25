环境：mac  lua5.1.5
源码：[tolua](https://github.com/topameng/tolua)   [lua-protobuf](https://github.com/starwing/lua-protobuf) 
百度云： [lua5.1.5_tolua_lua-protobuf](https://pan.baidu.com/s/1xS-GwiPfk6jkIjf42e0Flw)   密码:  ya1z
### 1. 复制源代码
将 lua-protobuf 中的 pb.c 及 pb.h 复制到 tolua 项目中
在 pb.c 中修改如下代码：
```c
LUALIB_API int luaopen_pb_io(lua_State *L) {
//  some code......
//    if (LUA_VERSION_NUM < 502) {
        luaL_register(L, "pb.io", libs);
//    }else{
//        luaL_newlib(L, libs);
//    }
    return 1;
}

LUALIB_API int luaopen_pb_conv(lua_State *L) {
//  some code......    
//    if (LUA_VERSION_NUM < 502) {
        luaL_register(L, "pb.io", libs);
//    }else{
//        luaL_newlib(L, libs);
//    }
    return 1;
}

LUALIB_API int luaopen_pb(lua_State *L) {
//  some code......
//    if (LUA_VERSION_NUM < 502) {
        luaL_register(L, "pb", libs);
//    }else{
//        luaL_newlib(L, libs);
//    }
    return 1;
}
```
### 2. LuaDll.cs 增加代码：
```c#
[DllImport(LUADLL, CallingConvention = CallingConvention.Cdecl)]
public static extern int luaopen_pb(IntPtr L);

[DllImport(LUADLL, CallingConvention = CallingConvention.Cdecl)]
public static extern int luaopen_pb_io(IntPtr L);

[DllImport(LUADLL, CallingConvention = CallingConvention.Cdecl)]
public static extern int luaopen_pb_conv(IntPtr L);

[DllImport(LUADLL, CallingConvention = CallingConvention.Cdecl)]
public static extern int luaopen_pb_buffer(IntPtr L);

[DllImport(LUADLL, CallingConvention = CallingConvention.Cdecl)]
public static extern int luaopen_pb_slice(IntPtr L);
```
### 3. LuaClient.cs 增加代码
```c#
void OpenLibs(){
    lua.OpenLibs(LuaDLL.luaopen_pb_io);
    lua.OpenLibs(LuaDLL.luaopen_pb_conv);
    lua.OpenLibs(LuaDLL.luaopen_pb_buffer);
    lua.OpenLibs(LuaDLL.luaopen_pb_slice);
    lua.OpenLibs(LuaDLL.luaopen_pb);
}
```
### 4. 编译
> sh ./build_osx.sh

将 lua-protobuf 中的代码 <font color="red"> luaunit.lua  serpent.lua  protoc.lua</font>  复制到项目中
### 5. 验证
```lua
debug = UnityEngine.Debug

local pb = require "pb"
local protoc = require "protoc"

assert(protoc:load[[
    syntax = "proto3";

    message Phone{
        string name = 1
        int64 num = 2
    }

    message Person{
        string name = 1
        int32 age = 2
        repeated Phone cs = 4
    }]])

local data ={
    name = "liu",
    age = 18,
    cs =
    {
        {name = "kill", num = 11111121},
        {name = "bob", num = 120000000}
    }
}

function Main()
    local bytes = assert(pb.encode("Person", data))
    debug.Log(pb.tohex(bytes))
    local data2 = assert(pb.decode("Person", bytes))
    debug.Log(require "serpent".block(data2))
end
```
<font color="red">**errors: xcode-select: error: tool 'xcodebuild' requires Xcode**</font>
xcodebuild的路径不正确, 用<font color="red">终端</font>>将路径切换到Xcode的目录下:

> sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer/