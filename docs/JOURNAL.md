# BlindGuard 專案工作日誌（JOURNAL.md）

本文件用於記錄各階段的專案進度、問題、調整紀錄與重要學習。

---

```
## 2025-5-20（二）

### 記錄人員：Eric

### 今日進度：
- 決定專案初版架構圖，架構圖檔可參照 docs/images/architecture_v1.png
- 成功轉換 BDD100K JSON ➝ YOLO txt 格式
- 決定YOLO v8 影像標籤種類，並建立bdd100k.yaml
- 將 convert_bdd_to_yolo.py 移至 datasets/ 並優化路徑
- 更改專案名稱為 Driver Mind AI

---
15:30跟老師討論後，改成以下方式實作：

1.定義 ROI（Region of Interest）區域座標
    •可用 OpenCV 或程式碼畫出 trapezoid/rectangle
    •例如設定風險區為畫面中下方 1/3 的區域
    
2.每幀 YOLO 推論出 bounding box
    •判斷物件的~~中心點~~(可自定義碰撞體積)是否落在 ROI 區域內
    
3.分區等級邏輯（舉例）
    •ROI 分為紅區（近）、橙區（中）、綠區（遠）
    •根據落點層級輸出「高風險」「中風險」回饋
    
4.加值功能（選用）：
    •可用 OpenCV cv2.polylines / cv2.fillPoly 視覺化警示區域
    •用 pyttsx3 或文字輸出提示：「前方高風險物件靠近」
---

### 學習內容：
- 了解 YOLO 格式標註需為中心點與寬高（normalized）
- 釐清YOLO 標籤框的碰撞交集判定(IoU)，決定好所使用的標籤類別

### 問題與修復：
- 錯誤：join() argument must not be NoneType
- 解法：從 JSON 檔名推回圖片名稱，而不是取錯誤的欄位

### 待辦事項：
- [ ] 台灣行車資料收集與特徵工程
- [ ] 整合 pyttsx3 警示語音模組
- [ ] 加入臉部視覺辨識
- [ ] 加入模型訓練流程（YOLOv8 CLI）
- [ ] 訓練日誌與 checkpoint 儲存
```

---

```
## 2025-5-21（三）

### 記錄人員：劉元新

### 今日進度：
- BDD100K資料集過於龐大，經測試colab環境難以負荷訓練，討論後改用BDD10K訓練。
- 決定YOLO v8 影像標籤種類，並建立bdd10k.yaml
- 討論後定義各類別台灣行車影像畫框範圍(在roboflow平台實作)，統一規範標記格式
- 開始進行roboflow切幀、畫框作業，製作五百張台灣行車紀錄
- 初步進行資料集EDA
- 用segmentation嘗試訓練，結果CPU無法負荷需要GPU加速，討論後改用openCV定義警戒區範圍。
- 用openCV依據分隔線做警示區 
---

### 學習內容：
- 了解模型訓練的環境資源配置/資料集大小影響
- 釐清類別畫框區域（影響後續物件偵測靈敏度與物件碰撞範圍）
- 模型訓練五百張耗費一小時，最終決定用三千到五千張資料做最終訓練
- 嘗試釐清風險RoI的判斷標準


### 問題與修復：
- 

### 待辦事項：
- [ ] 台灣行車資料收集與特徵工程
- [ ] 整合 pyttsx3 警示語音模組
- [ ] 加入臉部視覺辨識
- [ ] 加入模型訓練流程（YOLOv8 CLI）
- [ ] 訓練日誌與 checkpoint 儲存
- [ ] 測試台灣行車紀錄五百份資料訓練
- [ ] 前端展示
- [ ] 風險判定
```
