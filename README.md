# 防疫一號維修工具
## 使用說明
![](https://i.imgur.com/FAMcAuT.png)
![](https://i.imgur.com/xifVrne.png)
![](https://i.imgur.com/fxeBmYc.png)

## 維修sop:
1. RFID沒反應 => 更換一片新的RFID (更換時記得先關電)
2. 藍牙很怪 => 更新driver => 更新作業系統(到win10) => 換一片ESP32
3. 其他 自己想辦法

## 安裝ESP32開發板
1. In your Arduino IDE, go to File> Preferences
![](https://i.imgur.com/0RB9REw.png)

2. Enter https://dl.espressif.com/dl/package_esp32_index.json into the “Additional Board Manager URLs” field as shown in the figure below. Then, click the “OK” button:
![](https://i.imgur.com/mgxrflA.png)

## 分割Partition
1. 取代esp32開發板套件檔案
![](https://i.imgur.com/ixNyHGg.png)
    - 將repo中的board.txt取代掉原本的board.txt
    - windows版本的路徑如附圖，mac請自行尋找或問蜊維

## 上傳程式bin檔
- 如果esp32壞掉要換新的，需要上傳程式，用下面指令可以直接上傳bin檔，port 要改成對應的port ( --port <my_port>)

```python=
python esptool/esptool.py --chip esp32 --port COM11 --baud 921600 --before default_reset --after hard_reset write_flash -z --flash_mode dio --flash_freq 80m --flash_size detect 0xe000 esptool/boot_app0.bin 0x1000 esptool/bootloader_dio_80m.bin 0x10000 esptool/esp32_LTS.ino.bin 0x8000 esptool/esp32_LTS.ino.partitions.bin 
```

## 修改設定檔

1. /data/SETTING.json
```json=
{
    "STATION_ID": 5101,
    "STATION_NAME": "防疫一號-101",
    "STATION_AUTHOR": "NTUEE - EP Crossing Team",
    "DOMAIN_POSTFIX": ".ee.musou.toby.best",
    "WIFI_NAME": "NTU",
    "BACKUP_WIFI_NAME": "NTU2",
    "WIFI_PWD": "",
    "BACKUP_WIFI_PWD": "00000000",
    "TIMEOUT_RX": 200,
    "TIMEOUT_RFID": 200,
    "PERIOD_ALIVE_REPORT": 5000,
    "DELAY_LOW_TMP": 200,
    "DELAY_HIGH_TMP": 4000,
    "LOW_TMP": 32,
    "HIGH_TMP": 37.5,
    "WAIT_TMP": 8000,
    "WAIT_ERROR": 5000,
    "WAIT_SHOW": 5000,
    "QRCODE_LENGTH": 100,
    "SEND_TO_SERVER_STRSIZE": 200
}
```
2. 必要修改內容:
    - STATION_ID: 依照機器編號設定，如101號則設定為5101
    - STATION_NAME: 依照機器編號設定，如101號則設定為"防疫一號-101"
3. 可能需要修改內容:
    - WIFI_NAME、WIFI_PWD: 預設連線 Wi-Fi，如果他們那邊NTU Wi-Fi訊號不然後剛好那邊有他們自己的Wi-Fi的話好則可以將這組帳密改掉
    - BACKUP_WIFI_NAME、BACKUP_WIFI_PWD: 備用連線，同上
    - DELAY_LOW_TMP: 溫度過低時的警報聲，因為量產的mos品質太差，可能會需要較長的delay time 才可以有適當的聲音
4. 修改完存檔即可依上個步驟上傳設定檔

## 上傳設定檔
1. 在Arduino IDE安裝上傳設定檔用的工具:
將ESP32FS 資料結複製到Arduino安裝路徑內(如下圖)(mac也可以不過路徑自己找或問蜊維) 
![](https://i.imgur.com/1gH4Qk4.png)

2. 開啟Repair-Automatic-Forehead-Thermometer.ino:
![](https://i.imgur.com/cQkPv8x.png)

3. 選擇開發板
![](https://i.imgur.com/osVtFt0.png)

5. 選擇port
![](https://i.imgur.com/dCIlz9J.png)

7. 選擇partition
![](https://i.imgur.com/p7ia1My.png)

3. 點 [工具] => [ESP32 Sketch Data Upload]:
這時候他會自動上傳跟sketch同資料夾層級內 data/ 這個資料夾內的所有內容，這裡我們需要的檔案只有 data/SETTING.json


