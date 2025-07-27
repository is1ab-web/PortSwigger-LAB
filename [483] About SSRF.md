# PortSwigger：About SSRF

## <span class="red">Introduction</span>

- **<span class="light_purple">目標</span>：寄送 request 到 非預期的地方**

- **<span class="purple">一般來說</span>：連接至 <span class="red">internal-only 類型</span>的服務**

- **<span class="purple">其他情況</span>：強迫 server 連接至任意的 <span class="red">external</span> 系統 → 敏感資料洩漏**

&emsp;

## <span class="red">Impact</span>

- **Result in：**
    
    - (1) **<span class="purple">未經授權的動作</span>**

    - (2) **<span class="purple">組織內部資料存取權</span>**
    
    - (3) **<span class="purple">可執行任意指令</span>**

&emsp;

## <span class="red">Common SSRF attacks</span>

### <span class="red">SSRF attacks against the server</span>

- **<span class="light_purple">目標</span>：透過網路介面的 loopback (回送，一種檢驗機制)來向應用程式所在的 server 回傳 HTTP 請求**

- **<span class="red">PortSwigger Lab</span>：https://hackmd.io/@AndyShen/S1jFhatF1x**

- **應用程式的行為緣由？**
    
    - (1) **存取控制的檢查是<span class="blue">由前端 server 實作</span>：**
        - **前端組件 (API Gateway) 負責身份驗證，只有授權的請求才會被轉發到後端 server**
        
        - **後端 app server 接收 API Gateway 轉發的請求並提供服務**
        
        - **用戶 --> API Gateway (檢查權限) --> app server (執行請求)**
        
        - **如果 app server 沒有進行額外身份驗證，攻擊者就能夠直接向 app server 發送請求，從而繞過 API Gateway 的存取控制**
          - **例如： `curl http://backend-server/secret-data`**

    - (2) **基於<span class="light_purple">災後重建</span>的目標： 當管理員<span class="blue">遺失憑證</span>時，可以提供給管理員修復系統**
    
    - (3) **管理員介面可能會監聽<span class="red">不同的 ports</span> (通常無法被使用者直接接觸到)**

&emsp;

### <span class="red">SSRF attacks against the other back-end systems</span>

- **特徵：**

    - **應用程式的 server <span class="blue">能夠</span>與使用者無法接觸的<span class="blue">後端溝通</span>** 

    - **這些系統通常配置了<span class="blue">私有且不可路由(路由 : route)的 IP 位址</span>** 

    - **由於後端系統通常依賴網路拓撲進行保護，所以<span class="blue">安全防禦較為薄弱</span>**

- **<span class="red">PortSwigger Lab</span>：https://hackmd.io/@AndyShen/rJHrsRtKkg**

&emsp;

## <span class="red">Circumventing (規避) common SSRF defenses</span>

### <span class="red">SSRF with <span class="light_purple">blacklist-based</span> input filters</span>：

- **利用以下技巧規避<span class="light_purple">黑名單</span>輸入過濾：**

    - **利用 ```127.0.0.1``` 的<span class="blue">不同表示法</span>**
    **(such as ```2130706433```, ```017700000001```, or ```127.1```)**
    **(```127.0.0.1``` = (01111111 00000000 00000000 00000001)~2~ = (2130706433)~10~ = (0177 0000 0001)~8~ )**

    - **註冊<span class="blue">一個自訂的網域</span>並讓它指向 ```127.0.0.1```**
    **(Register your <span class="blue">own domain name</span> that resolves to 127.0.0.1)**

    - **利用 <span class="blue">URL encoding</span> 或是 <span class="blue">case variation</span> 混淆 (Obfuscate) 遭到過濾的字串 (blocked strings)**

    - **提供一個可控的 URL 會<span class="blue">重新導向</span>到目標 URL**
    **(舉例來說：在重新導向的過程中，將 URL 從 ```http:``` <span class="purple">轉換為</span> ```https:```  ，<span class="purple">用以繞過</span> SSRF 的過濾)**

- **<span class="red">PortSwigger Lab</span>：https://hackmd.io/@AndyShen/rJg-n8lqye**

### <span class="red">SSRF with <span class="light_purple">whitelist-based</span> input filters</span>

- **有些應用程式只允許<span class="light_purple">白名單</span>輸入**

- **輸入很可能<span class="blue">從一開始</span>就被過濾，或是檢查有沒有包含特定字串。可以利用 <span class="blue">URL 解析不一致性</span>去繞過這些過濾方法**

- **URL 的規範中有許多容易被忽視的細節，尤其是在採用<span class="blue">臨時實作</span>的方式來解析，或是用下列方法驗證 URL ：**
    - **在 URL 中<span class="light_purple">透過 @ 字元</span>，在<span class="blue">主機名稱<span class="red">前</span>插入帳號與密碼</span>等憑證資訊**
        - **例如：```https://expected-host:fakepassword@evil-host```**

    - **在 URL 中<span class="light_purple">透過 # 字元</span> 來表示 URL 的片段識別符 ( <span class="blue">混淆</span>或<span class="blue">欺騙</span>伺服器處理的 URL )**
        - **例如：```https://evil-host#expected-host```**

    - **<span class="light_purple">利用 DNS 的命名階層</span>，把需要的輸入<span class="blue">嵌入自己控制的 DNS 名稱</span>中**
        - **例如：```https://expected-host.evil-host```**

    - **<span class="light_purple">利用 URL-encode 的技巧</span>來<span class="blue">混淆</span>負責解析 URL 的 code ，這種方法對於「<span class="blue">處理 URL-encoded 字符過濾的 code</span>」 與 「<span class="blue">處理後端 HTTP req 的 code</span>」 <span class="red">有所差異</span>時特別有效。也可以<span class="light_purple">利用 double-encoding</span> 的技巧造成<span class="red">不一致性</span> ，這種方法專門用來應對那些<span class="blue">對接收資料重複 URL-decoding</span> 的伺服器**

    - **嘗試將以上方法<span class="red">混和使用</span>！！**

