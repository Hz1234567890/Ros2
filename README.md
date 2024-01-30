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

！[演示效果](./images/Ros测试.png)