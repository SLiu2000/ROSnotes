## ROS工程结构和通信机制

### ROS工程结构

![Figure_2.1.1](/Users/sissiliu/Desktop/Figure_2.1.1.png)

- catkin工作空间

  - catkin
    - ROS定制的编译构建系统
    - 对CMake的扩展
  - catkin工作空间
    - 组织和管理功能包的文件夹
    - 以catkin工具编译

- ROS编程步骤

  1. 建立工作空间

     ```shell
     mkdir -p  ~/catkin_ws/src           #~/catkin_ws为用户定义的工作空间目录
     ```

  2. 编译工作空间

     ```shell
     cd ~/catkin_ws                      #回到工作空间  
     catkin_make                         #编译工作空间
     source ~/catkin_ws/devel/setup.bash #编译完成后要source刷新环境
     ```

  - build文件：CMake和catkin缓存信息和中间文件

  - devel文件：生成的目标文件，包括（头文件、动态静态库，还有可执行文件）

  - src文件：package源代码包

  - package：

    - ROS软件的基本组织形式
    - catkin编译的基本单元
    - 一个package可以包含多个可执行文件

    包含CMakelist.txt和package.xml

  - CMakeLists.txt：

    | 名称                                                       | 内容                                  |
    | ---------------------------------------------------------- | ------------------------------------- |
    | cmake_minimum_required()                                   | 指定catkin最低版本                    |
    | project()                                                  | 指定软件包的名称                      |
    | find_package()                                             | 指定编译时需要的依赖项                |
    | add_message_files()/add_service_files()/add_action_files() | 添加消息文件/服务文件/动作            |
    | generate_messages()                                        | 生成消息、服务、动作                  |
    | catkin_package()                                           | 指定catkin信息给编译系统生成Cmake文件 |
    | add_library()/add_executable()                             | 指定生成库文件、可执行文件            |
    | target_link_libraries()                                    | 指定可执行文件去链接哪些库            |
    | catkin_add_gtest()                                         | 添加测试单元                          |
    | install()                                                  | 生成可安装目标                        |

  - cmakelist.txt结构

    由以下5个部分：

    1. cmake要求的最低版本

    ```cmake
    cmake_minimum_required(VERSION 2.8.3)
    ```

    1. package名称

    ```cmake
    project(mypackage)
    ```

    1. 编译package需要的依赖项

    ```cmake
    find_package(catkin REQUIRED COMPONENTS )
    ```

    1. 生成catkin程序包

    ```cmake
    catkin_package()
    ```

    1. 添加所需头文件

    ```cmake
    include_directories( ${catkin_INCLUDE_DIRS})
    ```

  - ![Figure_2.1.16](/Users/sissiliu/Desktop/Figure_2.1.16.png)

  - ![Figure_2.1.17](/Users/sissiliu/Desktop/Figure_2.1.17.png)

  - ![Figure_2.1.18](/Users/sissiliu/Desktop/Figure_2.1.18.png)

  - ![Figure_2.1.20](/Users/sissiliu/Desktop/Figure_2.1.20.png)

  - 一个规范的package有以下结构

    | 目录           | 描述                                            |
    | -------------- | ----------------------------------------------- |
    | src            | 存放.cpp源文件                                  |
    | srv            | 存放.srv文件 设置服务器类型（可选）             |
    | action         | 存放.action文件 设置actionlib（可选）           |
    | cfg            | 存放.cfg文件 设置动态调节参数（可选）           |
    | launch         | 存放launch文件 启动节点以及配置相应参数（可选） |
    | msg            | 存放.msg文件 存放数据类型 （可选）              |
    | include        | 存放.h头文件                                    |
    | config         | 存放URDF模型文件，已经其他配置文件 （可选）     |
    | scripts        | 存放python文件（可选）                          |
    | package.xml    |                                                 |
    | cmakeLists.txt |                                                 |

  - 常用指令

    | 命令              | 用法                                | 说明                    |
    | ----------------- | ----------------------------------- | ----------------------- |
    | rospack           | rospack find package_name           | 查找某个pkg的地址       |
    | roscd             | roscd package_name                  | 跳转到某个pkg路径下     |
    | rosls             | rosls package_name                  | 列举某个pkg下的文件信息 |
    | rosed             | roscd package_name file_name        | 编辑pkg中的文件         |
    | catkin_create_pkg | catkin_create_pkg <pkg_name> [deps] | 创建一个pkg             |
    | rosdep            | rosdep install [pkg_name]           | 安装某个pkg所需的依赖   |



