# 70S2开源插件源码

仅修改客户端插件（重命名中文函数Workflow编译异常、开启怪物变身riding.cpp）部分，其它为S2开源插件源码

## 项目结构

- **客户端插件**：[Client](https://github.com/manydots/Client)
- **服务端插件**：[Server](https://github.com/manydots/Server)

| 模块                 | 插件名称         | 部署路径             |
| -------------------- | ---------------- | -------------------- |
| Client（客户端插件） | Fd.dll           | 客户端目录           |
| Server（服务端插件） | libfd_monitor.so | /home/neople/monitor |
|                      | libfd.so         | /home/neople/game    |

### Actions Workflow

```yaml
# 移除管理项目Actions Workflow构建
# 使用workflow_dispatch手动触发构建
# Server/.github/workflows/build-server.yml  服务端插件
# Client/.github/workflows/build-client.yml  客户端插件

#...

# 手动触发
on:
  workflow_dispatch:

# push触发
on:
  push:
    branches: [ main ]

# ...

```

<div align="center">
  <img src="https://github.com/manydots/publicWelfarePlugin/raw/main/screenshot/server-workflow.png" width="100%">
  <img src="https://github.com/manydots/publicWelfarePlugin/raw/main/screenshot/client-workflow.png" width="100%">
</div>

### 插件使用

```shell
# 客户端插件
# (客户端插件关闭绑定IP fixIp())
# 客户端加载Fd.dll

# 服务端插件
# 插件上传
# libfd_monitor.so -> /home/neople/monitor
# libfd.so -> /home/neople/game

# run文件修改

# 服务器名称
SERVER_NAME="siroco"

# ...
# df_monitor_r
LD_PRELOAD="./libfd_monitor.so" ./df_monitor_r mnt_${SERVER_NAME} start &

# ...
# df_game_r
LD_PRELOAD="./libfd.so" ./df_game_r "${SERVER_NAME}11" start &

```

### Linux Cmake环境安装

```shell

# 安装cmake依赖
yum install -y gcc gcc-c++ glibc-devel.i686 libgcc.i686 libstdc++.i686 gcc-c++.i686

# 安装cmake
yum install -y wget
cd /usr/local
wget https://github.com/Kitware/CMake/releases/download/v3.22.6/cmake-3.22.6-linux-x86_64.tar.gz

# 或者手动上传至 /usr/local/cmake-3.22.6-linux-x86_64.tar.gz
tar -zxvf cmake-3.22.6-linux-x86_64.tar.gz
ln -sf /usr/local/cmake-3.22.6-linux-x86_64/bin/cmake /usr/bin/cmake

# 验证cmake版本
cmake --version

```

### Linux Cmake编译服务端插件

```shell

# 1.进入服务端项目
cd /Server/monitor
# cd /Server/server

# 2.修复 CMakeLists.txt 字符问题(如果有问题)
sed -i '1s/^.//' CMakeLists.txt

# 3.进入编译目录
rm -rf build && mkdir build && cd build

# 4.生成32位工程
# rm -rf ./*
cmake .. -DCMAKE_C_FLAGS="-m32" -DCMAKE_CXX_FLAGS="-m32"

# 5.执行编译
make -j4
```

### 其它资源

- [Cloudflare](https://developers.cloudflare.com/warp-client/get-started/windows/)
- [Visual Studio 2022](https://visualstudio.microsoft.com/zh-hans/downloads/)
