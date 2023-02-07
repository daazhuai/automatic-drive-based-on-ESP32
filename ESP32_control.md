## 作业要求

2021级（小组完成，每组不能超过4人）：
1.研读小车控制板PCB原理图，用Arduino C++完成驱动开发，能实现摄像头视频及照片读取（需调整相机参数）、Wifi热点开启、小车拐弯、直线行走、倒车等动作，每个动作封装为一个函数（30%）
2.电脑用Python与小车控制板连接，电脑做上位机，用参数和键盘实现控制小车在地图上行走，并对地图拍照，存储在SD卡上（30%）
3.使用百度EasyDL，对地图照片进行道路标注，训练一个EasyDL道路分割深度学习模型，生成一个云API，结合本地电脑、小车控制板、小车摄像头，实现小车在地图道路中自动行走（30%）
4.用Arduino在小车控制板上实现Web服务，做一个网页，用手机浏览器可以操控小车（10%）

截止日期：8月28日晚，说明文档需非常详细，文档代码及相关资料上传个人Github，B站视频发QQ实验班公告群。



## 开发环境搭建

### Arduino(C)

1 Arduino下载安装

2 串口驱动

**USB转串口**实现计算机USB接口到物理串口之间的转换：为没有串口的计算机增加串口，等于将传统的串口设备变成了USB设备

> 实现USB转串口：
>
> ① 开发板：USB转串口芯片
>
> ② 电脑：USB转串口驱动
>
> 注：有些USB转串口芯片是免驱动的；Win10系统自带部分驱动

软件中：

① 串口  选择连接串口：检测端口（此电脑（右键）→ 管理 → 硬件管理器 → 端口）

② 波特率  串口监视器波特率设置为115200

（程序中通过Serial.begin启动串口通信后可以在串口监视器中看到对应信息）

3 Arduino 环境主页面（开发板&库文件）

（1）开发板配置

文件 --- 首选项

<img src="C:\Users\liufangfei\AppData\Roaming\Typora\typora-user-images\image-20220813085803518.png" alt="image-20220813085803518" style="zoom:80%;" />

点开地址，将AppData-Local-Arduino15中的文件全部复制到该地址目录中

（2）免安装开发板环境/库文件配置：

库文件位置：C盘 → Program Files(x86) → Arduino → libraries（所有头文件所需库应该放到libraries内）

----------------

免安装环境：

将下载好的Arduino、APPData两个文件放到本机C盘对应的文件夹中，注意需要显示隐藏文件

> 文件目录：
>
> 将Arduino文件放置在`C:\Program Files (x86)`中
>
> 将APPData中的Arduino15文件放置在`C:\Users\liufangfei\AppData\Local`中
>
> >引航计划提供的资料：
> >
> >下载地址：https://aistudio.baidu.com/aistudio/datasetdetail/162528
> >
> >视频教程（前3min30s）：
> >
> >https://www.bilibili.com/video/BV1hN4y1T7H8?spm_id_from=333.999.0.0&vd_source=4172983847c80a53fb2e62caed743dc8

开发环境中：

① 开发板：工具 → 开发板 → 选择开发板 AI Thinker ESP32 CAM

② 串口：工具 → 串口 → 选择串口，使用串口监视器（右上方🔍）调试

> **问题：**
>
> 1.串口占用：
>
> 注册表编辑器中查看占用串口的软件，解除程序占用
>
> 2.环境变量：
>
> 报错：exec："cmd.exe"：executable file not found in %path%
>
> 解决方式：此电脑 → 右键 → 属性 → 搜索环境变量：在系统环境变量中添加cmd.exe所在路径
>
> > https://zhidao.baidu.com/question/1903702886412298020.html

### Thonny(Python)

1.需下载的软件

① Thonny https://thonny.org/

② python3 （安装pycharm时已安装）

③ esp32-micropython固件 https://micropython.org/download/esp32/

④ 开发板对应的串口驱动（Win10自带）

2.安装esptool

工具 → 管理插件 → 搜索esptool并安装，或运行cmd脚本（在已有python3&pip的条件下）：

```cmd
pip install esptool
```

3.固件擦除并更新

① 工具 → 设置 → 解释器 → 选择Micropython(ESP32)

