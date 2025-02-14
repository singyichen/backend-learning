---
title: COPI14 派車單建立作業
description: 
published: true
date: 2023-05-16T08:58:01.341Z
tags: 
editor: markdown
dateCreated: 2023-05-16T02:00:57.004Z
---

# 程式代號
## COPI14

# 程式名稱
## 派車單建立作業

# 作業目的
## 將會用到派車之異動單據輸入，包含銷貨單庫存異動單據、轉撥單、借入單及借入歸還單。

# 資料檔案
## (R/W)
### [COPTN 出貨派車單頭資料檔](http://192.168.25.60:9005/zh-tw/軟體開發/學習心得/11542/Report/Company/福氣物流/System/訂單管理系統/派車管理/資料來源/COPTN出貨派車單頭資料檔)
### [COPTO 出貨派車單身資料檔](http://192.168.25.60:9005/e/zh-tw/軟體開發/學習心得/11542/Report/Company/福氣物流/System/訂單管理系統/派車管理/資料來源/COPTO出貨派車單身資料檔)
## (R/O)
### CMSMQ 各系統單據設定檔
### COPTG 銷貨單單頭資料檔
### COPTH 銷貨單單身資料檔
### CMSMB 廠別資料檔
### CMSME 部門資料檔
### CMSMV 員工基本資料檔
### ADMMF 使用者資料
### CMSNA 付款條件檔
### CMSMI 派車資料設定檔
### CMSMR 交易對象分類方式檔
### COPMA 客戶基本資料檔
### INVMB 品號基本資料檔
### INVTA 異動單據單頭檔
### INVTF 借出入轉撥單頭檔
### INVTH 借出入歸還單頭檔

# 輸入畫面
## 單頭：主資料、派車資料、其他資料 ➔ COPTN
## 單身 ➔ COPTO
## 單尾 ➔ COPTN

