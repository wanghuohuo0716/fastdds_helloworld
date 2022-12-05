# 目录
- [目录](#目录)
- [一、FastDDS和Fast-RTPS区别](#一fastdds和fast-rtps区别)
- [二、FastDDS安装](#二fastdds安装)
  - [2.1 安装包下载](#21-安装包下载)
  - [2.2 安装步骤](#22-安装步骤)
  - [2.3 测试](#23-测试)
  - [2.4 添加环境变量](#24-添加环境变量)
  - [2.5 卸载](#25-卸载)
- [三、FastDDS使用](#三fastdds使用)
  - [3.1 创建CMake工程项目](#31-创建cmake工程项目)
  - [3.2 编写IDL文件](#32-编写idl文件)
    - [3.2.1 定义IDL数据类型](#321-定义idl数据类型)
    - [3.2.2 生成定义数据类型的源代码](#322-生成定义数据类型的源代码)
    - [3.2.3 生成类型源码解析](#323-生成类型源码解析)
  - [3.3 Publisher代码](#33-publisher代码)
    - [3.3.1 头文件](#331-头文件)
      - [3.3.1.1 包含数据类型头文件](#3311-包含数据类型头文件)
      - [3.3.1.2 包含FastDDS头文件](#3312-包含fastdds头文件)
      - [3.3.1.3 命名空间](#3313-命名空间)
    - [3.3.2 创建Publisher类](#332-创建publisher类)
      - [3.3.2.1 类定义与成员变量声明](#3321-类定义与成员变量声明)
      - [3.3.2.2 创建participant](#3322-创建participant)
      - [3.3.2.3 register数据类型并创建topic](#3323-register数据类型并创建topic)
      - [3.3.2.4 创建publisher](#3324-创建publisher)
      - [3.3.2.5 创建writer](#3325-创建writer)
    - [3.3.3 使用Publisher对象发送数据](#333-使用publisher对象发送数据)
      - [3.3.3.1 publish函数](#3331-publish函数)
      - [3.3.3.2  run函数](#3332--run函数)
  - [3.4 Subscriber代码](#34-subscriber代码)
    - [3.4.1 头文件](#341-头文件)
      - [3.4.1.1 包含数据类型头文件](#3411-包含数据类型头文件)
      - [3.4.1.2 包含FastDDS头文件](#3412-包含fastdds头文件)
      - [3.4.1.3 命名空间](#3413-命名空间)
    - [3.4.2 创建Subscriber类](#342-创建subscriber类)
      - [3.4.2.1 类定义与成员变量声明](#3421-类定义与成员变量声明)
      - [3.4.2.2 创建participant](#3422-创建participant)
      - [3.4.2.3 register数据类型并创建topic](#3423-register数据类型并创建topic)
      - [3.4.2.4 创建subscriber](#3424-创建subscriber)
      - [3.4.2.5 创建reader](#3425-创建reader)
    - [3.4.3 使用Subscriber对象接收数据](#343-使用subscriber对象接收数据)
  - [3.5 CMakeList.txt](#35-cmakelisttxt)
    - [3.5.1 新建工程](#351-新建工程)
    - [3.5.2 添加依赖](#352-添加依赖)
    - [3.5.3 生成IDL数据类型的库文件](#353-生成idl数据类型的库文件)
    - [3.5.4 生成可执行文件](#354-生成可执行文件)

# 一、FastDDS和Fast-RTPS区别
eProsima Fast RTPS 是一个高性能的发布订阅框架，使用基于发布者、订阅者和数据主题的解耦模型在分布式系统中共享数据。 Fast RTPS 实现了许多 DDS 规范，例如 DDS 安全性、DDS-XTypes、RPC over DDS、DDS TCP PSM 和现代 C DDS 映射，**但Fast RTPS中一些核心 DDS API 尚未完全符合标准。**

在过去几年中，eProsima 不断致力于新功能、创新工具和标准 API 的开发，以使 Fast RTPS 成为当今最完整的开源 DDS 中间件。

**现在(2020.5)**，eProsima 可以自豪地说，**eProsima Fast RTPS 实施符合 OMG DDS 1.4 和 RTPS 2.2 标准。**

**作为这一刻的象征，Fast RTPS 更名为 Fast DDS！**

以上来源[eProsima官网新闻。](https://www.eprosima.com/index.php/company-all/news/146-fast-rtps-is-now-fast-dds)


# 二、FastDDS安装

## 2.1 安装包下载

FastDDS提供了三种安装方式，分别是`bin`、`Source`、`docker image` 3 种方式。

**推荐使用`bin`安装方式**，FastDDS的`bin`安装方式实际也是**源码编译安装**。

在官网填写信息后，选择`bin`安装方式，进入[下载界面](https://www.eprosima.com/index.php/component/ars/repository/eprosima-fast-dds/eprosima-fast-dds-2-8-1)（该链接不需要填写信息！），下载安装包[eProsima Fast DDS 2.8.1 - Linux (32 & 64)](https://www.eprosima.com/index.php/component/ars/repository/eprosima-fast-dds/eprosima-fast-dds-2-8-1/eprosima_fast-dds-v2-8-1-linux-tgz?format=raw)

![](https://upload-images.jianshu.io/upload_images/20587097-d410c00334905384.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

该安装包有`install.sh`脚本，`install.sh`会自动`apt`安装各种依赖，然后进入`src`目录下，分别从源码构建`foonathan_memory_vendor`、`fastcdr`、`fastrtps`、`fastddsgen`。

![](https://upload-images.jianshu.io/upload_images/20587097-574b17fd7744e78a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

*   `foonathan_memory_vendor`，一个 STL 兼容的 C++ 内存分配器 库。
*   `fastcdr`，一个根据 CDR 标准进行数据序列化的 C++ 库。
*   `fastrtps`，eProsima Fast DDS库的核心库。
*   `fastddsgen`，一个使用 IDL 文件中定义的数据类型生成源代码的 Java 应用程序。

如果这些组件中的任何一个不需要，可以简单地重命名或从`src` 目录中删除。


## 2.2 安装步骤
注意编译需要**CMake3.11以上的版本**，安装编译需要时间比较久，大概15分钟(8代i7编译)。

下载该安装包到`Download`目录后，解压缩至新建文件夹并安装
`cd Download`
`mkdir fastdds2.8.1`
`tar -zxvf eProsima_Fast-DDS-v2.8.1-Linux.tgz -C fastdds2.8.1/`
`sudo ./install.sh`


**注意，如果要测试FastDDS中的`examples`，需要在`install.sh`脚本脚本中打开该选项，默认为`OFF`。**

![](https://upload-images.jianshu.io/upload_images/20587097-eccb792e8c2a5e25.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


`install.sh`脚本首先安装各种依赖，包括`git、build-essential、cmake、libssl-dev、libasio-dev、libtinyxml2-dev、openjdk-8-jre-headless。`

![](https://upload-images.jianshu.io/upload_images/20587097-31c5bc2a12d1b07c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

接着`install.sh`脚本编译安装`foonathan_memory_vendor。`

![](https://upload-images.jianshu.io/upload_images/20587097-a87d9f7312485139.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后`install.sh`脚本编译安装`fastcdr。`

![](https://upload-images.jianshu.io/upload_images/20587097-8da1d3b776129f2d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

随后`install.sh`脚本编译安装`fastrtps(Fast DDS)。`

![](https://upload-images.jianshu.io/upload_images/20587097-ce3626d3e3a43616.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

最后`install.sh`脚本拷贝`fastddsgen`二进制文件到系统目录中。

![](https://upload-images.jianshu.io/upload_images/20587097-4e04e84f88a19a16.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 2.3 测试

* **IDL文件生成接口文件**
```bash
git clone https://github.com/wanghuohuo0716/fastdds_helloworld.git
cd fastdds_helloworld/idl
fastddsgen HelloWorld.idl -d ./../include/idl_generate/ # -d选项指示生成的头文件保存目录
```

* **编译FastDDS的程序**

根据IDL文件生成接口文件后，**同一个终端内接着编译FastDDS程序**。

```bash
cd ..
mkdir build && cd build
cmake .. 
make
```

* **运行Publisher和Subscriber**

```
cd build/
./DDSHelloWorldPublisher
```

```
cd build/
./DDSHelloWorldSubscriber
```

可以看到终端输出

![](https://upload-images.jianshu.io/upload_images/20587097-dc7e04287c177437.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 2.4 添加环境变量

**如果2.3步骤可以正常执行，不需要再手动添加环境变量了。**

---

当使用eProsima Fast DDS运行应用程序实例时，它必须与安装包的库链接，可以临时添加环境变量

`export LD_LIBRARY_PATH=/usr/local/lib/`

也可以永久添加

`echo 'export LD_LIBRARY_PATH=/usr/local/lib/' >> ~/.bashrc`

## 2.5 卸载

该安装包同样提供`/uninstall.sh`卸载脚本，直接执行该脚本即可：

`cd fastddss2.8.1`
`sudo ./uninstall.sh`

**注意：如果任何其他组件已经以某种其他方式安装在系统中，它们也将被删除。为避免这种情况，请在执行之前编辑` ./uninstall.sh`脚本。**

# 三、FastDDS使用

## 3.1 创建CMake工程项目

新建一个CMake工程项目，在`fastdds_helloworld`目录中创建`idl`文件夹、`src`文件夹和`include`文件夹，接着在`include`文件夹中创建`idl_generate`文件夹。

```bash
mkdir -p fastdds_helloworld/src
cd fastdds_helloworld
mkdir idl
mkdir -p include/idl_generate
```

工作区目录结构如下，

![](https://upload-images.jianshu.io/upload_images/20587097-ef5b7ea78bfeabe0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

其中`idl`文件夹存放`idl`文件，`include/idl_generate`文件夹存放`idl`文件生成的头文件。

## 3.2 编写IDL文件
### 3.2.1 定义IDL数据类型

创建IDL文件 

`cd idl && touch HelloWorld.idl`

定义 `HelloWorld` 数据类型，它有两个元素：
* 类型为` uint32_t `的`index`
* 类型为` std::string` 的`message`

```
struct HelloWorld
{
    unsigned long index;
    string message;
};
```

### 3.2.2 生成定义数据类型的源代码

接着利用fastdds的工具`fastddsgen`，生成在 C++中实现此数据类型的源代码，在`idl/` 目录运行以下命令：

`fastddsgen -d ./../include/idl_generate/ HelloWorld.idl`

其中`-d`选项指定生成源代码存放的目录路径`./../include/idl_generate/`。

**`fastddsgen`是一个Java应用程序(已经在前述安装步骤中成功安装)，用来解析idl文件，生成数据类型定义的的源代码。**

### 3.2.3 生成类型源码解析

`fastddsgen`工具会为`HelloWorld.idl`文件生成四个文件，分别是
* `HelloWorld.h`
* `HelloWorld.cxx`
* `HelloWorldPubSubTypes.h`
* `HelloWorldPubSubTypes.cxx`

其中`HelloWorld.h，HelloWorld.cxx`中会**根据idl文件中定义的结构体生成对应的类，即 类型定义**：

```
class HelloWorld
{
public:
    ****
private:
    uint32_t m_index;
    std::string m_message;
};
```

其中`HelloWorldPubSubTypes.h，HelloWorldPubSubTypes.cxx`文件**在定义`HelloWorld`类的基础上**添加了一些了**对HelloWorld 类型的序列化和反序列化代码：**

```cpp
#include "HelloWorld.h"

class HelloWorldPubSubType : public eprosima::fastdds::dds::TopicDataType
{
public:

    typedef HelloWorld type;

    eProsima_user_DllExport HelloWorldPubSubType();
    eProsima_user_DllExport virtual ~HelloWorldPubSubType() override;
    ***
    unsigned char* m_keyBuffer;
};
```

## 3.3 Publisher代码

在**3.1节**的基础上，利用`fastdds`实现一个Publisher还需要以下三个步骤：

* 包含头文件
* 实现`Publisher`的类
* 实例化`Publisher`并发布数据

其中**实现`Publisher`的类**是核心！

### 3.3.1 头文件
#### 3.3.1.1 包含数据类型头文件

包含`HelloWorldPubSubTypes.h `文件，该文件定义了数据类型的序列化和反序列化函数，并且已经包含了数据类型定义的头文件`#include "HelloWorld.h"`。

```
#include "HelloWorldPubSubTypes.h"
```

#### 3.3.1.2 包含FastDDS头文件

这些头文件提供了使用 Fast DDS的API。
```
#include <fastdds/dds/domain/DomainParticipantFactory.hpp>
#include <fastdds/dds/domain/DomainParticipant.hpp>
#include <fastdds/dds/topic/TypeSupport.hpp>
#include <fastdds/dds/publisher/Publisher.hpp>
#include <fastdds/dds/publisher/DataWriter.hpp>
#include <fastdds/dds/publisher/DataWriterListener.hpp>
```

*   `DomainParticipantFactory`：**允许创建和销毁 DomainParticipant 对象。**
*   `DomainParticipant`：**充当所有其他实体对象的容器，并充当发布者、订阅者和主题对象的工厂。**
*   `TypeSupport`：**为participant提供序列化、反序列化和获取特定数据类型的key的函数。**
*   `Publisher`：**是负责创建 DataWriter的对象。**
*   `DataWriter`：**允许应用程序设置要在给定主题下发布的数据的值。**
*   `DataWriterListener`：**允许重新定义 DataWriterListener 的功能。**

#### 3.3.1.3 命名空间

添加在应用程序中使用的` eProsima Fast DDS` 类和函数的命名空间。
```
using namespace eprosima::fastdds::dds;
```

### 3.3.2 创建Publisher类

`fastdds`根据DDS规范提供了**相应概念的类定义**，如`DomainParticipant`，`Publisher`，`Topic`和`DataWriter`等。

在使用这些实体类之前，**需要理解这些实体类之间的关系**，根据下图进行分析。

![](https://upload-images.jianshu.io/upload_images/20587097-5c5af65528de30c3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**`fastdds`对这些DDS概念实例化是分层的**，这些实体之间的关系如下图：

![](https://upload-images.jianshu.io/upload_images/20587097-2369fb94916cf14e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**`fastdds`提供`DomainParticipant`类创建`participant`**

`topic`是由`participant`创建和管理，创建时设置`topic`的名称和绑定对应得`数据类型`以及`Qos`配置。

`datawriter`是由`publisher`创建和管理的，`datawriter`是负责往`topic`上写数据的。`publisher`在创建`datawriter`时会将对应的`topic`绑定到`datawriter`上。

每个participant最多只能拥有一个publisher，而一个publisher可以管理多个datawriter，对于不同的datawriter而言，Publisher是公共的。

#### 3.3.2.1 类定义与成员变量声明

**定义`HelloWorldPublisher`类，并声明类成员变量**

```
class HelloWorldPublisher
{
private:
    HelloWorld hello_;
    DomainParticipant* participant_;
    Publisher* publisher_;
    Topic* topic_;
    DataWriter* writer_;
    TypeSupport type_;
```

*   `DomainParticipant`：**充当所有其他实体对象的容器，并充当发布者、订阅者和主题对象的工厂。**
*   `Publisher`：**是负责创建 DataWriter的对象。**
*   `DataWriter`：**允许应用程序设置要在给定主题下发布的数据的值。**
*   `TypeSupport`：**为participant提供序列化、反序列化和获取特定数据类型的key的函数。**
*   `DataWriterListener`：**允许重新定义 DataWriterListener 的功能。**

#### 3.3.2.2 创建participant

**利用`DomainParticipantFactory`创建`participant`，`participant`所在的`Domain ID`为`0`，采用默认的`participantQos`策略。**

```
DomainParticipantQos participantQos;
participantQos.name("Participant_publisher");
participant_ = DomainParticipantFactory::get_instance()->create_participant(0, participantQos);
```

*   `DomainParticipantFactory`：**允许创建和销毁 DomainParticipant 对象。**

#### 3.3.2.3 register数据类型并创建topic

**`IDL`接口文件中定义的数据类型需要在`participant`中注册，把数据类型注册到participant上，这样`participant`在创建`topic`时，才能使用该数据类型。**

```
type_ = new HelloWorldPubSubType()
type_.register_type(participant_);
topic_ = participant_->create_topic("HelloWorldTopicName", "HelloWorld", TOPIC_QOS_DEFAULT);
```

`participant`根据默认Qos参数`TOPIC_QOS_DEFAULT`创建`topic`。

**`type_`变量的值是`IDL`接口文件中数据类型。**

`type_`是`TypeSupport`类型，可以为`participant`提供序列化、反序列化和获取特定数据类型的key的函数。

#### 3.3.2.4 创建publisher

`participant`根据`PUBLISHER_QOS_DEFAULT`创建`publisher`。

```
publisher_ = participant_->create_publisher(PUBLISHER_QOS_DEFAULT, nullptr);
```

#### 3.3.2.5 创建writer

`publisher`根据`DATAWRITER_QOS_DEFAULT`通过先前创建的`listener_`进而创建`datawtriter`。

```
writer_ = publisher_->create_datawriter(topic_, DATAWRITER_QOS_DEFAULT, &listener_);
```

### 3.3.3 使用Publisher对象发送数据

`HelloWorldPublisher`类需要定义`publish`和`run`两个函数，前者是将数据发布出去，后者可以限定指定条件，如定时发送等，这样设计更加利于程序后期的拓展性。

#### 3.3.3.1 publish函数

`publisher`发布数据实际是调用`datawriter`的`write`函数将数据发送出去。

```
bool publish()
{
    hello_.index(hello_.index() + 1);
    if (writer_->write(&hello_))
        return true;
    return false;
}
```

#### 3.3.3.2  run函数

```
void run(uint32_t samples)
{
    uint32_t samples_sent = 0;
    while (samples_sent < samples)
    {
        if (publish())
        {
            samples_sent++;
            std::cout << "Message: " << hello_.message() << " with index: " << hello_.index()
                        << " SENT" << std::endl;
        }
        std::this_thread::sleep_for(std::chrono::milliseconds(1000));
    }
}
```

上述的demo代码有两个缺陷，data是private，外部无法修改变量。
应该在publish函数中接收更新后的数据然后发布出去。
publisher类中是否要定义数据类型还是仅仅定义类型的指针？？？

PubListener这个类中的matched_表示与该datawriter通过topic建立连接的subscriber的数量，当新建连接或断开连接时，就会触发on_publication_matched()回调函数，demo中利用该回调打印输出，提示连接的建立与否。

可以通过matched_参数判断是否有DataReader建立连接，若有才publish，没有就不发布。



程序结束时，清理内存，销毁的时候也是分层，从下往上，先销毁datawriter，然后是publisher以及同级的topic，最后是participant




## 3.4 Subscriber代码

### 3.4.1 头文件
#### 3.4.1.1 包含数据类型头文件

包含`HelloWorldPubSubTypes.h `文件，该文件定义了数据类型的序列化和反序列化函数，并且已经包含了数据类型定义的头文件`#include "HelloWorld.h"`。

```
#include "HelloWorldPubSubTypes.h"
```

#### 3.4.1.2 包含FastDDS头文件

这些头文件提供了使用 Fast DDS的API。
```
#include <fastdds/dds/domain/DomainParticipantFactory.hpp>
#include <fastdds/dds/domain/DomainParticipant.hpp>
#include <fastdds/dds/topic/TypeSupport.hpp>
#include <fastdds/dds/subscriber/Subscriber.hpp>
#include <fastdds/dds/subscriber/DataReader.hpp>
#include <fastdds/dds/subscriber/DataReaderListener.hpp>
#include <fastdds/dds/subscriber/qos/DataReaderQos.hpp>
#include <fastdds/dds/subscriber/SampleInfo.hpp>
```

*   `DomainParticipantFactory`：**允许创建和销毁 DomainParticipant 对象。**
*   `DomainParticipant`：**充当所有其他实体对象的容器，并充当发布者、订阅者和主题对象的工厂。**
*   `TypeSupport`：**为participant提供序列化、反序列化和获取特定数据类型的key的函数。**
*   `Subscriber`：**它是负责创建和配置 DataReader 的对象。**
*   `DataReader`：**它是负责实际接收数据的对象。它在应用程序中注册标识要读取的数据的主题(TopicDescription)，并访问订阅者接收到的数据。**
*   `DataReaderListener`：**这是分配给数据读取器的侦听器。**
*   `DataReaderQoS`：**定义 DataReader 的 QoS 的结构。**
*   `SampleInfo`：**“read”或“taken”的是每个`SampleInfo`附带的信息。**

#### 3.4.1.3 命名空间

添加在应用程序中使用的` eProsima Fast DDS` 类和函数的命名空间。
```
using namespace eprosima::fastdds::dds;
```


### 3.4.2 创建Subscriber类

与Publisher类类似。

`fastdds`根据DDS规范提供了**相应概念的类定义**，如`DomainParticipant`，`Subscriber`，`Topic`和`DataReader`等。

在使用这些实体类之前，**需要理解这些实体类之间的关系**，根据下图进行分析。

![](https://upload-images.jianshu.io/upload_images/20587097-7e735b9017cc20dd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**`fastdds`对这些DDS概念实例化是分层的**，这些实体之间的关系如下图：

![](https://upload-images.jianshu.io/upload_images/20587097-5fd72fd1c79a0827.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**`fastdds`提供`DomainParticipant`类创建`participant`**

`topic`是由`participant`创建和管理，创建时设置`topic`的名称和绑定对应得`数据类型`以及`Qos`配置。

`datareader`是由`subscriber`创建和管理的，`datareader`是负责读`topic`上的数据。`subscriber`在创建`datareader`时会将对应的`topic`绑定到`datareader`上。

每个participant最多只能拥有一个subscriber，而一个subscriber可以管理多个datareader，对于不同的datareader而言，subscriber是公共的。

#### 3.4.2.1 类定义与成员变量声明

**定义`HelloWorldSubscriber`类，并声明类成员变量**

```
class HelloWorldSubscriber
{
private:
    DomainParticipant* participant_;
    Subscriber* subscriber_;
    Topic* topic_;
    DataReader* reader_;
    TypeSupport type_;
    class SubListener : public DataReaderListener
    {
        HelloWorld hello_;
        ***
    } listener_;
    ***
}
```

*   `DomainParticipant`：**充当所有其他实体对象的容器，并充当发布者、订阅者和主题对象的工厂。**
*   `Subscriber`：**是负责创建 DataReader 的对象。**
*   `DataReader`：**它是负责实际接收数据的对象。**
*   `TypeSupport`：**为participant提供序列化、反序列化和获取特定数据类型的key的函数。**
*   `DataReaderListener`：**这是分配给数据读取器的侦听器。**

注意：这里的`HelloWorld`数据类型变量的声明是在`SubListener`子类中定义的，并不是在`HelloWorldSubscriber`类中定义。

上述变量声明中最核心的是声明了一个`listener_`变量，该变量是`SubListener`子类的实例化对象。

**`SubListener`子类公共继承`DataReaderListener`类，从而可以在`datareader`和`datawriter`建立连接以及接收到新数据等事件时触发相应的回调函数，进而在`SubListener`子类内部执行相应的处理逻辑。**

#### 3.4.2.2 创建participant

**利用`DomainParticipantFactory`创建`participant`，`participant`所在的`Domain ID`为`0`，采用默认的`participantQos`策略。**

```
DomainParticipantQos participantQos;
participantQos.name("Participant_subscriber");
participant_ = DomainParticipantFactory::get_instance()->create_participant(0, participantQos);
```

#### 3.4.2.3 register数据类型并创建topic

**`IDL`接口文件中定义的数据类型需要在`participant`中注册，把数据类型注册到participant上，这样`participant`在创建`topic`时，才能使用该数据类型。**

```
type_ = new HelloWorldPubSubType()
type_.register_type(participant_);
topic_ = participant_->create_topic("HelloWorldTopicName", "HelloWorld", TOPIC_QOS_DEFAULT);
```


#### 3.4.2.4 创建subscriber
`participant`根据`SUBSCRIBER_QOS_DEFAULT`创建`subscriber`。

```
subscriber_ = participant_->create_subscriber(SUBSCRIBER_QOS_DEFAULT, nullptr);
```

#### 3.4.2.5 创建reader
`subscriber`根据`DATAREADER_QOS_DEFAULT`通过先前创建的`listener_`进而创建`datareader`。

```
reader_ = subscriber_->create_datareader(topic_, DATAREADER_QOS_DEFAULT, &listener_);
```

### 3.4.3 使用Subscriber对象接收数据
```
class HelloWorldSubscriber
{
private:
    ***
    class SubListener : public DataReaderListener
    {
        HelloWorld hello_;
        ***
    } listener_;
    ***
}
```

上述变量声明中最核心的是声明了一个`listener_`变量，该变量是`SubListener`子类的实例化对象。

**`SubListener`子类公共继承`DataReaderListener`类，从而可以在`datareader`和`datawriter`建立连接以及接收到新数据等事件时触发相应的回调函数，进而在`SubListener`子类内部执行相应的处理逻辑。**

`SubListener`子类的第一个**覆盖(override)**回调函数是 `on_subscription_matched`，当`datareader`和`datawriter`建立连接时，会触发该回调函数。

```
        void on_subscription_matched(DataReader*, const SubscriptionMatchedStatus& info) override
        {
            if (info.current_count_change == 1)
            {
                std::cout << "Subscriber matched." << std::endl;
            }
            else if (info.current_count_change == -1)
            {
                std::cout << "Subscriber unmatched." << std::endl;
            }
            else
            {
                std::cout << info.current_count_change
                        << " is not a valid value for SubscriptionMatchedStatus current count change" << std::endl;
            }
        }
```

第二个**覆盖(override)**回调函数是` on_data_available`，当`datareader`接收到`datawriter`发送的新数据时，会触发该回调函数。 
```
        void on_data_available(DataReader* reader) override
        {
            SampleInfo info;
            if (reader->take_next_sample(&hello_, &info) == ReturnCode_t::RETCODE_OK)
            {
                if (info.valid_data)
                {
                    samples_++;
                    std::cout << "Message: " << hello_.message() << " with index: " << hello_.index()
                                << " RECEIVED." << std::endl;
                }
            }
        }
```

这里定义了` SampleInfo `类的对象`info`，它确定新收到的数据状态，如数据是否已被读取或采集。

## 3.5 CMakeList.txt

### 3.5.1 新建工程
```cmake
cmake_minimum_required(VERSION 3.12.4)
project(DDSHelloWorld)
add_compile_options(-std=c++11)
```

在官方给的demo示例中，添加了C++版本至少为11，实际测试中去掉此项仍可正常编译通过。

### 3.5.2 添加依赖
```cmake
find_package(fastcdr REQUIRED)
find_package(fastrtps REQUIRED)
```

使用FastDDS只需要依赖`fastcdr`和`fastrtps`两个库即可，这两个库根据前面的安装步骤，安装在了`
/usr/local/share`下。

### 3.5.3 生成IDL数据类型的库文件
```cmake
file(GLOB DDS_HELLOWORLD_SOURCES_CXX "./include/idl_generate/*.cxx")
add_library(HelloWorld_IDL_lib ${DDS_HELLOWORLD_SOURCES_CXX})
```
利用CMake中的`file`命令，将数据类型的源码文件列表保存在CMake变量中，然后将`fastddsgen`生成的这些数据类型源码编译生成静态库文件。

**将数据类型单独编译，能解耦可执行文件编译时源码和类型文件一起编译，有助于提高编译效率，仅需要在运行时链接数据类型的库文件即可。**

### 3.5.4 生成可执行文件
```cmake
# Publisher
add_executable(DDSHelloWorldPublisher src/HelloWorldPublisher.cpp)
target_link_libraries(DDSHelloWorldPublisher 
    HelloWorld_IDL_lib
    fastrtps 
    fastcdr
)

# Subscriber
add_executable(DDSHelloWorldSubscriber src/HelloWorldSubscriber.cpp)
target_link_libraries(DDSHelloWorldSubscriber     
    HelloWorld_IDL_lib    
    fastrtps 
    fastcdr
)
```

生成`Publisher`和`Subscriber`可执行文件。

参考：
**官方C++版helloworld**：https://fast-dds.docs.eprosima.com/en/v2.3.3/fastdds/getting_started/simple_app/simple_app.html
**官方python版helloworld**：https://fast-dds.docs.eprosima.com/en/v2.3.3/fastddsgen/pubsub_app/pubsub_app.html#fastddsgen-pubsub-app
官方快速开始教程：https://fast-dds.docs.eprosima.com/en/latest/fastdds/getting_started/getting_started.html
**官方二进制安装教程**：https://fast-dds.docs.eprosima.com/en/latest/installation/binaries/binaries_linux.html
**官方代码helloworld示例**：https://github.com/eProsima/Fast-DDS-docs/blob/master/code/Examples
鱼香ros的cmake工程：https://github.com/fishros/dds_tutorial
静态编译的cmake工程：https://blog.csdn.net/briblue/article/details/124081170
fastdds的helloworld代码解释：https://blog.csdn.net/u012739527/article/details/124705821
eProsima Fast DDS-Gen介绍：https://fast-dds.docs.eprosima.com/en/latest/fastddsgen/introduction/introduction.html


待做：
fastdds如何通过同一个类使用不同类型的数据接口，利用模板类？
是否已有一些基本类型可供fastdds的程序使用，不用自己新建idl文件？
fastdds的subscriber必须在listener中编写回调函数么，不能像ROS一样，在创建topic时绑定回调函数？
动态 HelloWorld 示例学习：https://fast-dds.docs.eprosima.com/en/latest/fastdds/dynamic_types/examples.html
动态主题类型：https://fast-dds.docs.eprosima.com/en/latest/fastdds/dynamic_types/dynamic_types.html
