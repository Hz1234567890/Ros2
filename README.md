# Ros2
## 一、ros2的安装
### 1.1设置locale
* 根据文档描述只要是支持UTF-8的locale都可以
~~~
    sudo apt update && sudo apt install locales
    sudo locale-gen en_US en_US.UTF-8
    sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
    export LANG=en_US.UTF-8
~~~
***
### 1.2设置Ubuntu软件源
#### 1.2.1首先确认是否存在已经启用的Universe源
    apt-cache policy | grep universe

返回可能有若干行，但是应该包含类似如下内容：
~~~
 100 https://mirrors.aliyun.com/ubuntu jammy-backports/universe i386 Packages
     release v=22.04,o=Ubuntu,a=jammy-backports,n=jammy,l=Ubuntu,c=universe,b=i386
 100 https://mirrors.aliyun.com/ubuntu jammy-backports/universe amd64 Packages
     release v=22.04,o=Ubuntu,a=jammy-backports,n=jammy,l=Ubuntu,c=universe,b=amd64
 500 https://mirrors.aliyun.com/ubuntu jammy-updates/universe i386 Packages
     release v=22.04,o=Ubuntu,a=jammy-updates,n=jammy,l=Ubuntu,c=universe,b=i386
 500 https://mirrors.aliyun.com/ubuntu jammy-updates/universe amd64 Packages
     release v=22.04,o=Ubuntu,a=jammy-updates,n=jammy,l=Ubuntu,c=universe,b=amd64
 500 https://mirrors.aliyun.com/ubuntu jammy-security/universe i386 Packages
     release v=22.04,o=Ubuntu,a=jammy-security,n=jammy,l=Ubuntu,c=universe,b=i386
 500 https://mirrors.aliyun.com/ubuntu jammy-security/universe amd64 Packages
     release v=22.04,o=Ubuntu,a=jammy-security,n=jammy,l=Ubuntu,c=universe,b=amd64
 500 https://mirrors.aliyun.com/ubuntu jammy/universe i386 Packages
     release v=22.04,o=Ubuntu,a=jammy,n=jammy,l=Ubuntu,c=universe,b=i386
 500 https://mirrors.aliyun.com/ubuntu jammy/universe amd64 Packages
     release v=22.04,o=Ubuntu,a=jammy,n=jammy,l=Ubuntu,c=universe,b=amd64
~~~
**如果没有包含类似上述的内容，那么执行如下命令：**
~~~
sudo apt install software-properties-common
sudo add-apt-repository universe
~~~
***
### 1.3添加Ros2 apt仓库
#### 1.3.1添加证书
~~~
sudo apt update && sudo apt install curl -y
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
~~~
>这里可能会出问题，像
>>curl: (7) Failed to connect to raw.githubusercontent.com port 443: Connection refused 
>
>**直接去csdn搜吧！！！**前一天晚上出了问题的，第二天照着官网做了同样的步骤没有报错捏，很逆天qwq


####1.3.2添加ros仓库
~~~
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
~~~

### 1.4安装ros2包
**这里安装的是Desktop版本**
~~~
sudo apt update
sudo apt upgrade
sudo apt install ros-humble-desktop
~~~
>其他版本的安装可以参考[Ros2官网](https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debians.html)

### 1.5配置环境变量
~~~
source /opt/ros/humble/setup.bash
echo " source /opt/ros/humble/setup.bash" >> ~/.bashrc 
~~~

### 1.6测试
#### 1.6.1
打开第一个终端，启动一个数据的发布者节点：

    ros2 run demo_nodes_cpp talker

#### 1.6.2
打开第二个终端，启动一个数据的订阅者节点：

    ros2 run demo_nodes_py listener


![显示效果](./images/Ros测试.png)
#### 1.6.3
开启乌龟仿真

    ros2 run turtlesim turtlesim_node
再启动一个终端，运行

    ros2 run turtlesim turtle_teleop_key
用键盘控制乌龟运动

## 二、ROS2命令行操作
所有操作都集成在一个ros2的总命令中，后边第一个参数表示不同的操作目的，**比如node表示对节点的操作，topic表示对话题的操作**
### 2.1运行节点程序

    ros2 run 。。。
    
#### 例如运行海龟仿真节点和键盘控制节点

    ros2 run turtlesim turtlesim_node
    ros2 run turtlesim turtle_teleop_key

### 2.2查询节点信息
查看当前运行的ros2系统中都有哪些节点

    ros2 node list

##### 生成ros node的帮助文档
    ros2 node

利用info子命令，查看某一节点的详细信息
    
    ros2 node info 节点名称

    例如：
    ros2 node info /turtlesim 

### 2.3查看话题信息
查看当前系统中都有哪些话题
    ros2 topic list
##### 生成ros2 topic的帮助文档
    ros2 topic
利用echo子命令，生成某一话题的消息数据

    ros2 topic echo 话题名称
    
    例如：
    ros2 topic echo /turtle1/pose 

### 2.4发布话题信息
例:通过命令行发布话题指令来控制海龟，让海龟动起来<br>


    ros2 topic pub --rate 1 /turtle1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 2.0, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 1.8}}"

显示效果：<br>![发布话题信息](./images/发布话题信息.png)

### 2.5发送服务请求
例：产生新的海龟<br>
    
    ros2 service call /spawn turtlesim/srv/Spawn "{x: 2, y: 2, theta: 0.2, name: 'shuizao'}"
##### 生成ros2 service的帮助文档

    ros2 service
显示效果：<br>![发送话题请求](./images/发送服务请求--产生新海龟.png)
>在这里，我生成了一个名为“shuizao”的新乌龟，若想控制这只乌龟，需要将接口名称改为/shuizao/······<br>
具体接口名称可在**ros2 topic list**中查看<br>
例：
>>ros2 topic pub --rate 1 /turtle1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 2.0, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 1.8}}"<br>
>
>变为
>>ros2 topic pub --rate 1 /shuizao/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 2.0, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 1.8}}"