- **<span class="red">PortSwigger Lab</span>：https://hackmd.io/@AndyShen/BkcEZWLkee**

### <span class="red">Bypassing SSRF filters via <span class="light_purple">open redirection</span></span>

- **有時候可以利用<span class="light_purple">開放重新導向漏洞</span>來繞過基於 filter 的防禦**

- **可以藉由構造一個符合過濾條件的網址，讓請求被<span class="blue">重導向到想要的後端目標</span>**
    
    - **以下例子會重新導向至 ```http://evil-user.net```**
        
        - **```.../product/nextProduct?currentProductId=6&path=http://evil-user.net```**
    
    - **把上面的例子延伸，就能用來繞過針對 DNS name 的 URL filter**
        
        ```html=
        POST /product/stock HTTP/1.0
        Content-Type: application/x-www-form-urlencoded
        Content-Length: 118

        stockApi=http://weliketoshop.net/product/nextProduct?currentProductId=6&path=http://192.168.0.68/admin
        ```
        
    - **上述例子要成功最重要的是<span class="red">提供的stockAPI網址</span>是否屬於<span class="red">允許的網域</span>**

- **<span class="red">PortSwigger Lab</span>：https://hackmd.io/@AndyShen/B1OrmtPkxe**

&emsp;

## <span class="red">Blind SSRF vulnerabilities</span>

- **Blind SSRF 漏洞通常發生在「使用者<span class="blue">能讓應用程式向你提供的 URL 發送後端 HTTP request</span> ，但是該 request 的 response <span class="red">不會出現在前端(的 response) 當中</span>」**

- **這種類型的漏洞本身難以利用，但是一旦遭到利用就有可能在 <span class="red">server 或其他後端元件</span>中造成 <span class="red">RCE</span>**

### <span class="red">How to find and exploit blind SSRF vulnerabilities?</span>

- **偵測<span class="red">有無 blind SSRF 漏洞</span>最可靠的方法是用 <span class="light_purple">out-of-band(OAST=<span class="red">O</span>ut-of-band <span class="red">A</span>pplication <span class="red">S</span>ecurity <span class="red">T</span>esting)</span> 的技巧，主要是利用<span class="blue">向攻擊者控制的外部系統</span>發送 HTTP request ，然後監控<span class="blue">與該系統的網路互動行為</span>**

- **在測試 SSRF 漏洞時，常會觀察到針對提供的<span class="red">特定域名</span>進行 DNS 查詢，但後續卻<span class="red">無 HTTP request</span> 的現象。這種情況通常發生於應用程式<span class="blue">嘗試向目標 DNS name 發起 HTTP request</span> 而觸發初始 DNS 查詢，但實際的 HTTP 連線卻<span class="red">被網路層過濾機制</span>(在 OSI 模型的第三層（網路層）或第四層（傳輸層）進行的封包過濾)<span class="red">阻擋</span>**

&emsp;

## <span class="red">Finding hidden attack surface for SSRF vulnerabilities</span>

- **由於應用程式的正常流量會包含<span class="red">帶有完整 URL 的請求參數</span>，大部份的 SSRF 漏洞很容易被找到**

### <span class="red"><span class="light_purple">Partial URLs</span> in requests</span>

- **有時候應用程式<span class="red">只會將 hostname</span> 或是<span class="red">部分的 URL 放入 request 參數當中</span>，這些值隨後會<span class="blue">在 server 端被組合成一個完整的 URL</span> 並發送請求。因此，儘管這些值很容易被辨識為 hostname 或是 URL 路徑(潛在攻擊面很廣)，仍會因為<span class="red">無法控制最終被請求的整個 URL</span> 導致 SSRF 的可利用性受到限制**

### <span class="red">URLs within <span class="light_purple">data formats</span></span>

- **有些應用程式會以<span class="red">允許包含 URL 的格式</span>來傳輸資料，而這些 URL 可能會<span class="blue">被該格式的資料的<span class="red">解析器</span></span>發出請求，從而導致某些安全漏洞。像是 XML 資料格式，它在<span class="blue">網頁應用</span>中被廣泛用來在用戶端與伺服器之間<span class="blue">傳遞結構化資料</span>，當應用程式接收 XML 格式的資料並進行解析時就有可能出現 XXE Injection ，也可能因 XXE 而導致 SSRF**

### <span class="red">SSRF via the <span class="light_purple">Referer header</span></span>

- **某些應用程式使用<span class="blue">伺服器端分析軟體</span>來追蹤訪客行為。這類軟體通常會記錄請求中的 Referer 標頭，以便追蹤外部連結來源。分析軟體常會<span class="blue">主動訪問 Referer 標頭中出現的第三方 URL</span> ，目標是分析來源網站的內容，其中包括外部連結所使用的錨點文字。因此，<span class="red">Referer 標頭往往成為 SSRF 的重要攻擊面</span>**

- **註：錨點文字是 ```<a>``` 標籤中的可視文字內容，常見的像是 ```<a href="#top">↑<\a>``` 中， ```↑``` 就是錨點文字 (```#top``` 屬於片段識別符， SSRF 繞過白名單過濾的技巧中有提到過)**

&emsp;

## <span class="red">PortSwigger SSRF cheatsheet</span>

- **URL：https://portswigger.net/web-security/ssrf/url-validation-bypass-cheat-sheet**
