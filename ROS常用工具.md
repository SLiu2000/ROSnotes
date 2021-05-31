## ROS常用工具

| 种类           | 工具                                    |
| -------------- | --------------------------------------- |
| 仿真环境       | Gazebo                                  |
| 可视化调试工具 | Rviz、rqt系列工具                       |
| 命令行工具     | roscore、rostopic、rosservice、rosbag等 |
| 专用工具       | Moveit!                                 |

- **RViz**

  Rviz是一个可视化工具，你可能感觉它像Gazebo，但Rviz不是用来仿真的，它是用来图形化显示我们需要的一些信息。

  我们查看一下XBot中相关传感器的数据。

  - **仿真下运行**

    1. 添加ROS主从配置

       ```shell
       vim ~/.bashrc
       ```

       ```shell
       #export ROS_MASTER_URI=http://192.168.8.101:11311
       export ROS_MASTER_URI=http://127.0.0.1:11311
       #export ROS_HOSTNAME=192.168.8.xxx
       export ROS_HOSTNAME=127.0.0.1
       ```

    2. 启动仿真

       ```shell
       roslaunch robot_sim_demo robot_spawn.launch
       ```

    3. 启动Rviz，打开一个新的终端，输入以下命令

       ```shell
       rosrun rviz rviz
       ```

    4. 添加相关控件

       Rviz GUI中左边是要在Rviz里显示的各种类型的控件，点击add，加入robot model，就会载入当前的机器人模型。我们依此加入image，Laserscan。大家要注意，Rviz是可视化工具，这里面每个控件都是subscriber相关topic。大家打开每个空间设置，在下拉菜单中选择订阅的topic。

  - **在XBot上运行**

    1. 修改~/.bashrc，添加ROS主从配置（用于配置环境，创建客户机和机器人192.168.8.101的网络通信）

       ```shell
       vim ~/.bashrc
       ```

       ```shell
       export ROS_MASTER_URI=http://192.168.8.101:11311
       #export ROS_MASTER_URI=http://127.0.0.1:11311
       export ROS_HOSTNAME=192.168.8.xxx             #学生笔记本下使用ifconfig本机DHCP IP
       #export ROS_HOSTNAME=127.0.0.1
       ```

    2. 启动XBot（XBot开机自启，或运行以下命令）

       ```shell
       roslaunch xbot_bringup xbot-u.launch
       ```

       重复仿真中操作第3和第4步。

