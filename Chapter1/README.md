## 第一章：准备开发环境


### 1.1 安装开发工具

终端输入：

``` 
sudo apt update
sudo apt install -y \
    git \
    cmake \
    clang \
    check \
    gdb \
    curl \
    build-essential \
    pkg-config
```

先来解释一下都做了什么

`sudo apt update`

更新 APT 本地的包索引缓存，确保获取的是最新的包版本。

`apt install -y ...\`

无交互方式安装以下列出的软件包，`-y` 表示自动回答“yes”跳过确认。

那么我们都一口气安装了什么工具呢？

| 包名              | 作用                                   | 常见用途                                    |
| ----------------- | -------------------------------------- | ------------------------------------------- |
| `git`             | 分布式版本控制工具                     | 克隆仓库、管理代码版本                      |
| `cmake`           | 构建自动化工具，生成 Makefile 或 Ninja | 跨平台构建系统，用于配置 C/C++ 项目         |
| `clang`           | LLVM 提供的 C/C++/Objective-C 编译器   | 替代 GCC，强调模块化、诊断优秀              |
| `check`           | C 语言单元测试框架                     | 编写 C 单元测试代码，提供断言和测试报告     |
| `gdb`             | GNU 调试器                             | 调试 C/C++ 程序，如设置断点、单步执行       |
| `curl`            | 命令行 HTTP 客户端                     | 下载文件、测试接口、API 调用                |
| `build-essential` | 编译工具合集（含 gcc、g++、make 等）   | C/C++ 开发环境的核心包                      |
| `pkg-config`      | 查询编译时所需的库和参数               | 自动获取 `.pc` 文件中指定的 `-I`、`-l` 参数 |
| `ca-certificates` | SSL 根证书                             | 确保 curl、git 等工具能正常进行 HTTPS 通信  |

### 1.2 Linux的重要理念

Linux有很多要学习，我在这里只传达几个重要的理念，和标注出可能会用的命令。

1.  一切皆文件

无论是硬盘、网络、终端、设备，Linux 都以“文件”方式暴露出来。  
读文件 -\> 操作设备；写文件 -\> 控制网络；标准输入输出 -\> 就是文件描述符。

``` sourceCode
cat /proc/cpuinfo     # 查看CPU信息（本质是个文件）
ls /dev/tty*           # 所有终端也是设备文件
```

2.  用小工具组合构建大系统

不要造轮子，善用 `|` 管道、重定向、组合命令。Linux 鼓励用小程序“串起来”。

``` sourceCode
cat access.log | grep 404 | wc -l   # 查找所有 404 错误并统计数量
```

3.  自动化优于手动操作

Shell 是系统接口，不是临时敲命令的地方。重复的命令一定写脚本、一定重定向日志、一定写 Makefile。

``` sourceCode
#!/bin/bash
make && ./test && echo "测试通过"
```

4.  默认非 root，默认无权限

默认安全、默认最小权限。不用 root 权限运行服务、不随意 `chmod 777`、不共享宿主资源。

``` sourceCode
sudo adduser devuser   # 创建干净的开发用户
chown devuser:devuser  # 赋予目录权限
```

5.  可解释、可替换、可构建

Linux 一切都鼓励透明操作。配置是文本、依赖是包、结果是二进制、构建是自动化。

``` sourceCode
less /etc/ssh/sshd_config   # 所有服务配置都可查看、可编辑
```

### 1.3 Linux常用命令

文件与目录操作

| 命令              | 用途                         |
| ----------------- | ---------------------------- |
| `ls -al`          | 列出所有文件（含隐藏）+ 权限 |
| `cd /path/to/dir` | 切换目录                     |
| `cp a.txt b.txt`  | 复制文件                     |
| `mv a.txt dir/`   | 移动/重命名                  |
| `rm -rf dir/`     | 强制删除目录（危险）         |
| `tree`            | 树状显示目录结构（需安装）   |

用户权限管理

| 命令                    | 用途           |
| ----------------------- | -------------- |
| `whoami`                | 当前用户       |
| `sudo`                  | 提权执行       |
| `chmod +x file.sh`      | 赋可执行权限   |
| `chown user:group file` | 修改属主属组   |
| `groups`                | 当前用户组列表 |

软件安装与管理（Debian/Ubuntu）

| 命令                                  | 用途             |
| ------------------------------------- | ---------------- |
| `sudo apt update && sudo apt upgrade` | 更新软件源       |
| `sudo apt install cmake git gdb`      | 安装常用工具     |
| `dpkg -l`                             | 查看所有已安装包 |
| `which gcc`                           | 查找程序路径     |

进程与内存查看

| 命令          | 用途          |
| ------------- | ------------- |
| `ps aux`      | grep myprog\` |
| `top / htop`  | 实时资源占用  |
| `kill -9 PID` | 强制杀进程    |
| `free -h`     | 内存状态      |

网络相关

| 命令                                 | 用途             |
| ------------------------------------ | ---------------- |
| `curl -X POST http://localhost:8080` | 模拟接口请求     |
| `ss -tuln`                           | 查看端口监听状态 |
| `ping 8.8.8.8`                       | 测试网络连通性   |
| `ifconfig / ip a`                    | 查看网络 IP      |

查找与分析

| 命令                 | 用途                   |
| -------------------- | ---------------------- |
| `grep "main"`        | 在文件中查找文本       |
| `find . -name "*.c"` | 查找所有 `.c` 文件     |
| `du -sh *`           | 查看当前目录占用       |
| `df -h`              | 查看磁盘挂载和使用情况 |