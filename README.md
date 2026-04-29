# 🛠 天天新鮮麵包訂購系統 (Bakery Order System)

本專案是一個基於 **Java** 開發的全端訂單管理系統，旨在解決麵包店日常營運中的會員管理、產品上架與批次訂購需求。系統嚴謹遵循 **MVC (Model-View-Controller)** 架構設計。

---

## 📸 系統功能展示 (System Showcases)

### 🔐 身份驗證系統 (整合入口設計)
系統將不同角色整合於同一 UI，確保維護效率與介面簡潔。
<div align="center">
  <img src="images/employee_login_ui.png" width="350px">
  <img src="images/customer_register_ui.png" width="300px">
  <p><i>左：員工與客戶登入入口 / 右：客戶註冊介面</i></p>
</div>

### 🛒 訂購流程展示
支援即時金額運算，並採用批次處理機制將數據寫入資料庫。
<div align="center">
  <img src="images/customer_order_ui.png" width="800px">
  <br>
  <img src="images/order_success_ui.png" width="400px">
  <p><i>上：主訂購介面（含緩存清單） / 下：訂單成功寫入資料庫之反饋</i></p>
</div>

### ⚙️ 後台產品管理 (CRUD)
提供管理員完整的功能，對產品數據進行新增、修改與刪除。
<div align="center">
  <img src="images/product_management_ui.png" width="700px">
  <br><br>
  <img src="images/product_add_ui.png" width="230px">
  <img src="images/product_update_ui.png" width="230px">
  <img src="images/product_delete_ui.png" width="230px">
  <p><i>由左至右：產品新增、更新、刪除操作彈窗</i></p>
</div>

---

## 🌟 技術棧與架構設計

* **核心語言**：Java (JDK 11)
* **後端技術**：JDBC (Java Database Connectivity)
* **資料庫**：MySQL 8.0
* **前端介面**：Java Swing (桌面級互動 UI)
* **架構優化**：
    * **資料庫 I/O 優化**：設計「先暫存於 JTable，後批次寫入」的策略，降低對資料庫的頻繁請求。
    * **MVC 模式**：實現資料存取邏輯 (DAO) 與業務邏輯 (Controller) 解耦。

---

## 👨‍🏫 核心邏輯展示：批次訂單處理
```java
// 遍歷 JTable 清單，將「待送出」資料由 Controller 批次寫入 MySQL
for (int i = 0; i < rowCount; i++) {
    if ("待送出".equals(tableModel.getValueAt(i, 0))) {
        model.Orders order = new model.Orders();
        order.setOrders_no("ORD" + (startId + i)); 
        order.setProduct_no((String) tableModel.getValueAt(i, 7));  
        order.setAmount((int) tableModel.getValueAt(i, 4));
        orderController.processOrder(order); 
    }
}
