# ROS

## ROS编译

ROS中涉及的编程语言以C++和Python为主，ROS中的大多数程序两者都可以实现，在本系列教程中，每一个案例也都会分别使用C++和Python两种方案演示，大家可以根据自身情况选择合适的实现方案。

ROS中的程序即便使用不同的编程语言，实现流程也大致类似，以当前HelloWorld程序为例，实现流程大致如下：

1. 先创建一个工作空间；
2. 再创建一个功能包；
3. 编辑源文件；
4. 编辑配置文件；
5. 编译并执行。

上述流程中，C++和Python只是在步骤3和步骤4的实现细节上存在差异，其他流程基本一致。本节先实现C++和Python程序编写的通用部分步骤1与步骤2，1.3.2节和1.3.3节再分别使用C++和Python编写HelloWorld。

#### 1.创建工作空间并初始化

```
mkdir -p 自定义空间名称/src
cd 自定义空间名称
catkin_make
```

上述命令，首先会创建一个工作空间以及一个 src 子目录，然后再进入工作空间调用 catkin_make命令编译。

#### 2.进入 src 创建 ros 包并添加依赖

```
cd src
catkin_create_pkg 自定义ROS包名 roscpp rospy std_msgs
```

上述命令，会在工作空间下生成一个功能包，该功能包依赖于 roscpp、rospy 与 std_msgs，其中roscpp是使用C++实现的库，而rospy则是使用python实现的库，std_msgs是标准消息库，创建ROS功能包时，一般都会依赖这三个库实现。

------

**注意:** 在ROS中，虽然实现同一功能时，C++和Python可以互换，但是具体选择哪种语言，需要视需求而定，因为两种语言相较而言:C++运行效率高但是编码效率低，而Python则反之，基于二者互补的特点，ROS设计者分别设计了roscpp与rospy库，前者旨在成为ROS的高性能库，而后者则一般用于对性能无要求的场景，旨在提高开发效率。



本节内容基于1.3.1，假设你已经创建了ROS的工作空间，并且创建了ROS的功能包，那么就可以进入核心步骤了，使用C++编写程序实现：

#### 1.进入 ros 包的 src 目录编辑源文件

```
cd 自定义的包
```

C++源码实现(文件名自定义)

```cpp
#include "ros/ros.h"

int main(int argc, char *argv[])
{
    //执行 ros 节点初始化
    ros::init(argc,argv,"hello");
    //创建 ros 节点句柄(非必须)
    ros::NodeHandle n;
    //控制台输出 hello world
    ROS_INFO("hello world!");

    return 0;
}
```

#### 2.编辑 ros 包下的 Cmakelist.txt文件

```cmake
add_executable(步骤3的源文件名
  src/步骤3的源文件名.cpp
)
target_link_libraries(步骤3的源文件名
  ${catkin_LIBRARIES}
)
```

#### 3.进入工作空间目录并编译

```
cd 自定义空间名称
catkin_make
```

生成 build devel ....

#### 4.执行

**先启动命令行1：**

```
roscore
```

**再启动命令行2：**

```shell
cd 工作空间
source ./devel/setup.bash
rosrun 包名 C++节点
```

命令行输出: HelloWorld!

**PS:**`source ~/工作空间/devel/setup.bash`可以添加进`.bashrc`文件，使用上更方便

添加方式1: 直接使用 gedit 或 vi 编辑 .bashrc 文件，最后添加该内容

添加方式2:`echo "source ~/工作空间/devel/setup.bash" >> ~/.bashrc`



## ROS问题处理

换路径时候编译报错

![image-20240523160903177](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240523160903177.png)