# 一、概述

## 1、数据类型

### 1）ROS中的点云数据类型

在ROS中表示点云的数据结构有：

sensor_msgs::PointCloud

    都是浮点型,不再使用这一类。

<font color=red> sensor_msgs::PointCloud2

pcl::PointCloud<T></font>                        

最常用的就是后两种。

sensor_msgs::PointCloud2：表示任意n维数据。点值可以是任何原始数据类型（int，float，double等），并且可以将消息指定为具有高度和宽度值的“密集”值，从而为数据提供2D结构。

pcl::PointCloud<T>：
PCL库中的核心点云类,此类具有PointCloud2消息类型类似的结构，包括标头。在消息类和点云模板类之间进行转换很简单，并且PCL库中的大多数方法都接受两种类型的对象。不过，除其他原因外，最好在点云处理节点中使用此模板类，而不是与消息对象一起使用，因为您可以将各个点作为对象使用，而不必处理其原始数据。


### 2）确定点云消息的类型
+ sensor_msgs :: PointCloud对象，则它们都是浮点型的。要找出它们的名称，请查看channels（）向量的元素。

+ sensor_msgs :: PointCloud2对象，请查看fields（）向量的元素；否则为空。每个都有一个名称字段和一个数据类型字段。PCL具有用于提取此信息的方法，<io.h>

+ pcl :: PointCloud <T>对象，您可能已经知道字段的类型，因为您知道T是什么。如果这是您未编写的另一个节点发布的主题，则必须查看该节点的来源。PCL具有用于提取此信息的方法，<io.h>

### 3)常见的PointCloud2字段名称

+ x/y/z--点的X/Y/Z笛卡尔坐标（float32）

+ rgb--点上的RGB(24位压缩)颜色 (unit 32)

+ rgba--点处的A-RGB(32位压缩)颜色（uint32）

+ normal_x--点上法线向量的第一个分量(float32)

+ normal_y--点上法线向量的第二个分量(float32)

+ normal_z--点上法线向量的第三个分量(float32)

+ curvature--点上的表面曲率变化估计(float32)

+ j1/j2/j3--点的 第一/二/三 时刻不变(float32)

+ boundary_point--点的边界属性（例如，如果该点位于边界上，则设置为1）(bool)

+ principal_curvature_x--点上主曲率方向的第一个分量(float32)

+  principal_curvature_y--点上主曲率方向的第二个分量(float32)

+ principal_curvature_z--点上主曲率方向的第三个分量(float32)

+ pc1/pc2--点处的主曲率的第一/二分量的大小（float32）

+ pfh--点上的点特征直方图(PFH)特征签名(float32 [])

+ fpfh-点上的快速点特征直方图(FPFH)特征签名(float32 [])


### 4)点云装换

+ sensor_msgs::PointCloud  和   sensor_msgs::PointCloud2之间的转换使用：

    sensor_msgs::convertPointCloud2ToPointCloud 和 sensor_msgs::convertPointCloudToPointCloud2.

+ 关于sensor_msgs::PointCloud2  和  pcl::PointCloud<T>之间的转换使用：

    (来自于pcl_conversions的包，不推荐PCL的包)

    pcl::fromROSMsg 和 pcl::toROSMsg 

### 5）ros::NodeHandle(节点句柄)

#### a、两个用途
    首先,该节点句柄提供了简单的启动和关闭roscpp程序内部的节点

    其次，它提供了额外的命名空间解析层，可以使编写子组件更容易

#### b、自动启动和关闭

ros :: NodeHandle管理内部引用计数，以便启动和关闭节点，如下所示：(nh 就是一个节点句柄)

    ros :: NodeHandle nh;

在创建时，如果内部节点尚未启动，则ros​​ :: NodeHandle将启动该节点。一旦所有ros :: NodeHandle实例都被销毁，该节点将自动关闭。

#### c、命名空间

NodeHandles允许您为其构造函数指定命名空间：

    ros :: NodeHandle nh (“my_namespace”);

这使得与该NodeHandle相关的命名空间为<node_namespace>/my_namespace，而不仅仅是<node_namespace>。

还可以指定父NodeHandle和要追加的命名空间：

    ros :: NodeHandle nh1(“ns1”);
    ros :: NodeHandle nh2(nh1，“ns2”);
<font color=red>这会将nh2放入<node_namespace>/ns1/ns2 命名空间。</font>

#### d、全局名称
    ros :: NodeHandle nh(“/ my_global_namespace”);

<font color=red>不鼓励这样做，因为它阻止节点被推入命名空间（例如通过roslaunch）。但是有时候在代码中使用全局名称会很有用。</font>

#### e、私人名称

使用私有名称比直接使用私有名称（“~name”）调用NodeHandle方法有点琐碎。相反，您必须在私有命名空间内创建一个新的NodeHandle：

    ros :: NodeHandle nh(“~my_private_namespace”);
    ros :: Subscriber sub = nh.subscribe(“my_private_topic”，...);
将订阅<node_name> / my_private_namespace / my_private_topic


### 6)订阅不同的点云消息类型

#### a、准备：
    ros::NodeHandle nh;
    std::string topic = nh.resolveName("point_cloud");
    uint32_t queue_size = 1;

#### b、对于sensor_msgs::PointCloud2 topics:
    // 回调签名
    void callback(const sensor_msgs::PointCloud2ConstPtr&);

    //要创建订阅者，可以执行此操作:
    ros::Subscriber sub = nh.subscribe<sensor_msgs::PointCloud2> (topic, queue_size, callback);

#### c、对于直接接收pcl::PointCloud <T>对象的订阅者：
    // 需要包含pcl ros实用程序
    #include "pcl_ros/point_cloud.h"

    //回调签名，假设您的点是pcl::PointXYZRGB类型：
    void callback(const pcl::PointCloud<pcl::PointXYZRGB>::ConstPtr&);

    // 创建模板订阅者
    ros::Subscriber sub = nh.subscribe<pcl::PointCloud<pcl::PointXYZRGB> > (topic, queue_size, callback);

当使用sensor_msgs::PointCloud2订阅者订阅pcl::PointCloud <T>主题时，反之亦然，两种类型的sensor_msgs :: PointCloud2和pcl :: PointCloud <T>之间的转换（反序列化）由订户即时完成。

