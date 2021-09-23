# Enable Fast PWM
```%USERPROFILE%/.platformio/packages/framework-arduinoststm32-maple/STM32F1/variants/generic_stm32f103r/boards.cpp``` set 
```c
{&gpioa, &timer5, &adc1, 0, 1, 0}, /* PA0 */ 
 ```
```Marlin/src/pins/stm32f1/pins_CREALITY_V4.h``` comment
```c
 //#define FAN_SOFT_PWM 
```
```Marlin/Configuration.h``` uncomment 
```c
#define FAST_PWM_FAN
``` 
```Marlin/Configuration.h``` comment (DO NOT comment out #define SOFT_PWM_SCALE 0)
```c
 //#define FAN_SOFT_PWM
```
```Marlin/src/HAL/STM32F1/timers.h```
```c
#define STEP_TIMER_NUM 4 // for other boards, five is fine. 
```

********************************************************************************************************
gcode.h:
********************************************************************************************************
```c
...
+   static void M9891();
...
```

********************************************************************************************************
gcode.cpp:
********************************************************************************************************
```c
...
+   case 9891: M9891(); break;
...
```

********************************************************************************************************
Add file gcode\M9891.cpp:
********************************************************************************************************

```c
#include "../../inc/MarlinConfig.h"

#include "../gcode.h"

void GcodeSuite::M9891() {
    const uint8_t pport = parser.byteval('P');

    const uint16_t dfreq = parser.ushortval('F');

    // Set frequency for port
    
    set_pwm_frequency((pin_t)pport, (int)dfreq);
}
```