![ERP訂單管理系統派車管理作業派車單建立作業.png](http://192.168.25.60:8000/files/file_storage/f880156d.png)


# 欄位說明
## 單頭：主資料
### 1.派車單別 (TN001)

* 須先在「單據性質設定」設定「2A.派車單」的單別。

* 可點選(icon)或[F2]單據性質資料，此單別須存在或單據性質須相符，並顯示單據名稱。但不允許空白。

* 若此單別之編碼方式為自動編號者，輸入單別後跳至單據日期欄位。

* 連續新增時預設前一筆單別。 


### 2.派車單號 (TN002)

* 單別之編碼方式為「手動編號」須自行輸入不可空白,並確認是否重覆,其餘自動預設單號.


### 3.單據日期 (TN003)

* 當系統日大於等於庫存現行年月時,則預設系統日期.亦可修改,點選(icon)或[F2]日期快手查詢.但不允許空白.

* 當連續新增時自動預設前一筆單據日期資料.

* 所輸入之日期資料不可小於庫存現行年月.

* 預設派車日期等於單據日期.
 

### 4.運送方式 (TN004)

* 可點選(icon)或[F2]運輸方式代號查詢(CMSNJ-02),並顯示運輸方式中文名稱.但不可空白.

* 變動時則清除車號.

* 運送方式採「自送」方式時,單件運費,總運費及運費稅額欄位預設初值為0且自動顯示.


## 單頭：派車資料

### 1.車號 (TN005)

* 不可空白,可不存在

* 可點選(icon)或[F2]派車資料查詢,自動對應車號,送貨者資料.

* 手動輸入時,預設第一筆送貨者.

* 變動時則清除送貨者.

### 2.車次 (TN006)


### 3.送貨者 (TN007)

* 不可空白,可不存在.

* 可點選(icon)或[F2]廠商資料查詢(PURMA-02).所輸入之送貨者存在資料庫者,則預設送貨者名稱,主要路線,限制材積,限重,單件運費,材積單位.

* 運送方式設為「7.自送」時,可點選(icon)或[F2]派車資料查詢.


### 4.送貨者名稱 (TN008)

* 此欄位僅作顯示. 

### 5.主要路線 (TN009)

* 可點選(icon)或[F2]交易對象分類資料查詢.自動顯示簡稱.

### 6.備註 (TN022)

* 可點選(icon)或[F2]片語資料查詢.

### 7.廠別 (TN010)

* 輸入時預設「廠別資料檔」的第一筆,連續新增時自動預設前一筆廠別.

* 可點選(icon)或[F2]廠別資料查詢,並顯示廠別名稱.但不允許空白.

### 8.派車日期 (TN023)

* 此欄位僅作顯示.

### 9.列印次數 (TN022)

* 系統自動以零開始累計列印次數.

### 10.傳送次數 (TN026)

* 記錄FAX/MAIL之傳送累計次數.
 
### 11.確認碼 (TN021)

* 隱藏不可輸入,僅顯示圖示.

* 預設N.

### 12.簽核狀態碼 (TN025)

* 此欄位僅作顯示.簽核狀態碼類型區分為0.待處理,1.簽核中,2.退件,3.已核准,4.取消確認中,5.作廢中,6.取消作廢中,N.不執行電子簽核.


### 13.確認者 (TN024)

* 此欄位僅作顯示,自動顯示人員名稱.
 

## 單頭：其他資料

### 1.發票聯數 (TN011)

* 計1.二聯式,2.三聯式,3.二聯式收銀機發票,4.三聯式收銀機發票,5.電子計算機發票及6.免用統一發票六種.

### 2.課稅別 (TN012)

* 計1.應稅內含,2.應稅外加,3.零稅率,4.免稅及9.不計稅五種.

### 3.營業稅率 (TN013)

* 預設初值為「共用參數設定」之營業稅率.

* 顯示與輸入時以 5.00% 顯示.但存入時將資料除以 100 存入 0.0500.

* 此欄位必須大於等於零.
 
### 4.付款條件代號 (TN016)

* 可點選(icon)或[F2]付款條件代號查詢,並自動顯示付款條件名稱.


### 5.限制材積 (CUFT) (COP.MI006)

* 此欄位僅作顯示. 

### 6.限重(Kg) (COP.MI007)

* 此欄位僅作顯示. 

### 7.材積單位 (TN027)

* 此欄位僅作顯示.

* 分為1.英制(CUFT)及2.公制(CBM)兩種材積單位.

## 單身

### 1.序號 (TO003)

* 自動賦予.
 

### 2.路線別 (TO015)

* [F2]交易對象分類資料查詢,並自動顯示簡稱.

### 3.來源 (TO004)

* 計1.銷貨,2.庫存異動單據,3.轉撥單據,4.借出入單據及5.借出入歸還單據來源類型.

### 4.憑證單別 (TO005)

* 不可空白.變動時清除憑證單號.

* 來源為「銷貨」時,點選[F2]銷貨資料查詢.

* 來源為「庫存異動單據」時,點選[F2]庫存異動單據資料查詢.

* 來源為「轉撥單據」時,點選[F2]轉撥資料查詢.

* 來源為「借出入單據」時,點選[F2]借出入資料查詢.

* 來源為「借出入歸還單據」時,點選[F2]借出入歸還資料查詢.

* 依上述開窗預設憑證單別,憑證單號.

### 5.憑證單號 (TO006)

* 不可空白.

* 來源傳入「1.銷貨」->'1',「2.異動單據」->'2',「3.轉撥單據」->'2',「4.借出入單據」->'3',「5.借出入歸還單據」->'4',對應出本單材積,本單重量資料.

### 6.對象代號 (TO007)

* 此欄位僅作顯示.

### 7.對象簡稱 (TO008)

* 此欄位僅作顯示.

### 8.部門代號 (TO018)

* 此欄位僅作顯示. 

### 9.部門名稱 (CMS.ME002)

* 此欄位僅作顯示.

### 10.業務人員 (TO009)

* 此欄位僅作顯示.

### 11.件數(TO010)

* 總運費=單件運費*件數.預設未稅運費,運費稅額.

* 此欄位必須大於等於零.

### 12.本單材積 (TO011)

* 此欄位必須大於等於零.

 ### 13.本單重量 (TO012)

* 此欄位必須大於等於零.

### 14.單件運費 (TO013)

* 預設單件運費=單頭送貨者依條件取得之單件運費

* 總運費=單件運費*件數.預設未稅運費,運費稅額.

* 此欄位必須大於等於零.

### 15.總運費 (TO014)

* 此欄位必須大於等於零.

* 預設未稅運費,運費稅額.

### 16.未稅運費 (TO023)

* 此欄位僅作顯示.

### 17.運費稅額 (TO022)

* 此欄位必須大於等於零.

* 課稅別採「應稅內含」時,未稅運費=總運費-運費稅額.

### 18.備註 (TO016)

* [F2]片語資料查詢.

### 19.應付憑單別 (TO019)

* 此欄位僅作顯示.

### 20.應付憑單號 (TO020)

* 此欄位僅作顯示.

### 21.應付憑單序號 (TO021)

* 此欄位僅作顯示.

### 22.確認碼 (TO017)

* 隱藏欄位.預設N.

### 23.結帳碼 (TO024)

* 此欄位僅作顯示.預設N.


## 單尾

### 1.本幣運費金額 (TN014)

* 由單身未稅運費的累計.

* 本幣合計=本幣運費金額+本幣運費稅額.
 

### 2.本幣運費稅額 (TN015)

* 由單身運費稅額的累計. 

### 3.本幣合計

* 此欄位僅作顯示. 

### 4.累積材積(CUFT) (TN018)

* 由單身本單材積的累計.
 
### 5.累積重量(Kg) (TN019)

* 由單身本單重量的累計.

 
### 6.總件數 (TN017)

* 由單身件數的累計. 
