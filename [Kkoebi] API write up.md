# Burp suite - API
## API recon

在偵查API中，要先了解API相關的位置並確定位置，可以考慮`GET`請求：

```
GET /api/books HTTP/1.1
Host: example.com
```

確認節點後，就可以開始思考如何跟API交互，才可以利用有效HTTP請求來測試API，而其三個須知事項：
- API 處理的輸入數據，包括強制參數和可選參數。
- API 接受的請求類型，包括支援的 HTTP 方法和媒體格式。
- 速率限制和身份驗證機制。

## API documentation

通常記錄 API，以便開發人員知道如何使用，一般可以採用兩種：

- 人類可讀 -> 開發人員瞭解如何使用（ex：詳細的解釋、示例和使用場景）。
- 機器可讀 -> 軟體處理，以自動執行 API 集成和驗證（ex：JSON 或 XML 結構化格式）。

API 文件通常是公開可用的，如果在 API 供外部開發人員使用時，可透過查看文件來開始偵察。

即便API不公開，也可以透過瀏覽使用API的應用程式來存取他，在Burp suite中可利用Scanner來抓取API，以下是幾個可能引用API文件的端點：

- `/api`
- `/swagger/index.html`
- `/openapi.json`

此外，也要調查基本路徑，例如：

```
/api/swagger/v1/users/123
```

通常也可以使用Intruder功能來尋找。

### API Lab1 -> [write up](https://hackmd.io/@mio0813/Sy3H6D1yxg)

## Identifying and interacting with API endpoints
## Identifying API endpoints

在瀏覽應用程式過程中，可以留意URL的結構，其中可能會暗示跟API有關的端點，但有時也要注意JavaScript檔案，有時JS檔會包含未直接透過網頁而直接觸發API的可能性。

找到API端點後就可以利用剛剛上述提到的Repeater及Intruder來做互動，在互動過程 中，也須觀察錯誤訊息和其他回應，有些會包含可用來構造有效HTTP請求的資訊。

HTTP各種請求方法：
- GET - 擷取資料
- PATCH - 修改資料
- OPTIONS - 查詢資源支援哪些請求方法

:::info
在測試不同的 HTTP 方法時，請選擇低優先權的對象進行測試，這可以避免意外後果，例如：改動關鍵資料或產生過多紀錄。
:::

API通常預期資料會以特定格式傳送，如果要更改內容類型，我們可以修改`Content-Type`，而傳送資料的內容類型不同，API的行為也會有所差異，這也會導致我們發生下列三種可能：
- 觸發錯誤訊息，進而洩漏有用資訊
- 規避有瑕疵的防禦機制
- 利用處理邏輯

### API Lab2 -> [write up](https://hackmd.io/@mio0813/Byy5DFykee)

在前面有提到，可以使用 Intruder 來尋找一些隱藏的 API 端點，假設我們目前找到以下端點：

```
PUT /api/user/update
```
那我們可以想還會不會有其他端點，所以可以用暴力破解的方式來替換其他位置，就例如：

```
/api/user/delete  
/api/user/add  
/api/user/get  
```

而為了讓這個方法更有效率，應該使用一份「常見 API 名稱」的字典（wordlist）來幫你猜測那些可能存在的端點，另外也可以根據在前期資訊蒐集時得到的資訊，加入一些跟這個網站或應用程式特別相關的關鍵字。

## Finding hidden parameters

在進行 API 偵察時，可能會發現一些 API 支援但沒有文件記載的參數。你可以嘗試使用這些參數來改變應用程式的行為。

Burp Intruder：你可以用它自動找出隱藏參數。方法是使用一份常見參數名稱的字典，來取代現有的參數或加入新的參數。記得也要包含一些與應用程式相關的參數名稱，這些可以根據你前期的偵察結果來推測。

Param Miner 擴充套件（BApp）：這個工具可以幫你自動猜測每個請求最多 65,536 個參數名稱。Param Miner 會根據測試範圍（scope）中取得的資訊，自動猜測與應用程式相關的參數名稱。

內容探索工具（Content discovery tool）：這個工具能幫助我們發現一些不會直接顯示在頁面上、也無法直接瀏覽到的內容，其中也包含可能的參數。

## Mass assignment vulnerabilities

