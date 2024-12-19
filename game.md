# Cpp游戏开发

## 井字棋

### Windows

windows下使用Visual Studio开发

1.easyx提供peekmessage()函数利用类似消息队列机制拉取信息

2.及时清空/更新画面

3.双缓冲

```cpp
#include <graphics.h>
#include <iostream>

int main() {
	//printf("%ws", GetEasyXVer());
	//std::wcout << GetEasyXVer() << std::endl;
	initgraph(1200, 720);
	int x = 300;
	int y = 300;
	BeginBatchDraw();

	while (true) {
		ExMessage msg;
		while (peekmessage(&msg)) {
			if (msg.message == WM_MOUSEMOVE) {
				x = msg.x;
				y = msg.y;
			}
		}
		cleardevice();
		solidcircle(x, y, 100);
		FlushBatchDraw();
	}
	EndBatchDraw();
	return 0;
}
```

### linux

- 我们将easyx压缩包移至linux下面，将lib64的库文件以及include文件放到我们所需要的工程文件夹下面。
- 我们需要安装windows下的编译器 mingw，编译时需要设定编译其为`/usr/bin/x86_64-w64-mingw32-g++`
- 命令 `set(CMAKE_EXE_LINKER_FLAGS "-static-libgcc -static-libstdc++")` 让 cmake 采用静态编译
- 编译出可执行文件之后是.exe不可以直接执行，可以使用`wine`在linux下 模拟运行windows的程序
- 关于中文乱码，直接使用英文即可

CMakeLists.txt如下：

```makefile
# CMake 最低版本要求
cmake_minimum_required(VERSION 3.10)

# 项目名称
project(check_game)

# 指定 C 和 C++ 编译器
# set(CMAKE_C_COMPILER "/usr/bin/gcc")
# set(CMAKE_CXX_COMPILER "/usr/bin/g++")
set(CMAKE_C_COMPILER "/usr/bin/x86_64-w64-mingw32-gcc")
set(CMAKE_CXX_COMPILER "/usr/bin/x86_64-w64-mingw32-g++")

# 设置 C++ 标准
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)
set(CMAKE_EXE_LINKER_FLAGS "-static-libgcc -static-libstdc++")

# 设置编译输出路径
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)

# 链接库文件目录
link_directories(${PROJECT_SOURCE_DIR}/lib)

# 包含头文件目录
include_directories(${PROJECT_SOURCE_DIR}/inc)

# 可执行文件 target 定义
add_executable(check_game src/main.cpp)

# 链接静态库
target_link_libraries(check_game easyx)

```

文件夹结构如下：

![image-20241121111717991](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20241121111717991.png)

## 打地鼠

```C++
在类中定义std::function回调函数 实现一个设定回调函数的方法
在类外部实例化该类，可使用lambda表达式直接写出回调函数
```

