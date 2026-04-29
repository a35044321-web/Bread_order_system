# 🛠 天天新鮮麵包訂購系統 (Bakery Order System)

本專案是一個基於 **Java** 開發的全端訂單管理系統，旨在解決麵包店日常營運中的會員管理、產品上架與批次訂購需求。系統嚴謹遵循 **MVC (Model-View-Controller)** 架構設計，確保程式碼具備高度的可擴充性與維護性。

---
https://github.com/a35044321-web/Bread_order_system/blob/main/README.md
## 🌟 技術棧與架構設計

* **核心語言**：Java (JDK 11)
* **後端框架**：JDBC (Java Database Connectivity)
* **資料庫**：MySQL 8.0
* **前端介面**：Java Swing (桌面級互動 UI)
* **軟體架構**：
    * **Model**：封裝資料實體 (Entity)，如 `Customer`, `Product`, `Orders`。
    * **View**：提供直觀的互動介面，處理使用者輸入與即時反饋。
    * **Controller**：調度 UI 事件與 Service 邏輯。
    * **DAO (Data Access Object)**：專責處理 SQL 指令與資料庫持久化，實現邏輯解耦。

---

## 🚀 核心功能與技術亮點

### 1. 會員管理系統 (Registration & Login)
* **帳號校驗機制**：在註冊流程中，系統會透過 `find_customerdata_by_user_account` 方法即時查詢資料庫，防止重複帳號註冊，並給予使用者對應的 UI 提示。
* **多權限登入**：區分客戶端與員工端登入邏輯，根據角色導向不同功能頁面。

### 2. 批次訂購與即時運算 (Order Processing)
* **實時金額計算**：利用事件監聽器，當使用者更改產品數量時，系統會自動運算總金額並即時更新於 UI。
* **快取機制優化**：採用「先暫存於 `JTable`，後批次寫入資料庫」的策略。此舉能有效降低資料庫 I/O 頻率，提升系統反應速度。
* **自動生成唯一編號**：使用時間戳記演算法生成 Order ID，確保每筆訂單在資料庫中的唯一性。

### 3. 產品管理後台 (Inventory CRUD)
* 提供管理員完整的新增、修改、刪除功能，並透過 `private key` 進行精準的資料更新，確保庫存數據一致。

---

## 📸 系統介面展示

### 【功能一：客戶訂購介面】
展示客戶如何透過下拉式選單選擇商品，系統自動連動售價並緩存於右側清單。
<div align="center">
  <img src="image_c38402.png" width="700px" alt="客戶訂購頁面">
</div>

### 【功能二：資料庫同步成功】
確認下單後，資料會經由 Controller 批次寫入 MySQL，並回傳成功的狀態。
<div align="center">
  <img src="image_c37d7b.png" width="700px" alt="訂購成功畫面">
</div>

---

## 👨‍🏫 核心程式碼片段：註冊邏輯判斷
```java
// 透過 Service 層判斷帳號是否存在，達成邏輯與 UI 分離
if(customer_service_impl.find_customerdata_by_user_account(customer_account_input.getText())) {
    // 封裝資料並調用 Service 新增會員
    Customer customer = new Customer(customer_no, customer_name, customer_phone_number, customer_adress, user_account, user_password);
    customerserviceimpl.add_customer(customer);
    output.setText("註冊成功!");
} else {
    output.setText("帳號重複，請重新輸入!");
}