大量賦值（也叫 auto-binding，自動綁定）有時會無意間產生隱藏參數。這種情況發生在某些軟體框架會自動把請求中的參數綁定到程式裡的某個內部物件上。

結果就是：應用程式可能會意外地接受、處理一些開發者原本根本沒打算開放的參數，這就可能造成安全風險。

由於大量賦值是根據物件的欄位自動產生參數，所以可以透過手動檢查 API 回傳的物件內容，來找出那些被偷偷綁定的隱藏參數。

舉個例子來說：
你有一個 API 可以讓使用者更新自己的帳號資訊：

```
PATCH /api/users/
```

它會送出這樣的 JSON：

```json
{
    "username": "wiener",
    "email": "wiener@example.com"
}
```

這看起來沒什麼問題，但如果你再去打另一個 API：

```
GET /api/users/123
```

然後它回傳了這樣的資料：

```json
{
    "id": 123,
    "name": "John Doe",
    "email": "john@example.com",
    "isAdmin": "false"
}
```

id 跟 isAdmin 明明不是你 PATCH 裡出現的欄位，卻出現在回傳的物件裡，代表這些欄位可能也是內部使用的物件屬性，如果框架有支援 mass assignment，那攻擊者就有可能這樣偷偷送出請求：

```json
{
    "username": "hacker",
    "email": "hacker@example.com",
    "isAdmin": "true"
}
```
結果... 🤡 恭喜你變成管理員了。


## Testing mass assignment vulnerabilities

我們可以透過以下方法來測試是否能修改 isAdmin 這個參數的值，進而利用大量賦值漏洞。

### 第一步：嘗試加入 isAdmin 到 PATCH 請求中

送出以下 PATCH 請求：

```
{
    "username": "wiener",
    "email": "wiener@example.com",
    "isAdmin": false
}
```

看看應用程式有沒有接受這個參數，或是回傳任何異常行為。

### 第二步：傳送無效的 isAdmin 值，觀察反應

```
{
    "username": "wiener",
    "email": "wiener@example.com",
    "isAdmin": "foo"
}
```

如果應用程式的行為和剛才不同，代表它其實有處理這個參數，只是對某些值有不同邏輯，這可能暗示著，只要傳對值，它就會接受。

### 第三步：直接上真貨，送 isAdmin: true 嘗試升級權限

```
{
    "username": "wiener",
    "email": "wiener@example.com",
    "isAdmin": true
}
```

如果這個值被綁定到內部的 user 物件上，且系統沒有做適當的驗證或清洗（validation/sanitization），那麼使用者 wiener 就有可能被錯誤地賦予管理員權限。

### 最後驗證

登入 wiener 帳號試試看能不能使用 admin 功能，如果突然能看到後台、修改使用者資料或看到管理頁面那就完成了。

### API Lab3 -> [write up](https://hackmd.io/@mio0813/SJjgwYQyeg)

## Preventing vulnerabilities in APIs

在設計 API 時，確保從一開始就將安全性納入考量：

- 如果不打算讓你的 API 公開訪問，請確保保護好 API 文檔。
- 確保文檔保持最新，以便合法的測試者能全面了解 API 的攻擊面。
- 應用允許清單（allowlist），限制允許的 HTTP 方法。
- 驗證每個請求或回應的內容類型是否符合預期。
- 使用通用錯誤訊息，避免透露對攻擊者有用的資訊。
- 對所有版本的 API 進行保護，不僅僅是當前的生產版本。

為防止大量賦值漏洞，應對可以由用戶更新的屬性進行允許清單管理，並對不應該由用戶更新的敏感屬性進行阻擋清單管理。

## Server-side parameter pollution

有些系統包含內部 API，這些 API 並不直接從網路上可訪問。

伺服器端參數污染發生在當網站將用戶輸入嵌入到發送給內部 API 的伺服器端請求中，而未進行充分的編碼處理時，攻擊者可能能夠操控或注入參數，這可能讓他們做到這些事：

- 覆蓋現有的參數。
- 修改應用程序的行為。
- 訪問未授權的數據。

:::danger
這個漏洞有時也被稱為 HTTP 參數污染，這個術語也用來指代一種網頁應用防火牆（WAF）繞過技術。
:::

## Testing for server-side parameter pollution in the query string

