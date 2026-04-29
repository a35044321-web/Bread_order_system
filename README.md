# 🛠 天天新鮮麵包訂購系統 (Bakery Order System)

本專案是一個基於 **Java** 開發的全端訂單管理系統，旨在將餐飲實務邏輯轉化為數位化管理工具。系統遵循 **MVC (Model-View-Controller)** 架構設計，確保程式碼具備高度的擴充性。

---

## 📸 系統功能展示 (System Showcases)

### 🔐 身份驗證系統
整合式入口設計，透過後端邏輯判斷角色並導向對應功能頁面。
<div align="center">
  <img src="images/employee_login_ui.png" width="350px">
  <img src="images/customer_register_ui.png" width="300px">
  <p><i>左：整合登入入口 / 右：客戶註冊介面</i></p>
</div>

### 🛒 訂購流程展示
支援即時金額運算，並採用「UI 緩存、批次寫入」機制。
<div align="center">
  <img src="images/customer_order_ui.png" width="800px">
  <p><i>訂購主介面：即時運算總價並暫存於 JTable</i></p>
</div>

### ⚙️ 後台產品管理 (CRUD)
管理員專屬介面，實現產品數據的完整生命週期管理。
<div align="center">
  <img src="images/product_add_ui.png" width="230px">
  <img src="images/product_update_ui.png" width="230px">
  <img src="images/product_delete_ui.png" width="230px">
  <p><i>產品新增、修改、刪除操作彈窗</i></p>
</div>

---

## 🌟 技術核心拆解 (Technical Highlights)

為了達到世界級的開發水準，本專案在底層邏輯做了以下優化：

### 第一步：架構分層 (MVC Architecture)
將系統拆解為三個核心區塊，實現「關注點分離」：
* **Model (模型)**：封裝實體類（如 Customer, Product），定義數據結構。
* **View (視圖)**：利用 Java Swing 建立互動 UI，不包含任何業務邏輯。
* **Controller (控制器)**：負責橋接 UI 與資料庫，處理所有的事件監聽與轉發。

### 第二步：效能優化 (Batch DB Writing)
**挑戰關卡**：如果使用者點一下就寫入一次資料庫，會造成極大的效能浪費。
**解決方案**：
我設計了「緩存寫入」邏輯。使用者在介面選購時，資料僅儲存在內部的 `DefaultTableModel` 中，直到按下「確認送出」，系統才會啟動 JDBC 批次處理，一次性更新 MySQL 資料庫。

### 第三步：數據安全 (Data Integrity)
* **SQL 注入防禦**：全面使用 `PreparedStatement` 處理 SQL 語句。
* **邏輯校驗**：在寫入資料庫前，於 Controller 層進行防錯檢驗（例如：數量不可為負值）。

---

## 👨‍🏫 核心程式碼教學：批次訂單處理

這段邏輯是系統的心臟，負責將緩存區的「待送出」訂單正式轉為資料庫紀錄：

```java
// 1. 取得表格總列數
int rowCount = tableModel.getRowCount();

// 2. 遍歷清單尋找待處理項目
for (int i = 0; i < rowCount; i++) {
    // 檢查第一欄狀態是否為「待送出」
    if ("待送出".equals(tableModel.getValueAt(i, 0))) {
        
        // 3. 實例化數據模型
        model.Orders order = new model.Orders();
        order.setOrders_no("ORD" + (startId + i)); 
        order.setProduct_no((String) tableModel.getValueAt(i, 7));  
        order.setAmount((int) tableModel.getValueAt(i, 4));
        
        // 4. 透過 Controller 執行 JDBC 存取邏輯
        orderController.processOrder(order); 
    }
}
