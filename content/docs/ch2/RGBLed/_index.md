---
bookCollapseSection: false
weight: 2
title: "RGB LED"
---
RGBLED
======


{{< figure src="https://md.webduino.io/uploads/upload_b4d0213294faef2e2191c653c12c5fce.png" width="50%" >}}


{{< highlight python "linenos=inline,linenostart=1" >}}
from machine import Pin
import time

p0 = Pin(0, Pin.OUT)
for i in range(10):
    time.sleep(0.25)
    p0.value(i%2)
{{< /highlight >}}