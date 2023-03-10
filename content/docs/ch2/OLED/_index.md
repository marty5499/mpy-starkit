---
bookCollapseSection: false
weight: 4
title: "OLED"
---


OLED
====



{{< figure src="https://md.webduino.io/uploads/upload_ed1c52d1191c7fcf21ac4b5f5d8d10fe.png" width="50%" >}}


### SSD1306 128x64

{{< highlight python "linenos=inline,linenostart=1" >}}
import ssd1306
from machine import Pin,I2C
from webduino.board import Board

i2c = I2C(scl=Pin(0), sda=Pin(2), freq=100000)  #Init i2c
display = ssd1306.SSD1306_I2C(128,64,i2c) #create LCD object,Specify col and row
display.fill(0)
display.text("Init...",5,20)
display.show()

def cb(cmd):
    print(">> "+cmd)
    data = cmd.split(' ')
    display.fill(0)
    display.text("Temp:",0,0)
    display.text(data[0],20,12)
    
    display.text("Humi:",0,28)
    display.text(data[1],20,40)
    display.show()    

try:
    board = Board(devId='sroom')
    board.onTopic("data",cb)
    board.loop()
except Exception as e:
    print(e)
    machine.reset()
{{< /highlight >}}

### Wemos OLED


{{< figure src="https://md.webduino.io/uploads/upload_965b7ac582dcae080ca9533709096e6e.png" width="20%" >}}

{{< highlight python "linenos=inline,linenostart=1" >}}
import ssd1306
from machine import Pin,I2C
from webduino.board import Board

i2c = I2C(scl=Pin(5), sda=Pin(4), freq=100000)
display = ssd1306.SSD1306_I2C(64,48,i2c)
display.fill(0)
display.text("Init...",5,20)
display.show()

def cb(cmd):
    print(">> "+cmd)
    data = cmd.split(' ')
    display.fill(0)
    display.text("Temp:",0,0)
    display.text(data[0],20,12)
    
    display.text("Humi:",0,28)
    display.text(data[1],20,40)
    display.show()    

try:
    board = Board(devId='sroom')
    board.onTopic("data",cb)
    board.loop()
except Exception as e:
    print(e)
    machine.reset()
```