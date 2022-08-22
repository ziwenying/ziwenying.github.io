---
title: 【觀念 01】在瀏覽器輸入網址並送出後到顯示出網頁，這背後發生了什麼事情？
date: 2022-08-22 13:48:50
tags:
  - [HTTP]
categories:
  - [網路基礎觀念]
---

當使用者在瀏覽器上輸入網址並送出後，會發生以下的事情：

# 瀏覽器解析輸入的網址

網址（Uniform Resource Locator，URL），即「統一資源定位符」，瀏覽器會從中解析出必要的資訊。例如：

<!-- more -->

- 通訊協定（Protocol）：http、https 等等。
- 網域名稱（Domain）：網域名稱會需要做 DNS 解析。
- 資源路徑（Path）：存於伺服器上的檔名路徑，例如：/user/home。

![image](https://github.com/ziwenying/ziwenying.github.io/blob/main/2022/08/22/concept-01/URL.png?raw=true)

# DNS 解析

**網域名稱系統（Domain Name System）**是一項服務，可以將域名和 IP 位址相互對應。
假如前面瀏覽器解析出合法的 URL，就會向 DNS 發出請求來取得此 URL 所對應的 IP 位址；至於非合法的 URL，瀏覽器通常會導去搜尋引擎的入口頁進行搜尋。

在正式向 DNS 發送請求前，瀏覽器會先從暫存中尋找有沒有該網域的資訊，如果有快取就使用快取。最後假如都沒有找到該網域的資訊， ISP （Internet Service Provider）就會正式發出 DNS 解析。

# 建立 TCP/IP 連線

從 DNS 得到網域對應的 IP 位址後，瀏覽器會和網域的伺服器建立透過三次握手建立
TCP/IP（Transmission Control Protocol / Internet Protocol）連線。
**為什麼是三次？**主要為了確保資料在傳輸過程中不會出錯。

**客戶端向伺服器端發送請求（第一次握手）。**
→ 伺服器（伺服器端）確認自己可以從瀏覽器（客戶端）接收資料。

**伺服器端收到請求後，向客戶端發送回應（第二次握手）。**
→ 瀏覽器確認伺服器有接收到傳過去的資料。
→ 瀏覽器確認可以接收到伺服器回傳的資料。

**客戶端收到伺服器端的回應後，再次回傳確認資訊給伺服器（第三次握手）。**
→ 伺服器確認瀏覽器可以接收傳過去的資料包。

**㊗️㊗️㊗️成功建立連線！㊗️㊗️㊗️**
上述流程主要是為了確認「客戶端」和「伺服器端」雙方的**發送與接收**功能都正常。

# 瀏覽器發起請求，伺服器處理並返回 HTTP 回應。

伺服器確認請求是否合法有資格取得資源，  
並將結果與資料（不合法就不會取得資料）以 HTTP response 返回。

## 常見 HTTP 狀態碼介紹：

- 1XX：某種資訊，表示請求被接受，但需要進行處理，例如：100。
- 2XX：請求成功被接受。
- 3XX：重新導向，例如：301 （永久導向），表示請求的資源已經被移動到新的位置。
- 4XX：表示客戶端錯誤，例如：401（拒絕存取）、404 （找不到此頁）。
- 5XX：伺服器發生錯誤，例如：500 。

# 瀏覽器渲染畫面

瀏覽器開始解析伺服器回傳的資料：

1. 解析 HTML 和 CSS 檔案，分別產生 DOM (Document Object Model) 和 CSSOM (CSS Object Model)
2. 將 DOM 和 CSSOM 合併為 Render Tree（渲染樹），Render Tree 會計算元素的大小、顏色在畫面的位置等，並產生 Layout（布局）。
3. 最後開始繪製細節畫面（Paint），顯現給使用者。

---

**參考資料：**
[Summer。桑莫。夏天 - 在瀏覽器輸入網址並送出後，到底發生了什麼事？](https://cythilya.github.io/2018/11/26/what-happens-when-you-type-an-url-in-the-browser-and-press-enter/#%E4%B8%80%E7%80%8F%E8%A6%BD%E5%99%A8%E7%9A%84%E5%85%A7%E9%83%A8%E9%81%8B%E4%BD%9C%E6%A9%9F%E5%88%B6 "Summer。桑莫。夏天")
[Gary - [WEB] 從輸入網址列到渲染畫面，過程經歷了什麼事情？](https://ithelp.ithome.com.tw/articles/10228442 "Gary")
[Shubo 的程式開發筆記 - 經典前端面試題：從瀏覽器網址列輸入 URL 按下 enter 發生了什麼？](https://shubo.io/what-happens-when-you-type-a-url-in-the-browser-and-press-enter/ "Shubo")