要測試查詢字串中的伺服器端參數污染，可以將查詢語法字符放入輸入中，並觀察應用程序的回應。

假設有一個脆弱的應用程序，允許根據用戶名搜索其他用戶，當搜索某個用戶時，瀏覽器會發出以下請求：

```
GET /userSearch?name=peter&back=/home
```

為了搜索用戶信息，伺服器會向內部 API 發出以下請求：

```
GET /users/search?name=peter&publicProfile=true
```

在這種情況下，可以嘗試修改查詢字串中的 name 或 back 參數，並觀察伺服器如何處理它們，以檢查是否存在伺服器端參數污染的漏洞，如果參數未正確處理，可能會導致攻擊者操控請求，從而繞過授權或改變應用行為。

## Truncating query strings

可以使用 URL 編碼的 # 字元（%23）來嘗試截斷伺服器端的請求。為了幫助判斷是否成功截斷，也可以在 # 字元後加上一些辨識用的字串。

例如，你可以將查詢字串修改為：

```
GET /userSearch?name=peter%23foo&back=/home
```

前端將會嘗試訪問下列 URL：

```
GET /users/search?name=peter#foo&publicProfile=true
```

:::info
必須對 # 字元進行 URL 編碼（變成 %23），否則前端應用會把它當成 fragment identifier（片段識別符號），而不會將它傳給內部 API。
:::

1. 伺服器回應仍然顯示使用者 peter 的資料，那可能代表後端的查詢被截斷了（publicProfile=true 沒有生效）。

2. 伺服器回應錯誤訊息像是 "Invalid name"，表示應用可能把 foo 視為名稱的一部分，這代表請求沒有被截斷。

## Injecting invalid parameters

可以利用 URL 編碼的 & 字元（%26）來嘗試在伺服器端的請求中添加第二個參數。

例如，你可以將查詢字串改成：

```
GET /userSearch?name=peter%26foo=xyz&back=/home
```

這會導致前端組出以下的內部 API 請求：

```
GET /users/search?name=peter&foo=xyz&publicProfile=true
```

接下來檢查伺服器的回應，觀察新增的參數（foo=xyz）是如何被解析的：

1. 回應沒有變化，這可能表示參數被成功注入但被應用程式忽略了。
2. 出現錯誤或行為改變，那表示這個參數可能被應用程式解析和使用了。

## Injecting valid parameters

如果已經能夠修改查詢字串（query string），那就可以嘗試加入第二個有效參數，進一步干擾伺服器端的請求邏輯。

假設你已經發現了系統有支援 email 這個參數，那你就可以這樣改寫請求：

```
GET /userSearch?name=peter%26email=foo&back=/home
```

這樣在伺服器端，可能會被解析成：

```
GET /users/search?name=peter&email=foo&publicProfile=true
```

你要觀察伺服器的回應，看看新加進去的參數 email=foo 是否：

- 有被接受（改變了搜尋結果）
- 有被解析但被忽略（結果沒變）
- 還是直接出現錯誤（代表有驗證或阻擋）

## Overriding existing parameters

要確認應用程式是否容易受到伺服器端參數汙染（Server-side Parameter Pollution, SSPP）攻擊，可以試著「覆蓋原本的參數」，也就是注入一個同名的第二個參數。

修改原始查詢字串如下：

```
GET /userSearch?name=peter%26name=carlos&back=/home
```

這會讓伺服器端的內部 API 接收到這樣的請求：

```
GET /users/search?name=peter&name=carlos&publicProfile=true
```

而結果分析會根據不同技術而有不同的解析方式：

- PHP - 只取最後一個參數 → name=carlos
- ASP.NET - 合併兩個值 → name=peter,carlos → 可能會出現「Invalid username」錯誤
- Node.js / Express - 只取第一個參數 → name=peter → 結果可能沒有變

如果你成功覆蓋參數，就有機會進一步利用，例如可以這樣做：

```
GET /userSearch?name=peter%26name=administrator&back=/home
```

→ 如果後端只取第二個參數，說不定你就變成登入 administrator 了。

:::danger
SSPP 的威力來自於不同語言或框架對「多個同名參數」的解析行為不同，這給了攻擊者操控後端邏輯的機會。配合發現隱藏參數、觀察 API 結構等技巧，可以讓這種攻擊變得超有戲！
:::

