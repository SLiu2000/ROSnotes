## Ubuntu & ROS安装和配置

- docker commands:
  - List image: `docker image list`
  - List running image: `docker ps -a`
  - Commit changes: `docker commit [container ID] [name of the new repository]`
  - Save image: `docker save -o [filename.tar] [name of the repository]`
  - Start image: `docker start [name of the image]`
  - Stop image: `docker stop [name of the image]`
  - Run image: `docker run -p 81:80 dorowu/ubuntu-desktop-lxde-vnc`



- 编译工作空间：

  - ```bash
    #添加主从机，当使用仿真时添加如下
    export ROS_MASTER_URI=http://127.0.0.1:11311  #主机IP和端口，默认为11311端口
    export ROS_HOSTNAME=127.0.0.1                 #从机IP。仿真为主从机使用同一台计算机
    
    #添加主从机，当使用XBot时添加如下
    export ROS_MASTER_URI=http://192.168.8.101:11311   #主机IP和端口，默认为11311端口
    export ROS_HOSTNAME=192.168.8.x             #从机IP，请在笔记本上使用ifconfig查看实际IP
    ```



- Xbot仿真环境：
  - 启动XBot仿真环境：`roslaunch robot_sim_demo robot_spawn.launch`
  - 使用python脚本控制XBot机器人移动：`rosrun robot_sim_demo robot_keyboard_teleop.py`
  - 查看当前摄像头所对应的画面（启动rqt_image_view）：`rosrun image_view image_view image:=/camera/rgb/image_raw`
  - 机器人在运动过程中摄像头画面的变化情况：`rosrun robot_sim_demo robot_keyboard_teleop.py`
  - 列出所有节点：`rosnode list`
  - 查看/cmd_vel_mux节点信息：`rosnode info /cmd_vel_mux`