- rqt

  rqt是基于qt开发的可视化工具，扩展性好，灵活易用，可跨平台。r代表ROS，qt是指它是Qt图形界面（GUI）工具包。rqt由三个部分组成，除了rqt核心模块,还有rqt_common_plugin（后端图形工具套件），以及rqt_robot_plugins（机器人运行时的交互工具）。

  - **运行集成图形界面rqt_gui**
    - 打开一个终端

    - 输入`rqt`

    - 也可以运行单个插件

      ```shell
      rqt –standalone <plugin-name>
      ```

  - 常用插件

    rqt核心包提供了一些不需要rosrun就能运行的常用的插件，包括`rqt_console`, `rqt_graph`, `rqt_plot`，`rqt_logger_level`和`rqt_bag`。这些工具主要用于机器人的调试。前三个应该是在调试工作中会经常用到的。

    | 命令             | 描述                                                         | 使用场景                           |
    | ---------------- | ------------------------------------------------------------ | ---------------------------------- |
    | rqt_console      | 允许您通过节点查看发布的消息，包括info、warn、error、fatal。 | 用于节点出现错误时，查看重要信息。 |
    | rqt_logger_level | 查看调试消息                                                 | 查看低级信息                       |
    | rqt              | 通用`rqt`控制台，包括rqt_graph、rqt_plot等程序               |                                    |

    - **rqt_console**

      - 随时间收集消息，在列表视图里显示并实时更新，包括如下内容
        - message（用户指定的消息)
        - everity（消息的严重性级别，例如debug，info, warn, error等等）
        - node（广播节点的名称）
        - time（广播消息的时间），topics，location

      - **仿真下运行**

        1. 添加ROS主从配置

           ```shell
           vim ~/.bashrc
           ```

           ```shell
           #export ROS_MASTER_URI=http://192.168.8.101:11311
           export ROS_MASTER_URI=http://127.0.0.1:11311
           #export ROS_HOSTNAME=192.168.8.xxx
           export ROS_HOSTNAME=127.0.0.1
           ```

        2. 启动仿真

           ```shell
           roslaunch robot_sim_demo robot_spawn.launch
           ```

        3. 启动square_dance_demo.py，仿真下XBot机器人开始运动。打开一个新的终端，输入以下命令

           ```shell
           python ./square_dance_demo.py
           ```

        4. 启动`rqt_console`

           ```shell
           rqt_console
           ```

      - **XBot环境下运行**

        1. 修改~/.bashrc，添加ROS主从配置

           ```shell
           vim ~/.bashrc
           ```

           ```shell
           export ROS_MASTER_URI=http://192.168.8.101:11311
           #export ROS_MASTER_URI=http://127.0.0.1:11311
           export ROS_HOSTNAME=192.168.8.xxx         #学生笔记本下使用ifconfig本机DHCP IP
           #export ROS_HOSTNAME=127.0.0.1
           ```

        2. 启动XBot（XBot开机自启，或运行以下命令）

           ```shell
           roslaunch xbot_bringup xbot-u.launch
           ```

        3. 运行square_dance_demo_xbot.py，XBot机器人开始运动

           ```shell
           python ./square_dance_demo_xbot.py
           ```

        4. 启动`rqt_console`

           ```shell
           rqt_console
           ```

    - **rqt_plot**

      - **仿真下运行**

        1. 添加ROS主从配置

           ```shell
           vim ~/.bashrc
           ```

           ```shell
           #export ROS_MASTER_URI=http://192.168.8.101:11311
           export ROS_MASTER_URI=http://127.0.0.1:11311
           #export ROS_HOSTNAME=192.168.8.xxx
           export ROS_HOSTNAME=127.0.0.1
           ```

        2. 启动仿真

           ```shell
           roslaunch robot_sim_demo robot_spawn.launch
           ```

        3. 启动GMapping SLAM建图程序

           ```shell
           roslaunch slam_sim_demo gmapping_demo.launch    #启动GMapping SLAM程序
           ```

        4. 启动`rqt_plot`，输入需要跟踪的topic `/scan`

           ```shell
           rqt_plot
           #或
           rqt_plot topic_name
           ```

      - **XBot环境下运行**

        1. 修改~/.bashrc，添加ROS主从配置

           ```shell
           vim ~/.bashrc
           ```

           ```shell
           export ROS_MASTER_URI=http://192.168.8.101:11311
           #export ROS_MASTER_URI=http://127.0.0.1:11311
           export ROS_HOSTNAME=192.168.8.xxx         #学生笔记本下使用ifconfig本机DHCP IP
           #export ROS_HOSTNAME=127.0.0.1
           ```

        2. 启动XBot（XBot开机自启，或运行以下命令启动）

           ```shell
           roslaunch xbot_bringup xbot-u.launch
           ```

        3. 启动GMapping SLAM程序，打开一个新的终端，运行以下命令

           ```shell
           ssh xbot@192.168.8.101                             #ssh登录XBot
           roslaunch xbot_navi build_map.launch               #启动GMapping SLAM程序
           ```

        4. 启动`rqt_plot`，输入需要跟踪的topic `/scan`，也可以选择/mobile_base/sensors/echo后选择front或者rear，这几个topic只在XBot上有效。打开一个新的终端，运行以下命

           ```shell
           rqt_plot
           #或
           rqt_plot topic_name
           ```

    - **rqt_graph**

      `rqt_graph`是一个图像化显示通信架构的工具，直观的展示当前正在运行的node、topic和消息的流向。`rqt_graph`不会自动更新信息，需要手工点击刷新按钮进行刷新。

    - **rqt_logger_level**

      配合`rqt_console`查看日志的级别

    - **rqt_bag**

      用于图形化的查看包的内容

- **rosbag**

  `rosbag`主要的作用是记录发布在一个或多个topic上的信息，并在需要的时候回放这些信息。这两项功能可以用于调试机器人程序。具体来讲，就是我们可以先运行机器人，记录下关注的topic，例如摄像头或者另外一些传感器，之后多次回放这些topic的信息，然后通过实验来处理这些数据。这样就能方便的获取和分析来自各个传感器的信息。bag是一种特殊格式的文件，它用来储存带有时间戳的message数据，命名以.bag作为后缀。这类文件通常就是由`rosbag`之类的工具创建的。文件中的信息可以根据用户的需求反复回放。

  - **rosbag**

    - 用来记录和回放发布在一个或多个topic上的信息
    - 作用：记录机器人的运行信息，回放并处理这些数据

  - **bag**

    - 一种特定格式的文件，用来储存带有时间戳的message数据
    - 命名以.bag作为后缀
    - 通常由诸如rosbag之类的工具创建

  - **rosbag命令汇总**

    | 命令       | 描述                                                 |
    | ---------- | ---------------------------------------------------- |
    | record     | 用指定topics的内容记录一个包（.bag）文件             |
    | info       | 查看包中的内容                                       |
    | play       | 回放包中的内容                                       |
    | check      | 确定一个包是否可以在当前系统中使用，或者是否可以迁移 |
    | fix        | 修复包中的消息，以便在当前系统中播放                 |
    | filter     | 用Python表达式转换包                                 |
    | compress   | 压缩一个或多个包                                     |
    | decompress | 解压一个或多个包                                     |
    | reindex    | 重新索引一个或多个包                                 |

