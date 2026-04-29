# 🛠 天天新鮮麵包訂購系統 (Bakery Order System)

本專案是一個基於 **Java** 開發的全端訂單管理系統，旨在解決麵包店日常營運需求。

---

## 📸 系統功能展示 (System Showcases)

### 🔐 身份驗證系統
<div align="center">
  <img src="images/employee_login_ui.png" width="350px">
  <img src="images/customer_register_ui.png" width="300px">
  <p><i>左：員工與客戶登入入口 / 右：客戶註冊介面</i></p>
</div>

### 🛒 訂購流程展示
支援即時金額運算，並採用批次處理機制將數據寫入資料庫。
<div align="center">
  <img src="images/customer_order_ui.png.png" width="800px">
  <br>
  <img src="images/order_success_ui.png.png" width="400px">
  <p><i>上：主訂購介面 / 下：訂單成功寫入反饋（註：此二檔名含雙重副檔名）</i></p>
</div>

### ⚙️ 後台產品管理 (CRUD)
<div align="center">
  <img src="images/product_management_ui.png" width="700px">
  <br><br>
  <img src="images/product_add_ui.png" width="230px">
  <img src="images/product_update_ui.png" width="230px">
  <img src="images/product_delete_ui.png" width="230px">
  <p><i>管理員端：產品清單與 CRUD 操作介面</i></p>
</div>

---

## 🌟 技術棧與架構設計
* **語言與庫**：Java (JDK 11), Swing, JDBC
* **資料庫**：MySQL 8.0
* **架構優化**：採用 MVC 模式，並針對資料庫 I/O 進行批次寫入優化。

---

## 👨‍🏫 核心邏輯展示
```java
for (int i = 0; i < rowCount; i++) {
    if ("待送出".equals(tableModel.getValueAt(i, 0))) {
        model.Orders order = new model.Orders();
        order.setOrders_no("ORD" + (startId + i)); 
        order.setProduct_no((String) tableModel.getValueAt(i, 7));  
        orderController.processOrder(order); 
    }
}
