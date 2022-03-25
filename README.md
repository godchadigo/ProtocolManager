# ProtocolManager
## 一款基於HslCommunication的擴展插件 by黃御德

## 簡介
### ProtocolManager是hslCommunication的讀寫器擴展，打破了傳統讀取陣列一個一個對應的窘境，目前由本人開發大部分的功能，基本上可以單獨對每一個暫存器編上自己定義的標籤，包括讀以及寫，目前僅支持NetworkDeviceBase基類的擴展。

## 目前支持列表
- KeyenceMcNet(2022/03/23 測試人員:黃御德)
- Modbus(2022/03/23 測試人員:黃御德,不會游泳的魚)
- SiemensS7Net
- MelsecMcNet(2022/03/23 測試人員:不會游泳的魚)
- PanasonicMcNet
- OmronFinsNet
- KeyenceNanoSerial
- FujiSPB
- FatekProgram
- InovanceAMTcp
- YokogawaLinkTcp
- VigorOverTcp(2022/03/23 測試人員:黃御德)

## 使用方法

### 註冊機台

``` c#
            Protocol vigorPlc = new ProtocolLib.Protocol();   //實力化管理器
            vigorPlc.CustomName = "Vigor";                    //自訂機台名稱
            vigorPlc.IP = "192.168.0.20";					  //機台IP
            vigorPlc.Port = 5000;							  //機台Port
            vigorPlc.ReceiveTimeOut = 300;					  //接收超時
            vigorPlc.ConnectTimeOut = 1000;					  //連線超時
            vigorPlc.SleepTime = 1;							  //單筆數據讀取延遲
            vigorPlc.ScanTime = 50;                           //掃描時間
            vigorPlc.Type = ProtocolLib.ProtocolType.VigorOverTcp;//定義通訊協定
            vigorPlc.RegisterReadTag("Test", "D0", 4, new List<string>() 
            { "機械X", "機械Y", "機械X", "狀態代碼" }
            ,ProtocolLib.Type.ProtocolType.ProtocolTypeList.UInt16);//定義讀取標籤
            vigorPlc.RegisterWriteTag("電燈開關1","M0",ProtocolLib.Type.ProtocolType.ProtocolTypeList.Bool , false);定義寫入標籤非陣列
            vigorPlc.RegisterWriteTag("座標路徑", "D10", ProtocolLib.Type.ProtocolType.ProtocolTypeList.Int32 , true);//定義寫入標籤陣列
```
 
