# ProtocolManager 點位管理器

### 一款基於HslCommunication的擴展插件 by黃御德

### 為了我的頭髮著想，懇求各位大大斗內斗內!!
### 該插件目前屬於測試階段，不開放原始碼，但能仍可使用，未來的版本將進行收費，目前開放的版本可以解決大部分的難題，如果有問題將會再付費版更新，謝謝!
### 如需教育訓練請(1500 塊/1hr) [不教受 hslcommunication 套件操作，如想學習請至原作者群組購買課程] + Line:3026016

### 註冊機台
[![IMAGE ALT TEXT](http://img.youtube.com/vi/raLfhP-Ox2A/0.jpg)](https://youtu.be/raLfhP-Ox2A "註冊機台")
### 讀取 寫入 數據
[![IMAGE ALT TEXT](http://img.youtube.com/vi/S6IUPbTr2VY/0.jpg)](https://youtu.be/S6IUPbTr2VY "讀取 寫入 數據")
### 自動循環讀取數據
[![IMAGE ALT TEXT](http://img.youtube.com/vi/eX5RCPrtIug/0.jpg)](https://youtu.be/eX5RCPrtIug "自動循環讀取數據")
---

## 商業免費 免責聲明
- 此種模式完全免費，目前無廣告，目前無註冊碼，目前無期限，設備連線數量限制1台，但是使用前請盡可能的測試，如果因為使用組件發生了一些導致生命財產損失（例如和設備通訊導致意外事故，誤觸發旗標），本人一概不負責。
- 項目中必須聲明組件的原作者信息。
- 如果有使用上的問題，不提供技術支持。

---
## 商業付費 免責聲明
- 若是使用者本身設計不當造成一些導致生命財產損失（例如和設備通訊導致意外事故，誤觸發旗標），本人一概不負責。
- 本組件使用請依照環境需求，程式語言熟練程度斟酌使用，切勿勉強，人為損失本人一概不負責。
- 如果有使用上的問題，本人提供一定的技術支持。僅限於本組件的使用問題，不包含其他的業務邏輯。
---

## 簡介
### ProtocolManager是hslCommunication的讀寫器擴展，打破了傳統讀取陣列一個一個對應的窘境，目前由本人開發大部分的功能，基本上可以單獨對每一個暫存器編上自己定義的標籤，包括讀以及寫，目前僅支持NetworkDeviceBase基類的擴展。

---

## 免費 VS 付費
|項目 | 免費 | 付費 | 
| --------| -------- | -------- | 
| 設備數量    | 1台     | 無限制     | 
| 售後處理    | 無     | 有     | 
| 定期更新    | 無     | 有     | 
| 教學影片    | 無     | 有     | 
| 私人建議    | 無     | 有     | 
| 運行時間    | 暫時無     | 無限制     | 
| 掃描週期    | 暫時無     | 1ms     | 
| 數據點訂閱模式    | 暫時未開發     | 暫時未開發     | 
| 程式內核    | 級快     | 級快     | 
| 授權碼    | 無     | 暫時無     | 

---
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

---
## 注意事項
- 本擴展基於HslCommunication開發，使用前請先向原作者購買授權，本人不提供原套件的教學以及授權碼!
- 本擴展插件在使用者執行"ProtocolManager.Instance.Start();"時會在背景開一條無窮線程來掃描設備數據，所以使用者請勿再此指令再次寫迴圈，會造成cpu使用率突增!!
- 註冊"寫入標籤"時須注意最後一個參數"IsArray"代表該點為是否屬於陣列，請勿再非陣列點為寫入單一數據，也請勿在單一數據點位寫入陣列數據。
---
## 使用方法

### 1.註冊機台

``` c#
            Protocol vigorPlc = new ProtocolLib.Protocol();   //實力化管理器
            vigorPlc.CustomName = "Vigor";                    //自訂機台名稱
            vigorPlc.IP = "192.168.0.20";		  //機台IP
            vigorPlc.Port = 5000;		              //機台Port
            vigorPlc.ReceiveTimeOut = 300;		  //接收超時
            vigorPlc.ConnectTimeOut = 1000;		  //連線超時
            vigorPlc.SleepTime = 1;			  //單筆數據讀取延遲
            vigorPlc.ScanTime = 50;                           //掃描時間
            vigorPlc.Type = ProtocolLib.ProtocolType.VigorOverTcp;      //定義通訊協定
            vigorPlc.RegisterReadTag("狀態群組", "D0", 4, new List<string>() 
            { "機械X", "機械Y", "機械X", "狀態代碼" }
            ,ProtocolLib.Type.ProtocolType.ProtocolTypeList.UInt16);    //定義讀取標籤
            vigorPlc.RegisterWriteTag("電燈開關1","M0",ProtocolLib.Type.ProtocolType.ProtocolTypeList.Bool , false);        //定義寫入標籤非陣列
            vigorPlc.RegisterWriteTag("座標路徑", "D10", ProtocolLib.Type.ProtocolType.ProtocolTypeList.Int32 , true);      //定義寫入標籤陣列
                                    
            ProtocolManager.Instance.RegisterMachin("設備的名稱" , vigorPlc);            //註冊機台到駐列
            ProtocolManager.Instance.Start();                                       //啟動掃描
```

### 2.讀取標籤
```c#
            if(ProtocolManager.Instance.GetMachin("設備的名稱" , out Protocol device))
            {
                 //取得一個群組裡面的所有資料點
                 //注意這裡使用泛型型別，使用者可以自訂回傳的類別
                device.GetAllValue<bool>("狀態群組").ToList().ForEach(x => {
                    Console.WriteLine(string.Format("Tag: {0} , Value: {1} , IsSuccess: {2}", x.Key, x.Value.Value, x.Value.IsSuccess));
                });
                
                //取得單一群組裡的單一數據點
                //***** 沒錯取的數據就是這麼簡單，一行即可呦! *****//
                var posX = device.GetValue<int>("狀態群組", "機械X");
                var posY = device.GetValue<int>("狀態群組", "機械Y");
                var posZ = device.GetValue<int>("狀態群組", "機械Z");
                var status = cm122.GetValue<int>("狀態群組", "狀態代碼");
                
                //打印內容
                Console.WriteLine(string.Format("機械座標X: {0} , 數據點連線狀態: {1}" , posX.Value , posX.IsSuccess));
                Console.WriteLine(string.Format("機械座標Y: {0} , 數據點連線狀態: {1}" , posY.Value , posX.IsSuccess));
                Console.WriteLine(string.Format("機械座標Z: {0} , 數據點連線狀態: {1}" , posZ.Value , posX.IsSuccess));
                Console.WriteLine(string.Format("狀態代碼: {0} , 數據點連線狀態: {1}" , status.Value , status.IsSuccess));
                
            }
```

### 3.寫入數據標籤


```c#
            //初始化一個列表集合
            List<int> data = new List<int>();
            //迴圈塞入資料
            for (int i = 0; i< 50; i++)
            {
                data.Add(i);
            }
            //
            if (ProtocolManager.Instance.GetMachin("設備的名稱", out Protocol device))
            {
                device.WriteTagValue("電燈開關1", true);         //寫入單一個數據
                device.WriteTagValue("座標路徑", data.ToArray());//寫入陣列數據
                
            }
```



