---
bookCollapseSection: false
weight: 3
title: "ws2812"
---


彩燈 WS2812B
====

{{< figure src="https://md.webduino.io/uploads/upload_7ff5beb747f2dbc043c5300e2c32dd6e.png" width="50%" >}}


### ws2812b
{{< highlight python "linenos=inline,linenostart=1" >}}
import time
import machine, neopixel
from webduino.board import *

np = neopixel.NeoPixel(machine.Pin(2), 25)

def setLED(r,g,b):    
    for led in range(25):
        np[led] = (r,g,b)
    np.write()
 
setLED(0,1,0)
print('done.')
```