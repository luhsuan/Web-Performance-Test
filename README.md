# 網站測試筆記(壓力與安全性)

- 目錄
[ToC]

##  HTTP Method：表單中的 GET 與 POST 有什麼差別？

### GET vs POST Method

先舉個例子，如果 HTTP 代表現在我們現實生活中寄信的機制，那麼信封的撰寫格式就是 HTTP。我們姑且將信封外的內容稱為 http-header，信封內的書信稱為 message-body，那麼 HTTP Method 就是你要告訴郵差的寄信規則。

假設 GET 表示信封內不得裝信件的寄送方式，如同是明信片一樣，你可以把要傳遞的資訊寫在信封(http-header)上，寫滿為止，價格比較便宜。然而 POST 就是信封內有裝信件的寄送方式（信封有內容物），不但信封可以寫東西，信封內 (message-body) 還可以置入你想要寄送的資料或檔案，價格較貴。

使用 GET 的時候我們直接將要傳送的資料以 Query String（一種Key/Vaule的編碼方式）加在我們要寄送的地址(URL)後面，然後交給郵差傳送。使用 POST 的時候則是將寄送地址(URL)寫在信封上，另外將要傳送的資料寫在另一張信紙後，將信紙放到信封裡面，交給郵差傳送。

## 壓力測試
>透過不斷的給予系統施加壓力，直到產生無法接受之延遲或癱瘓，進而得知目標系統能承受的最大壓力。壓力測試須設定多種不同情境觀察系統在不同的壓力下所能執行最大工作量的運作時間與其效能表現。壓力測試根據以下事項執行:
>
#### 測試項目
* 強度、流量 : 
依據目標對象所提供服務之族群、性質制定不同的流量測試或操作。
Ex: 電商平台須測試大流量以便在進行優惠活動時能順利進行
* 動作 : 
加入一些使用者操作如登入、頁面瀏覽、填寫表單等使測試計畫更為真實。
* 資料交換 : 
表單、API接口等一些需要與伺服器端做資料交換需求的服務須依照訂定之資料格式傳遞測試性資料以便檢查傳遞正確性及回應速度。
* 多樣性 : 
測試計畫須規劃多種不同組合以便詳細的測試在不同情境下對應伺服器狀態及穩定度。

#### 測試環境準備
使用Apache JMeter，需在JAVA JDK 8以上的環境運行。

#### 測試軟體教學
參考以下Jmeter網站之操作教學與設定參數 : 
https://ithelp.ithome.com.tw/articles/10203900?sc=rss.qu

#### 報表分析
* Aggregate Report報表上的名詞說明
    * #Samples：表示你這次測試中一共發出了多少個請求，如果模擬10個使用者，每個使用者重複執行10次，那麽這裡將會顯示100
    * Average：每筆request回應時間的平均值
    * Median：中位數，也就是 50％ 使用者的回應時間
    * 90%、95%、99% Line：第90、95、99 百分位數使用者的回應時間
    * Min：最小回應時間
    * Max：最大回應時間
    * Error%：本次測試中出現錯誤的請求的數量/請求的總數
    * Throughput：默認情況下表示每秒完成的請求數（Request per Second）
    * KB/Sec：每秒從服務器端接收到的數據量

## 白箱測試

>白箱測試（white-box testing）又稱透明盒測試（glass box testing）、結構測試（structural testing）等，軟體測試的主要方法之一，也稱結構測試、邏輯驅動測試或基於程式本身的測試。 測試應用程式的內部結構或運作，而不是測試應用程式的功能（即黑箱測試）。

