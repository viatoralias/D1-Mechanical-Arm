# Basic Control

>The SDK and sample programs for D1 can be downloaded [here](https://unitree-firmware.oss-cn-hangzhou.aliyuncs.com/tool/d1_sdk.zip).

> After the download is complete, create a folder in it, and use cmake to build and make to compile. The following pices of code are the one used for controlling the arm:

> In this markdown file, we give examples for each program use using your prefered IDE:

## 1. Single joint Angle control of mechanical arm

Using the rt/arm_Command topic, assume that the generated seq value is 4 and set the Angle of joint 5 to 60. The data content for ```json {" seq ": 4," address ": 1," code ": 1," data ": {" id" : 5, "Angle" : 60, "delay_ms" : 0}}``` is:  
```cpp
#include <unitree/robot/channel/channel_publisher.hpp>
#include <unitree/common/time/time_tool.hpp>
#include "msg/ArmString_.hpp"

#define TOPIC "rt/arm_Command"

using namespace unitree::robot;
using namespace unitree::common;

int main()
{
    ChannelFactory::Instance()->Init(0);
    ChannelPublisher<unitree_arm::msg::dds_::ArmString_> publisher(TOPIC);
    publisher.InitChannel();

    unitree_arm::msg::dds_::ArmString_ msg{};
    msg.data_() = "{\"seq\":4,\"address\":1,\"funcode\":1,\"data\":{\"id\":5,\"angle\":60,\"delay_ms\":0}}";
    publisher.Write(msg);
 
    return 0;
}
```
## 2. Multi-joint Angle control of mechanical arm

Using the rt/arm_Command topic, it is assumed that the generated seq value is 4 and the joint Angle of the manipulator arm is ```json {0, -60, 60, 0, 30, 0, 0, 0}. The data content for {" seq ": 4," address ": 1," funcode ": 2," data ": {" mode" : 1, "angle0" : 0, "angle1" : to 60, "angle2" : 60, "angle3" : 0, "angle4" : 30, "ang le5":0,"angle6":0}}```:
```cpp
#include <unitree/robot/channel/channel_publisher.hpp>
#include <unitree/common/time/time_tool.hpp>
#include "msg/ArmString_.hpp"

#define TOPIC "rt/arm_Command"

using namespace unitree::robot;
using namespace unitree::common;

int main()
{
    ChannelFactory::Instance()->Init(0);
    ChannelPublisher<unitree_arm::msg::dds_::ArmString_> publisher(TOPIC);
    publisher.InitChannel();

    unitree_arm::msg::dds_::ArmString_ msg{};
    msg.data_() = "{\"seq\":4,\"address\":1,\"funcode\":2,\"data\":{\"mode\":1,\"angle0\":0,\"angle1\":-60,\"angle2\":60,\"angle3\":0,\"angle4\":30,\"angle5\":0,\"angle6\":0}}";
    publisher.Write(msg);
 
    return 0;
}
```

## 3. Enable/unload force control of mechanical arm joint motor
Through the rt/arm_Command topic, assuming that the seq value generated at this time is 4, control all joint unloading force. The data content for ```json {" seq ": 4," address ": 1," funcode ": 5," data ": {" mode" : 0}}```
```cpp
#include <unitree/robot/channel/channel_publisher.hpp>
#include <unitree/common/time/time_tool.hpp>
#include "msg/ArmString_.hpp"

#define TOPIC "rt/arm_Command"

using namespace unitree::robot;
using namespace unitree::common;

int main()
{
    ChannelFactory::Instance()->Init(0);
    ChannelPublisher<unitree_arm::msg::dds_::ArmString_> publisher(TOPIC);
    publisher.InitChannel();

    unitree_arm::msg::dds_::ArmString_ msg{};
    msg.data_() = "{\"seq\":4,\"address\":1,\"funcode\":5,\"data\":{\"mode\":0}}";
    publisher.Write(msg);
 
    return 0;
}
```

## 4. Robotic arm position and posture return to zero

Through the rt/arm_Command topic, assuming that the seq value generated at this time is 4, the manipulator is controlled to return to zero position. The data content is' ```json {"seq":4,"address":1,"funcode":7}``` '
```cpp
#include <unitree/robot/channel/channel_publisher.hpp>
#include <unitree/common/time/time_tool.hpp>
#include "msg/ArmString_.hpp"

#define TOPIC "rt/arm_Command"

using namespace unitree::robot;
using namespace unitree::common;

int main()
{
    ChannelFactory::Instance()->Init(0);
    ChannelPublisher<unitree_arm::msg::dds_::ArmString_> publisher(TOPIC);
    publisher.InitChannel();

    unitree_arm::msg::dds_::ArmString_ msg{};
    msg.data_() = "{\"seq\":4,\"address\":1,\"funcode\":7}";
    publisher.Write(msg);
 
    return 0;
}
```

## 5. Obtain real-time joint Angle

The seq value of the instruction is fixed at 10, the upload frequency is 10Hz, and the data is distinguished by the address address +code function code, which requires callback monitoring.
```cpp
#include <unitree/robot/channel/channel_subscriber.hpp>
#include <unitree/common/time/time_tool.hpp>
#include "msg/PubServoInfo_.hpp"
#include "msg/ArmString_.hpp"

#define TOPIC "current_servo_angle"
#define TOPIC1 "arm_Feedback"

using namespace unitree::robot;
using namespace unitree::common;

void Handler(const void* msg)
{
    const unitree_arm::msg::dds_::PubServoInfo_* pm = (const unitree_arm::msg::dds_::PubServoInfo_*)msg;

    std::cout << "servo0_data:" << pm->servo0_data_() << ", servo1_data:" << pm->servo1_data_() << ", servo2_data:" << pm->servo2_data_()<< ", servo3_data:" << pm->servo3_data_()<< ", servo4_data:" << pm->servo4_data_()<< ", servo5_data:" << pm->servo5_data_()<< ", servo6_data:" << pm->servo6_data_() << std::endl;
}

void Handler1(const void* msg)
{
    const unitree_arm::msg::dds_::ArmString_* pm = (const unitree_arm::msg::dds_::ArmString_*)msg;

    std::cout << "armFeedback_data:" << pm->data_() << std::endl;
}

int main()
{
    ChannelFactory::Instance()->Init(0);
    ChannelSubscriber<unitree_arm::msg::dds_::PubServoInfo_> subscriber(TOPIC);
    subscriber.InitChannel(Handler);

    ChannelSubscriber<unitree_arm::msg::dds_::ArmString_> subscriber1(TOPIC1);
    subscriber1.InitChannel(Handler1);

    while (true)
    {
        sleep(10);
    }

    return 0;
}
```
