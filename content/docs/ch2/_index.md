---
bookCollapseSection: true
weight: 2
title: "二、使用傳感器"
---


#### tags: `micropython` > [[回首頁]](https://md.kingkit.codes/cYuxdDkOSJ2guIzPG-KHqA?view)

使用傳感器範例
========

### 按下紅色小怪獸，OLED會顯示 Hello
{{< figure src="https://md.webduino.io/uploads/upload_d6d4224af36b4e34d818375b23bc2812.png" width="30%" >}}

### [積木程式連結](https://webbit.webduino.io/blockly/#lRQYa86p9mDRG)
{{< figure src="https://md.webduino.io/uploads/upload_cd133835e33f8e96021190de4101f606.png" width="80%" >}}

- boot.py： 建立 WiFi , MQTT 連線，並確保斷線自動重新連線
- main.py： 使用者自行撰寫的程式碼，可能是控制一個或多個傳感器，並且接收mqtt訊息


## boot.py
{{< highlight python "linenos=inline,linenostart=1" >}}
#####################
try:
    import cmd
    print("import cmd....")
    machine.reset()
except:
    pass
#####################

import machine, time
from webduino.board import Board
from machine import Timer

try:
    time.sleep(1.5)
    board = Board(devId='wa01')
    chk = Timer(-1)
    chk.init(period=500, mode=Timer.PERIODIC, callback=lambda t:board.check())

except Exception as e:
    print(e)
    machine.reset()
{{< /highlight >}}
## main.py 使用 OLED
{{< highlight python "linenos=inline,linenostart=1" >}}
import ssd1306
from machine import Pin,I2C

gnd = Pin(15,Pin.OUT)
gnd.value(0)

i2c = I2C(scl=Pin(13), sda=Pin(12), freq=100000)  #Init i2c
lcd=ssd1306.SSD1306_I2C(128,64,i2c) #create LCD object,Specify col and row
lcd.text("ESP8266",0,0)
lcd.show()

def runCode(msg):
    exec(msg)

board.onTopic('app',runCode)
print("gogogo...")
{{< /highlight >}}
