### 输入输出

#### 脉宽调制 PWM

使用 mbed 库中提供的数据类型 `PwnOut` 将指定引脚定义脉冲输出

```c
#include "mbed.h"

// 指定引脚D9
PwnOut pout(D9);

int main(){

    // 指定周期2.0f，即2s
    pout.period(2.0f);
    // 指定占空比50%
    pout.write(0.50f);
    while(1);
}
```

<br>

### 数字信号处理

低通滤波器、高通滤波器、带通滤波器
