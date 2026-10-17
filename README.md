# FHIR R4 體溫輸入與調閱（GitHub Pages）

本資料夾是可直接部署到 GitHub Pages 的純靜態版本，只需發布 `index.html`；不需要 Python、啟動腳本、套件、建置流程或後端。原本的本機 Python 版本保留在另一資料夾，不影響這個 Pages 版本。

預期網址：<https://henry22887382-commits.github.io/FHIR_temp_test/>

## 六步操作

1. 填入講師提供的 FHIR R4 base URL、虛構測試病人 ID、授權方式；可先檢查 `/metadata` 能力宣告。
2. 填量測時間（含時區）與體溫數值；固定使用 LOINC `8310-5`、UCUM `Cel`。
3. 預覽 Observation JSON，核對目標後勾選確認才會 POST；成功後會嘗試依 ID 自動讀回。
4. 依 Observation ID 讀回，核對病人、時間、數值與單位。
5. 依病人與體溫代碼查詢，使用伺服器提供的 `next` 載入後續頁面。
6. 在講師授權的範圍內，輸入同學的虛構 Patient ID／Observation ID 互讀並核對病人。

## FHIR 伺服器設定

頁面所有 API 請求都由瀏覽器直接送往使用者手動填入的 base URL；本頁不提供代理或後端。GitHub Pages 是 HTTPS，因此 FHIR base URL 也必須使用 HTTPS，否則瀏覽器會封鎖混合內容請求。

FHIR 伺服器管理者需允許 CORS 來源 **`https://henry22887382-commits.github.io`**。來源只包含 hostname，不包含 `/FHIR_temp_test/` 路徑。伺服器還需允許本頁使用的 GET、POST 與 `Accept`、`Content-Type`、`Authorization`、`Prefer` 標頭；若 POST 以 `Location` 回傳 ID，需公開該回應標頭。實際設定由伺服器管理者依服務環境完成。

授權方式需依講師指示選擇。短效 Bearer token 僅保留於目前頁面記憶體，重新載入後需重填；不要將長期秘密 token 寫進 HTML 或提交到 repo。互讀是否成功也取決於伺服器對該帳號的讀取權限。

`/metadata` 顯示的是伺服器自我宣告能力，不等同於實際 API、CORS 或帳號權限測試。請只使用虛構病人與虛構量測資料。尚未連接講師的伺服器做端到端實測。