### 測試項目
* Code Smell (Maintainability domain)可維護性
* Bug (Reliability domain)可靠性
* Vulnerability (Security domain)安全性
* Security Hotspot (Security domain)安全性
### 軟體執行教學(windows)
#### SonarQube
1. 依照官方軟硬體需求表:https://docs.sonarqube.org/latest/requirements/requirements/
2. 下載並安裝jdk 11 : https://www.oracle.com/java/technologies/javase-jdk11-downloads.html
3. SonarQube下載網址 : https://www.sonarqube.org/downloads/
4. 解壓縮後以系統管理員身分依序執行這三個檔案
![](https://i.imgur.com/OviTAtj.png)
5. 將jdk路徑添加到wrapper.conf的檔案內，之後再執行一次StartSonar.bat : https://community.sonarsource.com/t/community-version-startsonar-bat-fails-with-sonarqube-requires-java-11-to-run/12204/7
6. 登入 http://localhost:9000/
7. 使用系統預設的管理員憑據（Account:admin ,pwd: admin）登陸成功
#### SonarScanner
8. 再下載SonarScanner，用來分析程式來源 : https://docs.sonarqube.org/latest/analysis/scan/sonarscanner/
9. 依照上方說明文件，在要分析程式碼的根目錄新增一個sonar-project.properties，projectName是專案名字，sources是原始檔所在的目錄
10. 在SonarQube新增專案，創建專案名稱 :arrow_right: 產生token :arrow_right: 將SonarScanner的bin目錄添加到％PATH％環境變量 
 ![](https://i.imgur.com/5MaZazX.png)
11. 在SonarQube新增專案的地方，複製內涵token的指令碼 :arrow_right: 在要分析程式碼的根目錄叫出"以系統管理員身分開啟window poewrshell" :arrow_right: 將複製的指令碼貼上並執行
12. 執行完成後回到SonarQube主頁觀看分析結果

官方說明文件 : https://docs.sonarqube.org/latest/setup/install-server/

教程 : https://www.itread01.com/content/1542253277.html 
https://www.itread01.com/content/1563847384.html
https://blog.poychang.net/sonarqube-csharp/
https://dotblogs.com.tw/supershowwei/2016/10/30/164450
https://myweb.ntut.edu.tw/~jykuo/train/2017Sonarqube.pdf
### 軟體執行教學(Linux) 
#### SonarQube
1. 安裝jdk 11 Linux Debian Package https://www.oracle.com/java/technologies/javase-jdk11-downloads.html
2. 解壓縮，將conf/wrapper.conf打開 將jdk在系統的位置加入wrapper.java.command=/usr/lib/jvm/jdk-11.0.8/bin/java
3. 啟動，輸入:
```
cd /home/pt/Downloads/sonarqube-8.4.2.36762/bin/linux-x86-64

./sonar.sh start
```
4. 登入 http://localhost:9000/
#### SonarScanner
1. 在所測資料夾根目錄打開cmd
2. 通過修改profile檔案，在cmd輸入:sudo gedit /etc/profile
3. 在開啟的檔案最下面加入並儲存
```
export
 SONAR_SCANNER_HOME=/home/pt/Downloads/sonar-scanner-4.4.0.2170-linux
export
 PATH=$ {SONAR_SCANNER_HOME}/bin:${PATH}
```
4. 要讓剛才的修改馬上生效，在cmd輸入:source /etc/profile
5. 再輸入sonar-scanner -h，若有出現下面圖片內容 表示成功
```
usage: sonar-scanner [options]

Options:
 -D,--define <arg>     Define property
 -h,--help             Display help information
 -v,--version          Display version information
 -X,--debug            Produce execution debug output
```
6. 再將SonarQube project 產生出的內涵指令貼上cmd
7. 分析完後即可在 http://localhost:9000/ 看到結果
參考網站 : https://www.itread01.com/content/1550144356.html

## 黑箱測試
>以第三方角度進行安全性測試，主要為滲透測試，模擬在僅知系統網域、URL的情形下自外部的入侵狀況。項目依據國際組織OWASP所公布2017前10項滲透行為以及政府機關滲透測試專案徵求書內容擬定。

參考網站 : 
https://secbuzzer.co/post/116
https://secbuzzer.co/post/115


