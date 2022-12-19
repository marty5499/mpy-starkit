---
weight: 2
bookFlatSection: false
title: "2. 使用開發板物件"
---

2.使用開發板物件
========

Webduino 程式庫提供了 Board 類別，該類別包含了 WiFi 、 MQTT、存取參數檔等功能。
並內建WebServer提供設定 wifi等資訊、線上程式更新等。
此外斷線會自動重新連線，包辦所有繁瑣又基礎的功能。

board 物件內建功能如下
- 讀取參數檔(value.js)，連上指定 WiFi，斷線自動重連
- 連到 mqtt Server，可進行 pub/sub 訊息，斷線自動重連
- 內建 WebServer，可透過瀏覽器連接開發板設定 WiFi帳密等資訊
- 開發板內建訂閱命令集，可遠端進行
    - 確認開發板狀態
    - 讓開發板重新開機
    - 讓開發板下載指定程式檔案

### 開發板物件使用範例
{{< highlight python "linenos=inline,linenostart=1"  >}}
from webduino.board import Board
import machine, time

try:
    time.sleep(1.5)
    board = Board(devId='new')
    board.start(checkTime=0.5)

    # mqtt sub
    def runCode(msg):
        exec(msg)

    board.onTopic('app',runCode)

except Exception as e:
    print(e)
    machine.reset()
{{< /highlight >}}


### 接收所有開發板傳來的訊息
{{< figure src="board_recv.png" width="70%" >}}

### 開發板訂閱 Topic
{{< figure src="board_sub.png" width="70%" >}}

### 建立開發板物件，並連上 WiFi、MQTT，接收MQTT傳來訊息

devId 如果不輸入，會讀取根目錄下 value.js
:::info
value.js 裡面存放 wifi 帳號密碼，開發板 deviceId , ssid 等資訊
:::
{{< highlight python "linenos=inline,linenostart=1"  >}}
import machine, time
from webduino.board import Board
from machine import Timer

try:
    time.sleep(1.5)
    board = Board(devId='esp01')
    chk = Timer(-1)
    chk.init(period=500, mode=Timer.PERIODIC, callback=lambda t:board.check())

except Exception as e:
    print(e)
    machine.reset()
{{< /highlight >}}

### 訂閱指定 mqtt 寫法
{{< highlight python "linenos=inline,linenostart=1"  >}}
def runCode(msg):
    exec(msg)

board.onTopic('app',runCode)
{{< /highlight >}}