- **RViz**

  - **显示类型（部分）**

    | 名称              | 描述                                                       |
    | ----------------- | ---------------------------------------------------------- |
    | Axes              | 显示一组坐标轴                                             |
    | Camera            | 从该摄像机的视角创建一个新的渲染窗口，在其上覆盖原来的图像 |
    | Grid              | 显示2D或3D网格                                             |
    | Grid Cells        | 从网格中提取单元，通常是代价地图中的障碍物                 |
    | Image             | 创建一个带有图像的渲染窗口                                 |
    | InteractiveMarker | 从一个或多个交互标记服务器显示3D对象                       |
    | Laser Scan        | 显示来自激光扫描仪的数据                                   |
    | Map               | 在地平面显示地图                                           |
    | Markers           | 允许程序员通过主题显示任意的原始形状                       |

    | 名称        | 描述                                             |
    | ----------- | ------------------------------------------------ |
    | Path        | 从导航功能包集显示一条路径                       |
    | Point       | 将点绘制成一个小球体                             |
    | Pose        | 将位姿绘制成箭头或轴                             |
    | Pose Array  | 绘制箭头“云”，每一个箭头代表位姿阵列中的一个位姿 |
    | Point Cloud | 由点云显示数据                                   |

  - **窗口内容**

    看一下窗口的其他部分，最下面显示的是时间，右上方view的type是可选的，基本上Orbit和TopDownOrtho两个就已经够用了，一个用于3D视图，另一个则是2D俯视图。在窗口最上方，菜单栏上是关于当前模式的选项,interact显示当前的交互式标记，move camera是view中提到过的3D视图，select允许以鼠标点击的形式选择条目。

    | 状态    | 意义 | 颜色 |
    | ------- | ---- | ---- |
    | OK      | 正常 | 黑色 |
    | Warning | 警告 | 黄色 |
    | Error   | 错误 | 红色 |
    | Disable | 禁用 | 灰色 |

    每个显示都有一个相应的状态与之对应，通过状态可以了解某一显示是否正常。状态一共有四种：OK（正常），Warning（警告），Error（错误）和Disabled（禁用）。不同的状态用不同的背景色表示，黑色为OK，黄色为Warning，红色为Error，灰色为Disabled



<u>问题</u>：

1. ```shell
   chmod u+x sine_talker.py
   ```

2. 两个python程序的含义

   - sine_talker.py

   ```python
   #!/usr/bin/env python
   
   import rospy
   import math
   from std_msgs.msg import Float64
   
   def sine_talker():
       pub = rospy.Publisher('sine_signal',Float64,queue_size=5)
       rospy.init_node('sine_talker')
       rate = rospy.Rate(20)
       while not rospy.is_shutdown():
           time_now = rospy.get_time()
           sine_signal = math.sin(time_now)
           if 0.8 > abs(sine_signal):
               out_put = "Normal output: %s" % sine_signal
               rospy.loginfo(out_put)  
           elif 0.8 < abs(sine_signal) and 0.9 > abs(sine_signal):
               out_put = "Dangerous output: %s" %sine_signal
               rospy.logwarn(out_put)
           elif 0.9 < abs(sine_signal):
               out_put = "Wrong output: %s" %sine_signal
               rospy.logerr(out_put)
           pub.publish(sine_signal)
           rate.sleep()
   
   if __name__ == '__main__':
       try:
           sine_talker()
       except rospy.ROSInterruptException:
           pass
   ```

   - rqt_plot_randomnu.py

   ```python
   #!/usr/bin/env python
   
   import rospy
   from random import random
   from std_msgs.msg import Float32
   
   def talker():
       pub = rospy.Publisher('chatter', Float32, queue_size=10)
       rospy.init_node('demo')
       rate = rospy.Rate(10) # 10hz
       while not rospy.is_shutdown():
           hello_int = random() * 50
           rospy.loginfo(hello_int)
           pub.publish(hello_int)
           rate.sleep()
   
   if __name__ == '__main__':
       try:
           talker()
       except rospy.ROSInterruptException:
           pass
   ```

