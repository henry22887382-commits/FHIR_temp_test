# FHIR R4 體溫輸入與調閱

繁體中文單頁操作介面，依 FHIR R4 建立、讀回及查詢體溫 Observation。僅使用虛構測試資料。體溫代碼固定為 LOINC `8310-5`，單位固定為 UCUM `Cel`。

## 六步操作

1. **連線與授權設定**：手動填入講師提供的 FHIR R4 base URL、虛構測試病人 ID 與授權方式。可先按「檢查伺服器 /metadata」查看能力宣告。
2. **輸入量測資料**：填入含時區的量測時間及體溫數值。
3. **預覽並上傳**：檢查產生的 JSON 和 POST 目標，勾選確認後才送出。上傳成功後頁面會嘗試依 ID 讀回。
4. **讀回並核對**：檢查 Observation ID、病人、量測時間、體溫值與單位；也可手動輸入 ID 再讀取。
5. **依病人查詢**：搜尋指定病人的體溫 Observation；若結果有 `next` 分頁，按「載入下一頁」繼續。
6. **與同學互讀**：填入同學提供的虛構 Patient ID 和 Observation ID，或查詢該病人的體溫紀錄，並核對回傳病人。

## 本機操作

需要已安裝 Python 3。將 `index.html`、`server.py` 與 `啟動網頁.bat` 放在同一資料夾：

- Windows 可雙擊 `啟動網頁.bat`，再於瀏覽器開啟 <http://127.0.0.1:8000/>。
- 也可在此資料夾執行 `py -3 server.py`，或 `python server.py`；指定埠號可用 `py -3 server.py 8080`。
- 停止伺服器時，在命令視窗按 Ctrl+C。

`server.py` 只在本機提供靜態頁面，不會代理 FHIR API。瀏覽器會直接連到手動填寫的 FHIR base URL。

## GitHub Pages

GitHub Pages 只需發布 `index.html`，不需要安裝或執行 Python，也不會執行 `server.py`。此 repo 的預期頁面網址為 <https://henry22887382-commits.github.io/FHIR_temp_test/>。在 GitHub repo 的 **Settings → Pages** 設定從發布分支的根目錄部署後，即可用該網址開啟。

FHIR base URL、測試病人 ID、量測時間、體溫與授權方式都在頁面中手動填寫。Bearer token 僅留在目前頁面的記憶體，不寫入 HTML、網址或瀏覽器儲存空間；重新載入頁面後需重新輸入。不要將長期秘密 token 放進程式或提交至 repo。

從 GitHub Pages 連接遠端 FHIR 服務時，FHIR 伺服器必須使用 HTTPS，且 CORS 必須允許來源 **`https://henry22887382-commits.github.io`**（來源不含 repo 的 `/FHIR_temp_test/` 路徑）。伺服器也需允許頁面使用的請求方法與標頭；若回應以 `Location` 提供新 Observation ID，需透過 CORS 公開 `Location` 標頭。實際 CORS 設定請由伺服器管理者依其環境完成。

`/metadata` 只呈現伺服器自行宣告的能力，不會驗證實際 API 存取權、CORS 是否可用，或目前帳號是否有建立、讀取及跨病人查詢權限。

## 驗證範圍

尚未連接講師提供的 FHIR 伺服器實測。能否上傳、讀回、搜尋及互讀，取決於實際 base URL、授權、伺服器權限與 CORS 設定。請只使用虛構病人及量測資料。

