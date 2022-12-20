---
weight: 4
bookFlatSection: false
title: "4. 第一次使用就上手"
---

#### tags: `micropython` > [[回首頁]](https://md.kingkit.codes/cYuxdDkOSJ2guIzPG-KHqA?view)
第一次使用就上手
=====

待補充

- 線上燒錄
- 安裝程式庫
- 建立 main.py
- 和網頁互動
    - Blockly
    - Node-Red
    - 網頁
    - 萬用遙控器



## main.py 啟動流程說明

1. 檢查 cmd.py 是否存在，存在就執行，執行後刪檔重開
2. 檢查參數檔案 value.js 是否存在，沒有就建立預設參數檔
3. 建立開發板物件、讀取 value.js 開啟熱點
4. 進行 WiFi 和 MQTT連線

{{< highlight python "linenos=inline,linenostart=1" >}}
# 預設參數檔
var data={
    'IP': 'xxx.xxx.xxx.xxx', 
    'AP': 'webduino.io', 
    'zone': 'global', 
    'Ver': '0.99b', 
    'ssid1': 'KingKit_2.4G', 
    'ssid2': '', 
    'ssid3': '', 
    'passwd1': 'webduino', 
    'passwd2': '', 
    'passwd3': '', 
    'openAp': 'No',
    'MAC': '??:??:??:??:??:??', 
    'devId': 'a12345', 
    'devSSID': 'wa5498', 
    'devPasswd': '12345678',
}
{{< /highlight >}}
| Key       | 預設值 |               說明 |
| --------- | ------ | ------------------:|
| zone      | global |        MQTT Server |
| devId     | --     |   開發板唯一識別碼 | 
| devSSID   |        | 開發板WiFi熱點名稱 |
| devPasswd |        | 開發板WiFi熱點密碼 |
| openAp    | No     |   熱點是否持續打開 |
| ssid1     |        |    第一組WiFi ssid |
| passwd1   |        |    第一組WiFi 密碼 |
| ssid2     |        |    第二組WiFi ssid |
| passwd2   |        |    第二組WiFi 密碼 |
| ssid3     |        |    第三組WiFi ssid |
| passwd3   |        |    第三組WiFi 密碼 |




## 程式碼說明

{{< highlight python "linenos=inline,linenostart=1" >}}
import machine
from webduino.board import Board

def cb(cmd):
    print(">> "+cmd)

try:
    board = Board(devId='smart')
    board.onTopic("test",cb)
    board.loop()
except Exception as e:
    print(e)
    machine.reset()
{{< /highlight >}}
上面程式碼是最簡單的建立開發板、連網、接收訊息的程式範例
如果自己寫的程式，也需要有一個無窮迴圈處理某件事 (例如持續偵測按鈕是否有按下、接收傳感器狀態等，就可以使用下列方式)

### 將檢查Server訊息放在自訂迴圈內

接收mqtt訊息，需要執行 board.loop() 主動檢查是否有訊息，如果需要自行撰寫迴圈，可改用 board.check() 來進行接收mqtt訊息檢查
{{< highlight python "linenos=inline,linenostart=1" >}}
# 將 board.loop() 改成下列程式碼

while True:
    board.check()
{{< /highlight >}}
## 初始化預設使用的類別庫

{{< figure src="https://md.webduino.io/uploads/upload_8076f9ea4df3fe71f006635f2449e100.jpeg" >}}

## 預設控制命令

為了方便控制、更新、協作物聯網裝置，Webduino micropython版本
開發板上線後，會預設訂閱MQTT通道，實現下列功能
- [ ] 更新 main.py，執行指定的功能
- [ ] 下載安裝或更新指定的程式庫
- [ ] 回報板子狀況 (記憶體量、目前運行功能、mac address)


## 開發板上線流程
1. 如果 cmd.py 存在就執行後重開
2. 進行 wifi 、 mqtt 連線
3. 訂閱 Topic: ${deviceId}/cmd
4. 發送 Topic: waboard/state 開發板的 macAddress,功能說明

https://webduinoio.github.io/webduino-remote/index.html#-MugCxmGecBp_r9rMquA

waboard/state
- ${CMD} ${macAddress} ${String}

${deviceId}/cmd 資料內容如下
|  命令  | 參數1  | 參數2            | 
|:------:| ------ | ---------------- |
|  ping  | 無     | 無               |
| reset  | 無     | 無               |
| reboot | 無     | 無               |
|  save  | ${url} | ${path/filename} |