② 在Port or WebREPL 中选择对应的端口号。

③ 点击 install or update firmare：

​     Port选择端口号，Firemare 选择 步骤一中3 所下载的micropython固件包，选中 Erase flash before installing 然后点击安装

> 串口占用问题：
>
> > 固件擦除步骤即为了清楚固件中原有的py程序，解决串口占用问题
>
> 开始菜单下搜索并打开注册表编辑器（regedit），查看路径如下
>
> ```
> 计算机\HKEY_LOCAL_MACHINE\HARDWARE\DEVICEMAP\SERIALCOMM
> ```
>
> 在右侧ComDb右键选择删除：让系统重新构建COM端口的列表，更改那些端口被占用的状态
>
> > https://xinzhi.wenda.so.com/m/a/1523288061202474?ivk_sa=1024320u



## Arduino基础语法

> [【Arduino】基本语法梳理_陌湘萘的博客-CSDN博客](https://blog.csdn.net/sinat_28782331/article/details/85866157)

define

> \#define 在编译之前给常量命名
>
> 在Arduino中，定义的常量不会占用芯片上的任何程序内存空间，编译时编译器会用事先定义的值来取代这些常量。
>
> 然而这样做会产生一些副作用，例如，一个已被定义的常量名已经包含在了其他常量名或者变量名中。在这种情况下，文本将被＃defined 定义的数字或文本所取代。

const

```c
//const type name
//const 和 type 都是用来修饰变量的，它们的位置可以互换
//由于常量一旦被创建后其值就不能再改变，所以常量必须在定义的同时赋值（初始化），后面的任何赋值行为都将引发错误。

const int Led = 32
```

> define&const定义常量的区别：[const与#define的区别、优点 - 腾讯云开发者社区-腾讯云 (tencent.com)](https://cloud.tencent.com/developer/article/1496003#:~:text=const定义常量从汇编的角度来看，只是给出了对应的内存地址，而不是象%23define一样给出的是立即数，所以，const定义的常量在程序运行过程中只有一份拷贝，而,%23define定义的常量在内存中有若干个拷贝。 const常量可以进行调试的。)

```
// 数字信号引脚函数
pinMode();
digitalRead();
digitalWrite();
```

```c
// 任务1：简易台灯
// 按钮控制灯泡亮灭（长按长亮，不按则灭）
const int an34 = 34;
const int LED32 = 32;
void setup() {
  Serial.begin(115200);
  pinMode(an34,INPUT);
  pinMode(LED32,OUTPUT);
}

void loop() {
  Serial.println(digitalRead(an34));
  int con = digitalRead(an34);
  digitalWrite(LED32,con);
}

// 任务2：单个按钮完整台灯
// （一按一亮/一按一灭）
const int LED32 = 32;
const int an34 = 34;
boolean sig = true;
void setup() {
  pinMode(LED32,OUTPUT);
  pinMode(an34,INPUT);
}

void loop() {
  while(digitalRead(an34)==HIGH){};
  delay(300);
  if(sig == true){
    digitalWrite(LED32,HIGH);
    sig = !sig;
    }
  else{
    digitalWrite(LED32,LOW);
    sig = !sig;
    }
}

// 任务3：双按钮完整台灯
// 两个按钮分别控制灯泡亮灭（一按一亮/一按一灭）
const int LED32 = 32;
const int LED33 = 33;
const int an34 = 34;
const int an35 = 35;
boolean sig1 = true;
boolean sig2 = true;

void setup() {
  pinMode(LED32,OUTPUT);
  pinMode(an34,INPUT);
  pinMode(LED33,OUTPUT);
  pinMode(an35,INPUT);
}

void loop() {
  while(digitalRead(an34)==HIGH && digitalRead(an35)==HIGH){};
  if(digitalRead(an34)==LOW && sig1==true){
    delay(300);
    digitalWrite(LED32,HIGH);
    sig1 = !sig1;
    }
  if(digitalRead(an34)==LOW && sig1==false){
    delay(300);
    digitalWrite(LED32,LOW);
    sig1 = !sig1;
    }
  if(digitalRead(an35)==LOW && sig2==true){
    delay(300);
    digitalWrite(LED33,HIGH);
    sig2 = !sig2;
    }
  if(digitalRead(an35)==LOW && sig2==false){
    delay(300);
    digitalWrite(LED33,LOW);
    sig2 = !sig2;
    }
}
```

## 硬件调试

### PWM信号

构成PWM信号的组件：

① 频率：每秒钟电平变化的次数，决定每秒周期数（高-低-高为一个周期），频率越高电流越平滑

② 分辨率：决定精度（占空比取值范围）

③ 占空比：一个脉冲周期内高电平的所整个周期占的比例

每种PWM信号通过一个信号通道传递



在ESP32上有一个LEDC外设模块专用于输出PWM（脉冲宽度调制）波形

LEDC外设模块可以用于输出可以生成16路独立的数字波形，波形的周期和占空比分辨率可以配置

> PWM（脉冲宽度调制）→ 不同通道设置不同频率/分辨率 → 输出信号到外接LED、电机、舵机等

```c
//ledcSetup()函数
//功能为设置LEDC通道对应的频率和计数位数（占空比分辨率）

//其第一个参数chan表示通道号，取值为0~15,即可设置16个通道,其中高速通道(0~7)由80MHz时钟驱动,低速通道(8~15)由1MHz时钟驱动；
//第二个参数freq为期望设置的频率；
//第三个参数为占空比分辨率的计数位数，其取值为0~20
//（该值决定后面 ledcWrite 方法中占空比可写值，比如该值写10，则占空比最大可写1023 即(1<<bit_num)-1
double ledcSetup(uint8_t chan, double freq, uint8_t bit_num)

//ledcAttachPin()
//功能为将指定的LEDC通道绑定到指定的IO口上以实现PWM的输出

//其第一个参数pin表示我们需要输出的IO口，第二个参数channel为我们指定的LEDC通道
void ledcAttachPin(uint8_t pin, uint8_t channel);

//ledcWrite()函数功能为指定的LEDC通道的输出占空比

//第一个参数chan为指定的LEDC通道，第二个参数duty表示占空比，其取值范围与ledcSetup()函数的bit_num有关
void ledcWrite(uint8_t chan, uint32_t duty)
```

```C
const int led32 = 32;
const int freq = 1000;
const int r = 8;
const int rate = 0;
const int road1 = 0;

void setup() {
  // 将频率和分辨率配置到通道0
  ledcSetup(road1,freq,r);
  // 将引脚绑定到通道0
  ledcAttachPin(led32,road1);

}

void loop() {
  ledcWrite(road1,rate);
  int t = rate;
  for(int i=0; i <= 255; i++){
    t++;
    ledcWrite(road1,t);
    delay(50);
    }
  for(int i=0; i <= 255; i++){
    t--;
    ledcWrite(road1,t);
    delay(50);
    }
}
```

mcpwm：代替ledc作电机的pwm驱动

ESP32有两个mcpwm单元，每一个mcpwm单元有三个定时器，每个定时器有两个引脚

> https://blog.csdn.net/qqliuzhitong/article/details/121510050

```C
// 初始化：将26脚绑定到0号单元0号定时器A脚
mcpwm_gpio_init(MCPWM_UNIT_0,MCPWM0A,26);
mcpwm_gpio_init(MCPWM_UNIT_0,MCPWM0B,27)
mcpwm_config_t motor_pwm_config = {
    // 频率
    .frequency = 1000,
    // A、B脚占空比
    .cmpr_a = 0,
    .cmpr_b = 0,
    // 设置占空比类型为高电平占比
    .duty_mode = MCPWM_DUTY_MODE_0,
    .counter_mode = MCPWM_UP_COUNTER,
  };
// 调用mcpwm_init使配置生效
esp_err = mcpwm_init(MCPWM_UNIT_0, MCPWM_TIMER_0, &motor_pwm_config);
// 设置单元-定时器-通道占空比
mcpwm_set_duty(MCPWM_UNIT_0, MCPWM_TIMER_0, MCPWM_OPR_A, 60);
mcpwm_set_duty(MCPWM_UNIT_0, MCPWM_TIMER_0, MCPWM_OPR_B, 0);
// 开启/关闭定时器
mcpwm_start(MCPWM_UNIT_0, MCPWM_TIMER_0);
mcpwm_stop(MCPWM_UNIT_0, MCPWM_TIMER_0);
```

### 电机

**电机控制**

<img src="C:\Users\liufangfei\AppData\Roaming\Typora\typora-user-images\image-20220813092816054.png" alt="image-20220813092816054" style="zoom:80%;" />

永磁体磁场 --- 通电线圈磁场 → 产生作用力

电流正入反出/正出反入 → 电机正转/反转 → 小车前进/后退

<img src="C:\Users\liufangfei\AppData\Roaming\Typora\typora-user-images\image-20220813092541171.png" alt="image-20220813092541171" style="zoom:80%;" />

接线：VCC、GND、A（GPIO26）、B（GPIO27）（A、B控制转向）电池为电机直接供电（7.4V）

### 舵机（伺服电机）

**舵机控制**

<img src="C:\Users\liufangfei\AppData\Roaming\Typora\typora-user-images\image-20220813092449574.png" alt="image-20220813092449574" style="zoom:67%;" />

GPIO13接M996舵机 --- 控制小车转向

舵机需用PWM信号驱动，频率固定为50（1s50个周期，每周期20ms）

左右极限：

高电平0.5ms，低电平19.5ms → 最左位置

高电平1.5ms，低电平18.5ms → 最右位置

每周期高电平占0.5ms-2.5ms，对应旋转角度0°-180°

```C
// 1.舵机初始化到90°位置

// 2.舵机缓慢由0°转至180°
```

-------------

**小车综合动作实验**

```C
const int servo=15;
const int motorA=26;
const int motorB=27;
//PWM
const int freq=50;
const int resolution=8;
const int servo_channel=0;
const int motorA_channel=1;
const int motorB_channel=2;

void setup() {
  //舵机
  ledcSetup(servo_channel,freq,resolution);
  ledcAttachPin(servo,servo_channel);
  //电机A
  ledcSetup(motorA_channel,freq,resolution);
  ledcAttachPin(motorA,motorA_channel);
  //电机B
  ledcSetup(motorB_channel,freq,resolution);
  ledcAttachPin(motorB,motorB_channel);
  //初始化
  ledcWrite(servo_channel,20);
  ledcWrite(motorA_channel,0);
  ledcWrite(motorB_channel,0);
}

void loop() {
  while(1){
    ledcWrite(motorA_channel,128);
    delay(3000);
    ledcWrite(servo_channel,7);
    delay(1000);
    ledcWrite(motorA_channel,128);
    delay(3000);
    ledcWrite(servo_channel,20);
    delay(1000);
    ledcWrite(motorA_channel,128);
    delay(3000);
    ledcWrite(servo_channel,32);
    delay(1000);
    ledcWrite(motorA_channel,128);
    delay(3000);
    ledcWrite(servo_channel,20);
    delay(1000);
    }
}
```

### 网络摄像头

摄像头安装：

https://www.bilibili.com/video/BV1PY4y1A77U/?spm_id_from=333.788&vd_source=4172983847c80a53fb2e62caed743dc8

WiFi调试：

① 将网页控制LED程序烧入主板（需要开启ESP32模块的WIFI热点供摄像头连接）

② 将IP_Camera程序烧入摄像头（烧写前拔出摄像头SD卡，防止串口占用）

上电 主板复位 摄像头复位 查看摄像头串口消息

> MCU端开启局域网热点
>
> 摄像头、电脑连接局域网热点
>
> 使用电脑访问摄像头网页（摄像头内烧写的程序生成的网页），查看实时摄像结果

### 测速针

主板接测速针，作网关开启WiFi，启动WebServer

PC端接入主板局域网，向主板发送HTTP请求（.py文件），查看测速针读数

### imu

主板接imu，作网关开启WiFi，启动WebServer

PC端接入主板局域网，向主板发送HTTP请求（.py文件），查看imu读数

> 采用串口调试
>
> 复位后保持imu位置不动，请求一次读数；（初始位置角度）
>
> 转动，再一次请求读数（转动后角度）

> 报错：A fatal error occurred: Failed to connect to ESP32: Timed out waiting for packet header
>
> ① connected时按住boot脚
>
> ② 拔掉外接模块烧录后再接入（外接模块可能占用串口）

## 小车综合控制-地图行进拍摄

### IP_Camera端

#### 1. 头文件：库引入

```C
// STEP1：头文件：库引入

#include "WiFi.h"
#include "esp_camera.h"
#include "esp_timer.h"
#include "img_converters.h"
#include "Arduino.h"
#include "soc/soc.h"           // Disable brownout problems
#include "soc/rtc_cntl_reg.h"  // Disable brownout problems
#include "driver/rtc_io.h"
#include <ESPAsyncWebServer.h>
#include <StringArray.h>
#include "FS.h"                // SD卡所用库
#include "SD_MMC.h"            // SD卡所用库
#include "time.h"
#include <WiFiUdp.h>
#include "driver/timer.h"
```

#### 2. 常量定义&预设置

> **1 Camera&SD Card**
>
> 摄像头参数：定义一个camera_config_t类型的结构体变量，然后给结构中的成员赋值（code中：使用默认配置，命名为config）
>
> 初始化参数配置：调用err = esp_camera_init(&camera_config);函数进行初始化（code中：使用默认config，无需配置）
>
> > https://blog.csdn.net/professionalmcu/article/details/121970340
>
> ```c
> camera_config_t camera_config = {
> 	// 参数信息
>     ...
> };
> // 初始化摄像头参数
> esp_err_t err;
> err = esp_camera_init(&camera_config);
> // 初始化摄像头
> configInitCamera();
> ```
>
> SD/MMD：均为存储卡
>
> > SD、MMD的区别：https://zhidao.baidu.com/question/4875813.html
>
> ```C
> // 初始化SD卡
> initMicroSDCard();
> // 定义root文件，内容为SD_MMC卡中内容，并生成文件列表
> File root;
> root = SD_MMC.open("/");
> listDirectory(SD_MMC);
> ```
>
> **2 AsyncWebServer：**
>
> 在主板和摄像头模组搭建WebServer → 实现PC/手机可以使用浏览器（Web-HTTP）访问主板并发起请求，主板通过HTTP向摄像头发起请求，WebServer通过响应函数对请求作出响应，完成功能
>
> > Arduino for ESP32 中默认就有WebServer，但内置WebServer都是同步的，不支持同时处理多个连接
> >
> > AsyncWebServer为异步WebServer，可以处理页面的多个连接和多个用户的请求
>
> > https://blog.csdn.net/Naisu_kun/article/details/107019175
> >
> > https://blog.csdn.net/weixin_44107116/article/details/122522243
>
> ```C
> // 1.声明WebServer对象
> AsyncWebServer server(80);
> 
> // 2.向请求端响应
> request->send(200, "text/plain", "Hello World!");
> 
> // 3.定义回调函数
> // 对应根目录"/";handleRoot为已自定义好的函数
> server.on("/", HTTP_GET, handleRoot);
> // 对应根目录,[]中为request传入参数，{}中为函数体内容
> server.on("/", HTTP_GET, [](AsyncWebServerRequest * request) {
>     request->send_P(200, "text/html", list.c_str());
>   });
> 
> // 4.启动服务器
> server.begin(); 
> ```

```C
// STEP2：常量定义（WiFi账号密码、网关IP、静态IP、各引脚）

// 1.WiFi配置
// 主板WiFi账号及密码
const char* ssid = "luckin";
const char* password = "123456789";
// 静态IP（网络摄像头）
IPAddress local_IP(192, 168, 4, 5);
// 网关IP（主板）
IPAddress gateway(192, 168, 4, 1);
// 子网掩码
IPAddress subnet(255, 255, 0, 0);
// DNS（可选）
IPAddress primaryDNS(8, 8, 8, 8);   
IPAddress secondaryDNS(8, 8, 4, 4); 

// 2.Web服务
// 在80端口创建AsyncWebServer服务
AsyncWebServer server(80);
// 相关参数（对应不同请求更改不同参数，驱动程序运行）
boolean takeNewPhoto = false;
String lastPhoto = "";
String list = "";
hw_timer_t * timer = NULL;
hw_timer_t * timer_1 = NULL;
boolean isRecording = false;
// HTTP GET请求参数
const char* PARAM_INPUT_1 = "photo";
const char* PARAM_INPUT_2 = "record_time";
const char* PARAM_INPUT_3 = "record_interval";

// OV2640摄像头模块引脚定义 (CAMERA_MODEL_AI_THINKER)
#define PWDN_GPIO_NUM     32
#define RESET_GPIO_NUM    -1
#define XCLK_GPIO_NUM      0
#define SIOD_GPIO_NUM     26
#define SIOC_GPIO_NUM     27
#define Y9_GPIO_NUM       35
#define Y8_GPIO_NUM       34
#define Y7_GPIO_NUM       39
#define Y6_GPIO_NUM       36
#define Y5_GPIO_NUM       21
#define Y4_GPIO_NUM       19
#define Y3_GPIO_NUM       18
#define Y2_GPIO_NUM        5
#define VSYNC_GPIO_NUM    25
#define HREF_GPIO_NUM     23
#define PCLK_GPIO_NUM     22

// 保存相机配置参数
camera_config_t config;
// 定义照片保存文件
File root;

// 3.声明并定义函数（内容略）
void configInitCamera(){
    ...
  }

void initMicroSDCard(){
  ...
}

void takeSavePhoto(){
  ...
}

void take_save_record(long duration, long interval) {
  ...
}

void stop_record() {
  if (isRecording) {
   ...
}

void listDirectory(fs::FS &fs) {
  ...
}

void deleteFile(fs::FS &fs, const char * path){
  ...
}
```

#### 3.初始化

> 1 断电探测器
>
> ESP32的电平低于某个值（该值可以设定），触发了断电探测器，断电探测器会使得ESP32重新启动
>
> > https://blog.csdn.net/qq_31232793/article/details/87889368
>
> 2 WiFi
>
> > 文档ESP32 Learn with Arduino及笔记中有详细使用说明
>
> 3 摄像头&SD卡初始化
>
> 调用ov2640camera及SD卡封装好的函数初始化，内置返回成功/失败信息，使用串口监视器查看/调试
>
> 4 WebServer
>
> AsyncWebServer（在变量定义部分已介绍）

```C
void setup() {
  // 1. 禁用断电探测器
  WRITE_PERI_REG(RTC_CNTL_BROWN_OUT_REG, 0);
  
  // 打开串口用于debug
  Serial.begin(115200);

  // 检查静态IP配置
  if (!WiFi.config(local_IP, gateway, subnet, primaryDNS, secondaryDNS)) {
    Serial.println("STA Failed to configure");
  }
  // 2. WiFi连接
  // 使用账号密码接入WiFi
  WiFi.begin(ssid, password);
  // WiFi状态为未连接时在串口输出“...”
  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.print(".");
  }
  // 成功连接时串口输出信息
  Serial.println("");
  Serial.println("WiFi connected");
  Serial.print("Camera Stream Ready! Go to: http://");
  Serial.println(WiFi.localIP());
  
  // 3. 摄像头初始化
  // 初始化摄像头模组
  Serial.println("Initializing the camera module...");
  configInitCamera();
  // 初始化SD卡
  Serial.println("Initializing the MicroSD card module... ");
  initMicroSDCard();
  
  // 4. WebServer
  // c_str()就是将C++的string转化为C的字符串数组，c_str()生成一个const char *指针，指向字符串的首地址
  server.on("/capture", HTTP_GET, [](AsyncWebServerRequest * request) {
    if (takeNewPhoto) {
      request->send_P(200, "text/plain", "");
    } else {
      camera_fb_t * frame = esp_camera_fb_get();
      request->send_P(200, "image/jpeg", (const uint8_t *)frame->buf, frame->len);
      esp_camera_fb_return(frame);
      frame = NULL;
    }
  });
  
  server.on("/list", HTTP_GET, [](AsyncWebServerRequest * request) {
    request->send_P(200, "text/html", list.c_str());
  });
  
  server.on("/view", HTTP_GET, [](AsyncWebServerRequest * request) {
    String inputMessage;
    String inputParam;
    // GET input1 value on <ESP_IP>/view?photo=<inputMessage>
    if (request->hasParam(PARAM_INPUT_1)) {
      inputMessage = "/" + request->getParam(PARAM_INPUT_1)->value();
      Serial.print("Trying to open ");
      Serial.println(inputMessage);
      inputParam = PARAM_INPUT_1;
    }
    else {
      inputMessage = "No message sent";
      inputParam = "none";
    }
    Serial.println(inputMessage);
    request->send(SD_MMC, inputMessage, "image/jpg", false);
  });
  
  // 当主板向摄像头模组删除照片的GET请求，要删除的图片名以参数形式早request中传入
  // delete?photo=<inputMessage>
  server.on("/delete", HTTP_GET, [] (AsyncWebServerRequest *request) {
    String inputMessage;
    String inputParam;
    // 获取传入的参数信息
    if (request->hasParam(PARAM_INPUT_1)) {
      inputMessage = "/" + request->getParam(PARAM_INPUT_1)->value();
      inputParam = PARAM_INPUT_1;
    }
    else {
      inputMessage = "No message sent";
      inputParam = "none";
    }
    Serial.println(inputMessage);
    // 删除对应的文件
    deleteFile(SD_MMC, inputMessage.c_str());
    request->send(200, "text/html", "Done. Your photo named " + inputMessage + " was removed." +
                                     "<br><a href=\"/list\">view/delete other photos</a>.");
  });

  server.on("/record", HTTP_GET, [](AsyncWebServerRequest * request) {
    String inputMessage_2;
    String inputMessage_3;
    if (request->hasParam(PARAM_INPUT_2)) {
      inputMessage_2 = request->getParam(PARAM_INPUT_2)->value();
      if (request->hasParam(PARAM_INPUT_3)) {
        Serial.println("Start record... Can't get ip camera stream now.");
        inputMessage_3 = request->getParam(PARAM_INPUT_3)->value();
        take_save_record(inputMessage_2.toInt(), inputMessage_3.toInt());
      }
      else {
        inputMessage_3 = "Request on /record lacks Param 1!";
        Serial.println(inputMessage_3);
      }
    }
    else {
      inputMessage_2 = "Request on /record lacks Param 2!";
      Serial.println(inputMessage_2);
    }
  });
  
  server.on("/stop_record", HTTP_GET, [](AsyncWebServerRequest * request) {
    Serial.println("Stop record. You can get ip camera stream now.");
    stop_record();
  });
  
  // 开启WebServer服务
  server.begin();
  // 定义SD卡存储文件夹
  root = SD_MMC.open("/");
  listDirectory(SD_MMC);
}
```

#### 4.主循环

```c
void loop() {
  if (takeNewPhoto) {
    takeSavePhoto();
    takeNewPhoto = false;
  }
  delay(1);
}
```



### ESP32_MCU端

#### 1.头文件：库引入

```c
#include "WiFi.h"
#include "esp_timer.h"
#include "Arduino.h"
#include "soc/soc.h"           // Disable brownout problems
#include "soc/rtc_cntl_reg.h"  // Disable brownout problems
#include "driver/rtc_io.h"
#include "driver/mcpwm.h"
#include <ESPAsyncWebServer.h>
#include <StringArray.h>
#include <FS.h>
```

#### 2.常量定义&预设置

> 码盘是指测量角位移的数字编码器

```c
// 1. 常量定义
// (1) 引脚
// LED引脚
const int front_led_pin = 32;
const int back_led_pin = 33;
const int left_turn_led_pin = 21;
const int right_turn_led_pin = 22;
const int brake_led_pin = 23;
// 码盘引脚
const int encoder_pin = 2;
int count = 0;
float encoder_speed = 0.0;
int encoder_interval_ms = 500;
// 电机引脚
const int motor_pwm_pin_A = 27;
const int motor_pwm_pin_B = 26;
// servo pwm pin
const int servo_pwm_pin = 13;
// (2) 参数
// 电机/舵机参数
int motor_duty_cycle = 30;
int servo_turn_angle = 45;
float servo_duty_cycle_center = 7.5;
float servo_duty_cycle_differ = 5;
// WiFi账号及密码
const char* ssid = "luckin";
const char* password = "123456789";
// (3) HTML界面
const char index_html[] PROGMEM = R"rawliteral(
    // 省略HTML内容
    ...
    )rawliteral";

// 2. 预处理
// (1) 开启WebServer服务
AsyncWebServer server(80);
// (2) 声明变量
// 结构变量
esp_err_t esp_err;
// 声明函数（函数内容略）
void toggle_light(int color);
void control_all_light(bool);
void move_forward();
void move_backward();
void motor_stop();
void turn_left();
void turn_right();
void straight();
void IRAM_ATTR count_add() {
  count += 1;
}
```

#### 3.初始化

```c
void setup() {
  // 1. 启用串口
  Serial.begin(115200);
  // 2. WiFi开启
  WiFi.mode(WIFI_AP);
  if(!WiFi.softAPConfig(IPAddress(192, 168, 4, 1), IPAddress(192, 168, 4, 1), IPAddress(255, 255, 0, 0))){
      Serial.println("AP Config Failed");
  }
  WiFi.softAP(ssid, password, 1, 0, 10);

  IPAddress IP = WiFi.softAPIP();
  Serial.print("AP IP address: ");
  Serial.println(IP);

  // 3. 关闭断电测试器
  WRITE_PERI_REG(RTC_CNTL_BROWN_OUT_REG, 0);

  // 4. 初始化
  // LED引脚初始化
  pinMode(front_led_pin, OUTPUT);
  pinMode(back_led_pin, OUTPUT);
  pinMode(left_turn_led_pin, OUTPUT);
  pinMode(right_turn_led_pin, OUTPUT);
  pinMode(brake_led_pin, OUTPUT);

  // 码盘引脚初始化
  pinMode(encoder_pin, INPUT);
  attachInterrupt(encoder_pin, count_add, RISING);

  // PWM信号初始化
  // PWM信号参数
  mcpwm_gpio_init(MCPWM_UNIT_0, MCPWM0A, motor_pwm_pin_A);
  mcpwm_gpio_init(MCPWM_UNIT_0, MCPWM0B, motor_pwm_pin_B);
  mcpwm_config_t motor_pwm_config = {
    .frequency = 1000,
    .cmpr_a = 0,
    .cmpr_b = 0,
    .duty_mode = MCPWM_DUTY_MODE_0,
    .counter_mode = MCPWM_UP_COUNTER,
  };
  esp_err = mcpwm_init(MCPWM_UNIT_0, MCPWM_TIMER_0, &motor_pwm_config);
  if (esp_err == 0)
    Serial.println("Setting motor pwm success!");
  else {
    Serial.print("Setting motor pwm fail, error code: ");
    Serial.println(esp_err);
  }

  // 舵机PWM配置
  mcpwm_gpio_init(MCPWM_UNIT_1, MCPWM1A, servo_pwm_pin);
  mcpwm_config_t servo_pwm_config;
  servo_pwm_config.frequency = 50;
  servo_pwm_config.cmpr_a = 0;
  servo_pwm_config.duty_mode = MCPWM_DUTY_MODE_0;
  servo_pwm_config.counter_mode = MCPWM_UP_COUNTER;
  esp_err = mcpwm_init(MCPWM_UNIT_1, MCPWM_TIMER_1, &servo_pwm_config);
  if (esp_err == 0)
    Serial.println("Setting servo pwm success!");
  else {
    Serial.print("Setting servo pwm fail, error code: ");
    Serial.println(esp_err);
  }
  mcpwm_start(MCPWM_UNIT_1, MCPWM_TIMER_1);

  // 5. WebServer响应函数
  server.on("/", HTTP_GET, [](AsyncWebServerRequest * request) {
    request->send_P(200, "text/html", index_html);
  });
  // 其他目录节点的响应函数略
  // WebServer服务开启
  server.begin();
}
```

#### 4.主循环

```c
// 主要用作串口监视和调试
void loop() {
  count = 0;
  delay(encoder_interval_ms);
  encoder_speed = count / 18.0 / 21 * 6.2 * 3.14 * 1000 / encoder_interval_ms;
  Serial.print("Speed: ");
  Serial.println(encoder_speed);
}
```