### API Lab4 -> [write up](https://hackmd.io/@mio0813/H1Uaw9X1xl)

## Testing for server-side parameter pollution in REST paths

一個 RESTful API 可能會將參數名稱與數值放在 URL 路徑中，而非查詢字串（query string）。舉例來說，可以參考以下路徑：

```
/api/users/123
```

這個 URL 路徑可以被拆解如下：

- `/api` 是 API 的根端點（root endpoint）。
- `/users` 代表一個資源，在這裡是使用者（users）。
- `/123` 代表一個參數，在這裡是一位特定使用者的識別碼（ID）。

想像一個應用程式，它允許你根據使用者名稱編輯個人資料。請求會被送到以下端點：

```
GET /edit_profile.php?name=peter
```

這會在伺服器端觸發以下請求：

```
GET /api/private/users/peter
```

攻擊者有可能透過操縱伺服器端的 URL 路徑參數來利用這個 API，為了測試這個漏洞，可以嘗試加入路徑穿越字串（path traversal sequences）來修改參數，並觀察應用程式的反應。

就例如可以提交這樣的值作為 name 參數：

```
peter/../admin
```
進行 URL 編碼後為：

```
GET /edit_profile.php?name=peter%2f..%2fadmin
```

這可能導致伺服器端的請求變成：

```
GET /api/private/users/peter/../admin
```

如果伺服器端的客戶端或後端 API 會對路徑進行標準化處理（normalize），
那麼該路徑可能會被解析成：

```
/api/private/users/admin
```

## Testing for server-side parameter pollution in structured data formats

攻擊者可能會藉由操縱參數，來利用伺服器在處理其他結構化資料格式（如 JSON 或 XML）時的漏洞，為了測試這類問題，可以嘗試在使用者輸入中注入意料之外的結構化資料，並觀察伺服器的反應。

舉例來說，某個應用程式允許使用者編輯個人資料，並透過請求送出修改內容給伺服器端的 API，瀏覽器會發出以下請求：

```
POST /myaccount  
name=peter
```

這會在伺服器端觸發以下請求：

```
PATCH /users/7312/update  
{"name":"peter"}
```

你可以試著加入一個 access_level 參數，如下所示：

```
POST /myaccount  
name=peter","access_level":"administrator
```

如果伺服器在將使用者輸入嵌入 JSON 時沒有適當的驗證或清理（validation / sanitization），那麼會導致以下伺服器端請求：

```
PATCH /users/7312/update  
{"name":"peter","access_level":"administrator"}
```

這可能導致帳號 peter 被賦予管理員權限，考慮一個類似的例子，但這次是使用者在客戶端輸入資料時是透過 JSON 格式傳送的，納在編輯你的名字時，瀏覽器會發出以下請求：

```
POST /myaccount  
{"name": "peter"}
```

這會導致伺服器端收到如下請求：

```
PATCH /users/7312/update  
{"name":"peter"}
```

你可以嘗試將 access_level 參數加入請求中，如下所示：

```
POST /myaccount  
{"name": "peter\",\"access_level\":\"administrator"}
```

如果伺服器在處理這些資料時，先解碼後再插入到 JSON 裡，而且沒有進行妥善的編碼（encoding）或驗證，就會產生以下的伺服器端請求：

```
PATCH /users/7312/update  
{"name":"peter","access_level":"administrator"}
```

結果就是：使用者 peter 可能就會被升級為管理員權限 🤯

結構化格式注入（Structured Format Injection） 不只會出現在「請求中」，也可能出現在伺服器的回應中。例如：如果使用者輸入的內容雖然安全地儲存在資料庫裡，但在回傳時被嵌入 JSON 回應而沒有經過適當的編碼處理，
也可能造成一樣的注入問題。

:::info
以上例子雖然是以 JSON 為例，
但其實伺服器端參數汙染（SSPP）可以發生在任何結構化資料格式中。
:::

## Preventing server-side parameter pollution


為了防止伺服器端參數污染，應採取以下措施：

- 使用允許清單（allowlist）：明確定義哪些字元是不需要編碼的，其他所有使用者輸入都應在納入伺服器端請求前進行適當的編碼處理。
- 強制格式驗證：確保所有輸入都符合預期的格式與結構，避免不正常或刻意構造的參數混入請求中。

---
