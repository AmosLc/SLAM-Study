## 新建机器人模型并仿真

### 一 文件与语法

#### 1 概述

URDF与xacro文件都可以用来描述要搭建的机器人平台。
其中，URDF比较详细复杂，xacro比较简单使用了宏相关知识

#### 2 功能包文件的结构

urdf用于存放机器人模型的URDF或者xacro文件
meshes用于放置URDF中引用的模型渲染文件
launch保存相关启动文件
config用于保存rviz的配置文件



### 二 搭建的具体过程

#### 1 新建功能包文件,并把功能包复制到～catkin_ws/src文件夹里面

（要符合规范，包含cmakeList和xml文件，参考ROS书上的构建功能包的方法或者修改已有的功能包结构，直接手动新建文件夹是没用的。）

#### 2 定义机器人的机构

编辑URDF文件，定义好机器人模型。这里可以直接用solidworks建模，然后导出为URDF格式即可，不需要在这里花费大量时间。

#### 3 编译安装包

进入catkin_ws文件夹，打开终端，输入下面代码编译：

```
catkin_make
```

#### 4 在rviz可视化环境中查看搭建的机器人平台

打开ROS控制台，启动rviz可视化环境并查看模型：

```
roscore
roslaunch display_mrobot_chassis_urdf.launch
```

#### 5* 安装gazebo控制器（否则会导致后面的gazebo仿真机器人操控功能包会安装失败！）

```
sudo apt-get install ros-kinetic-gazebo-ros-control
```



### 三 基于ArbotiX和rviz的仿真器

ArbotiX是一款控制电机和舵机的控制版，帮助实现机器人在rviz中的运动。主要是ROS书的mrobot_teleop功

能包。

#### 1 安装ArbotiX功能包

进入工作空间的src文件夹，打开终端。输入如下代码：

```
git clone https://github.com/vanadiumlabs/arbotix_ros.git
```

#### 2 编译安装包

进入catkin_ws文件夹，打开终端，输入下面代码编译：

```
catkin_make
```

#### 3 配置ArbotiX

(参考ros书上配置方法，而且注意launch文件在ros书第4章有配置方法)
1）创建launch文件
2）创建配置文件



#### 4 运行仿真环境

打开ROS控制台，输入如下代码运行rviz+ArbotiX环境,并使用键盘控制小车在环境中运行：

```
6 在rviz中查看Kinect2点云内容roscore
roslaunch mrobot_description arbotix_mrobot_with_kinect.launch
roslaunch mrobot_teleop mrobot_teleop.launch
```


这里可能执行第三条语句会报错，这是因为mrobot_teleop功能包中mrobot_teleop.py文件没有写入权限造成的，进入功能包相应位置执行如下代码：

```
chmod 777 mrobot_teleop.py
```

然后再次编译，执行前述代码即可。

#### 5 数据记录与回放

在这里，可以使用如下命令行开始记录数据：

```
mkdir ~/bagfiles
cd ~/bagfiles
rosbag record -a
```

记录完成后，按下Ctrl+C暂停记录数据。
打开根目录下的bagfiles文件夹，找到刚刚记录的后缀为.bag的文件,输入下面代码可以查看之前生成的数据记录文件：

```
rosbag info '保存的文件名称.bag'
```

如果想回放，可以输入下面代码：
(一定注意，之前录入数据时的环境要全部打开，功能包要全部启动完毕，数据才可以复现！)

```
rosbag play '保存的文件名称.bag'
```



### 四 在gazebo下仿真完整机器人/在rviz下查看仿真的摄像头内容

#### 1 启动ros管理器

```
roscore
```



#### 2 打开gazebo仿真界面（注意！必须先进行这一步，不然后面可能出问题！）

```
rosrun gazebo_ros gazebo
```

#### 3 打开rviz可视化界面

```
rosrun rviz rviz
```



#### 4 打开仿真模块（机器人已经定义好了的，并且功能包已经编译好！）

```
roslaunch mrobot_gazebo view_mrobot_with_kinect_gazebo.launch
```

#### 5 打开键盘控制功能

```
roslaunch mrobot_teleop mrobot_teleop.launch
```



#### 6 在rviz中查看Kinect2点云内容

在rviz中FixFrame为camera_frame_optical，然后添加一个PointCloud2类型的插件。

修改插件的topic中的话题为/camera/depth/points。接着就可看到画面了。

（这里或者可以打开RQT窗体查看点云图案）

```
rqt_image_view
```



































