---
bookCollapseSection: false
weight: 1
title: "LED"
---

LED
=====



### 點亮 LED
{{< highlight python "linenos=inline,linenostart=1" >}}
import machine
pin = machine.Pin(18, machine.Pin.OUT)
pin.value(1)
{{< /highlight >}}
### 閃爍 LED
{{< highlight python "linenos=inline,linenostart=1" >}}
import time
import machine

pin = machine.Pin(18, machine.Pin.OUT)
while True:
	pin.value(1)
	time.sleep(0.5)
	pin.value(0)
	time.sleep(0.5)
{{< /highlight >}}

### 呼吸燈

{{< highlight python "linenos=inline,linenostart=1" >}}
from machine import Pin, PWM
import time

DUTY_MAX = 2**16 - 1

duty_u16 = 0
delta_d = 16*4

p = PWM(Pin(5), 1000, duty_u16=duty_u16)
print(p)

time.sleep(1.5)

while True:
    p.duty_u16(duty_u16)
    time.sleep(0.001)
    duty_u16 += delta_d
    if duty_u16 >= DUTY_MAX:
        duty_u16 = DUTY_MAX
        delta_d = -delta_d
    elif duty_u16 <= 0:
        duty_u16 = 0
        delta_d = -delta_d
```