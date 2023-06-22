### 实用 GPIO

> 用到再查，熟能生巧，别上来就背图，一天你就忘了！

![](./image/basic/ab1.png)

#### 仅输入引脚

下面的四个引脚由于内部没有上拉下拉电阻，所以仅仅支持输入信号  
GPIO 34  
GPIO 35  
GPIO 36  
GPIO 39

<br>

#### SPI Flash 闪存引脚

这些引脚都是对 ESP32 内部 flash 进行操作的，最好不要使用这些引脚进行输入输出操作！

GPIO 6 (SCK/CLK)  
GPIO 7 (SDO/SD0)  
GPIO 8 (SDI/SD1)  
GPIO 9 (SHD/SD2)  
GPIO 10 (SWP/SD3)  
GPIO 11 (CSC/CMD)

<br>

#### 电容触摸引脚

这个引脚比较有意思，他们自带了电容触摸传感器，当我们直接用手触摸引脚时会发生电荷改变，从而传感器接收到并输出大小不一的信号脉冲

Arduino IDE 中的触摸引脚分配存在问题。GPIO 33 在分配中与 GPIO 32 交换。这意味着，如果要引用 GPIO 32，则应在代码中使用 T8；对于 GPIO 33，则使用 T9

T0 (GPIO 4)
T1 (GPIO 0)
T2 (GPIO 2)
T3 (GPIO 15)
T4 (GPIO 13)
T5 (GPIO 12)
T6 (GPIO 14)
T7 (GPIO 27)
T8 (GPIO 33)
T9 (GPIO 32)

<br>

下面是一个小实验，我们要实现触摸 GPIO4（对应 T0）口实现板上 LED 亮灭；  
仅需使用一个公对母线，连接到 GPIO4 口上，然后用手指触摸引出的公端口即可进行测试；

使用 `touchRead` 方法，检测对应 GPIO 口上的传感器对应返回数值；

经过测试，得出结果：当手指触摸时数值 10-20，手指移开后数值 70-80；所以可以得到以下检测代码

```c
const int LED = 2;

void setup()
{
  pinMode(LED,OUTPUT);

  Serial.begin(115200);
  delay(1000); // give me time to bring up serial monitor
  Serial.println("ESP32 Touch Test");
}

void loop()
{
  // touchRead(T0)直接读取对应带触摸传感器GPIO的代号
  Serial.println(touchRead(T0));
  // touchRead(4)或者直接指定GPIO口
  touchRead(4)<=20 ? digitalWrite(LED,HIGH) : digitalWrite(LED,LOW);
}
```

<br>

#### ADC 模数转换器引脚

基本上半数以上引脚都支持模数转换，具体引脚请看图片，这里空间有限不一一指出

在使用 WIFI 时建议仅使用 ADC1 类型的引脚，因为 ADC2 类型的引脚大概率会出错；

ADC 引脚用于将电压值转换为数值，但是实际情况不是线性的，ESP32 存在以下特殊情况：

- 0.0v-0.1v 时，转换数值均为 0
- 3.2v-3.3v 时，转换数值均为 4095

<br>

#### DAC 数模转换器引脚

这个就比较少了，只有俩  
DAC1 (GPIO25)
DAC2 (GPIO26)

<br>

#### RTC

<br>

#### PWM 脉冲宽度调制

所有具有 OUTPUT 特性的引脚均可使用 PWM

<br>

### PWM

```c
// 定义LED引脚的编号
const int ledPin = 2; // 15 对应GPIO16

// 设置PWM属性
const int freq = 5000; // PWM频率
const int ledChannel = 0; // PWM通道
const int resolution = 8; // 分辨率

void setup(){
    // 配置LED的PWM功能
    ledcSetup(ledChannel, freq, resolution);

    // 将PWM通道附加到要控制的GPIO引脚
    ledcAttachPin(ledPin, ledChannel);
}


// 增加LED亮度
void loop(){
    for(int dutyCycle = 0; dutyCycle <= 255; dutyCycle++){
        // 使用PWM改变LED亮度
        ledcWrite(ledChannel, dutyCycle);
        delay(15);
    }
    // 减小LED亮度
    for(int dutyCycle = 255; dutyCycle >= 0; dutyCycle--){
        // 使用PWM改变LED亮度
        ledcWrite(ledChannel, dutyCycle);
        delay(15);
    }
}
```

<br>

### 定时器