### ROS通信机制

- 节点

  - **master - 节点管理器**

    - 每个node启动时都要向master注册
    - 管理node之间的通信

  - **roscore**

    启动的方法非常简单，用`roscore`这条指令，在终端命令行输入`roscore`会启动master，当然他还顺带启动了rosout和parameter Service。rosout负责日志输出，它是一个node，告诉我们现在系统有什么错误，我们可使用这些信息用于调试。Parameter Service用于进行一些参数的配置，`roscore`是启动ROS的第一步。

  - **node**

    `roscore`启动了master节点，第二步我们要启动一些具体的ROS程序。这里讲一下node（节点）这个概念，节点是什么呢？ node其实就是进程，在ROS里面我们有个专门的名字叫做node（节点）。node是ROS的一个进程，我们之前说过，一个pkg可以有多个可执行文件，可以是C++编译生成的可执行文件，也可以是Python脚本，这每一个可执行文件，运行起来就是一个个node。`roscore`本质上是启动了一个node，只不过稍微做了封装。

  - **rosrun**：启动一个node

    ```shell
    rosrun [pkg_name] [node_name]
    ```

  - **rosnode**：管理和调试node

    | 命令                   | 描述                                                         |
    | ---------------------- | ------------------------------------------------------------ |
    | rosnode list           | 列出当前运行的node信息                                       |
    | rosnode info node_name | 显示出node的详细信息                                         |
    | rosnode kill node_name | 结束某个或多个node                                           |
    | rosnode kill           | 进入交互模式。能够从编号列表中选择要结束的节点，这对于结束匿名节点非常有用。 |
    | rosnode kill -a/--all  | 结束所有节点                                                 |
    | rosnode cleanup        | 清除不可到达节点的注册信息                                   |

  - **roslaunch**

    如果我们的node很多怎么办？比如一个机器人系统设计的很完备，有多node，我们是不是需要依此使用命令`roscore`，`rosrun` ，`rosrun`...`rosrun`。实际上不用，ROS为我们提供了一个更简便的方法，`roslaunch`可以帮我们同时启动master和多个node和配置参数服务器。我们之前讲ROS的工程结构，通常可以在包目录下建一个launch文件夹，里面有.launch文件，这个.launch文件就是我们这里要运行的这个launch。通过配置好启动规则，要启动哪些node，传什么参数进去等等，就可以达到批量启动node的目的。如果我们没有运行`roscore`，运行 roslaunch会帮我们自动启动roscore。

    ```shell
    roslaunch [pkg_name] [file_name.launch]
    ```

  - **launch文件**

    Launch文件规则和packag.xml写法规则一样，它遵循xml标签语言规范。在launch里面可以指定要启动的node，配置参数，还可以嵌套其他的launch文件，设置参数等。初学者自己在写的时候，建议直接找别人的模板来改， 遇到问题上官网查一下具体的写法。

  - **通信方式**

    ROS的通信方式大致可以分为话题、服务、动作和参数服务器。

    - Topic
    - Service
    - Actionlib
    - Parameter Service

- 话题

  Topic（话题）是ROS用的最多的一种通信方式，两个node之间要通信，一般会先定义好一个共同的话题，然后节点a向这个话题publish消息，node b会subscribe订阅这个话题，那么a说话b就能收到。

  - 一个完整的Topic通信有以下七个步骤

    1. Publisher注册
    2. Subscriber注册
    3. ROS master进行信息匹配
    4. Subscriber发送连接请求
    5. Publisher确认连接请求
    6. Subscriber尝试与Publisher建立连接
    7. Publisher向Subscriber发送数据

  - **rostopic**

    | 命令                        | 说明                     |
    | --------------------------- | ------------------------ |
    | rostopic info topic_name    | 显示某个topic的属性信息  |
    | rostopic echo topic_name    | 显示某个topic的内容      |
    | rostopic pub topic_name ... | 向某个topic发布内容      |
    | rostopic bw topic_name      | 查看某个topic的带宽      |
    | rostopic hz topic_name      | 查看某个topic的频率      |
    | rostopic list               | 列出当前所有的topic      |
    | rostopic find topic_type    | 查找某个类型的topic      |
    | rostopic type topic_name    | 查看某个topic的类型(msg) |

  - **rosmsg**

    | 命令                  | 说明              |
    | --------------------- | ----------------- |
    | rosmsg list           | 列出系统上所有msg |
    | rosmsg show /msg_name | 显示某个msg内容   |

