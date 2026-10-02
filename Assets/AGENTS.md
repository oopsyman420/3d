# AI Agent 共用協作規範

## 開始前

- 回覆、計畫、提交說明與教學文件均使用繁體中文；類別、變數與路徑用清楚英文命名。
- 先讀 README.md、docs/02_初始化與InputSystem.md、docs/03_AI協作流程.md、docs/PROJECT_STATUS.md 與本次任務。
- 固定 Unity Editor 6000.3.17f1。不得自行升級 Editor、套件或切換渲染管線。
- 檢查實際 Packages/manifest.json、packages-lock.json 與 ProjectSettings，不能把骨架視為已初始化專案。

## 輸入系統：強制規則

- 只使用 com.unity.inputsystem 與 UnityEngine.InputSystem。
- Active Input Handling 必須設為 Input System Package (New)，不可設成 Both 或 Input Manager (Old)。
- 禁止新增 UnityEngine.Input、Input.GetAxis、Input.GetAxisRaw、Input.GetButton、Input.GetKey、Input.GetMouseButton 等舊 API；禁止使用舊 StandaloneInputModule。
- 遊戲操作以 Input Actions 定義，集中於 Assets/_Project/Input；使用 InputActionReference 或 PlayerInput 接入遊戲邏輯。直接查詢 Keyboard.current/Mouse.current 只限有明確需求的工具或除錯操作，須處理裝置不存在的情況。
- 使用 uGUI 時，EventSystem 使用 InputSystemUIInputModule，並驗證 UI Actions 指派正確。UI Toolkit 則依該版本官方支援方式設定，不要強行套用 uGUI 元件。
- Action 的 Enable/Disable 與事件訂閱/取消必須成對；停用或銷毀後不得重複收到輸入事件。
- 不假定套件最新版適合本專案。使用此 Editor 的 Package Manager 所提供的相容版本，將實際版本提交到 manifest 與 lock 檔並記錄。

## 資產與實作

- 自有資產放 Assets/_Project；第三方資產放 Assets/ThirdParty，避免修改第三方內容。
- 資產移動、重新命名與刪除，優先透過 Unity Editor；保留既有 .meta 與 GUID，不可重建來修復參照。
- 不手寫不存在的 Scene、Prefab 或 .meta 來假裝完成 Editor 工作。必要時提供 Editor 工具或明確操作步驟。
- 新增公開或 SerializeField 欄位，須交代 Inspector 指派、預設值與操作方式。
- 優先簡單 MonoBehaviour 與小型 C# 類別；沒有需求不增加框架、單例群、服務容器或額外套件。
- 本範本尚未建立 asmdef；初期可用預設組件。開始建立 Unity Test Framework 測試時，按測試與執行程式依賴建立 asmdef，避免測試引用不到程式碼。
- 不提交 Library、Temp、Obj、Logs、UserSettings、Builds、憑證、帳密或付費素材。

## 驗證與交接

- 流程：規劃 → 實作 → 編譯 → 執行 → 測試 → 審查；失敗修復後重測。
- 不能操作 Unity 時，清楚標記「待 Editor 驗證」，不可宣稱 Play Mode 或 Build 通過。
- 修改前檢查現有變更，不覆蓋使用者工作。每次只做一個可驗證任務。
- 完成後列出修改檔案、目的、Inspector 設定、驗證結果與未完成事項；更新 docs/PROJECT_STATUS.md。
- 需要與外部工具串接時，另設該工具的薄層設定，引用本檔，避免複製規則造成不同版本。AGENTS.md 是共用約定，不保證每個 Agent 自動讀取。
