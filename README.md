# MD 簡報產生器

一個不需要建置工具的單頁 Markdown 簡報產生器。將 Markdown 內容轉換成 Reveal.js 簡報，可在瀏覽器中編輯、選擇版型、設定背景圖片、預覽並匯出成獨立 HTML。

## 線上使用

- GitHub Pages：<https://edreamer.github.io/mdppt/>
- GitHub Repository：<https://github.com/edreamer/mdppt>
- 目前版本：`v2.1.0`

## 功能

- 以 Markdown 快速建立投影片。
- 使用 `---` 分隔水平投影片。
- 使用 `###` 建立同一章節內的垂直子投影片。
- 提供 10 種配色版型與 6 種翻頁特效。
- 在版型設定中輸入圖片網址作為簡報背景。
- 背景圖片、配色版型與翻頁特效會保存於瀏覽器的 `localStorage`。
- 自動處理內容過長的投影片，必要時拆成延續頁面。
- 支援匯入 `.md`／`.markdown` 檔案。
- 匯出目前內容為可獨立開啟的 `mdppt.html`。
- 匯出版本會保留目前的主題、翻頁特效與背景圖片，但移除編輯工具列，只保留全螢幕按鈕。
- 支援響應式版面與瀏覽器全螢幕播放。

## 快速開始

### 直接開啟

開啟 [線上版本](https://edreamer.github.io/mdppt/) 即可使用，不需要安裝 Node.js 或其他建置工具。

### 本機預覽

由於第三方套件透過 CDN 載入，建議使用本機 HTTP 伺服器預覽：

```powershell
cd C:\Users\David\Documents\netlify\mdppt
python -m http.server 8091 --bind 127.0.0.1
```

接著開啟 <http://127.0.0.1:8091/>。

也可以使用其他靜態檔案伺服器；本專案沒有建置指令。

## Markdown 語法

最小範例：

````markdown
# 第一張投影片

這是第一張投影片的內容。

---

## 第二張投影片

- 重點一
- 重點二

---

## 第三張投影片

### 子投影片 A

這是垂直子投影片。

### 子投影片 B

按向下方向鍵查看下一頁。
````

支援標題、段落、清單、引用、表格、程式碼區塊與其他由 marked 解析的 Markdown 語法。

## 版型設定

開啟編輯器後，可以在右側設定：

1. 選擇配色版型。
2. 選擇翻頁特效。
3. 輸入圖片網址，作為簡報背景。
4. 按下「產生簡報」套用內容。

背景圖片欄位接受 `http:`、`https:` 或 `data:` 圖片網址。清空欄位即可恢復版型原本的背景。圖片會以滿版、置中、不重複方式顯示。

瀏覽器保存的設定鍵如下：

| 設定 | `localStorage` key |
| --- | --- |
| 配色版型 | `mdppt-theme` |
| 翻頁特效 | `mdppt-transition` |
| 背景圖片網址 | `mdppt-background-image` |

Markdown 編輯內容本身不會保存；重新整理後會載入內建範例內容。

## 匯出 HTML

點擊工具列的下載按鈕後，會產生 `mdppt.html`。匯出檔案包含：

- 目前編輯器中的 Markdown。
- 目前選用的配色版型。
- 目前選用的翻頁特效。
- 目前的背景圖片設定。
- Reveal.js 與 marked.js 的 CDN 載入設定。

匯出檔仍需要網路連線才能載入 CDN；若要支援完全離線使用，需要把第三方套件下載到專案內並改用相對路徑。

## 技術架構

- HTML、CSS、JavaScript：全部集中在 `index.html`。
- [Reveal.js 4.5.0](https://revealjs.com/)：簡報播放與投影片導覽。
- [marked 4.3.0](https://marked.js.org/)：Markdown 解析。
- [Font Awesome 6.4.0](https://fontawesome.com/)：工具列圖示。
- GitHub Pages：提供公開靜態網站。

本專案沒有 `package.json`、建置流程或必要的執行期伺服器。

## 專案結構

```text
mdppt/
├── index.html      # 主要應用程式與 GitHub Pages 入口
├── README.md       # 使用說明與維護文件
├── CHANGELOG.md    # 版本變更記錄
└── VERSION         # 目前版本號
```

## 版本管理

本專案使用 [Semantic Versioning](https://semver.org/)：`MAJOR.MINOR.PATCH`。

- `MAJOR`：不相容的操作方式或資料格式變更。
- `MINOR`：向後相容的新功能。
- `PATCH`：向後相容的錯誤修正或文件更新。

目前版本號以 `VERSION` 為準，並同步更新 README 與 `CHANGELOG.md`：

1. 完成修改並執行語法與瀏覽器檢查。
2. 更新 `VERSION`。
3. 在 `CHANGELOG.md` 新增版本區段與日期。
4. 同步更新 README 的目前版本。
5. 提交至 `main`，由 GitHub Pages 發布。
6. 以公開網址確認首頁與重要功能後，再將版本視為已發布。

建議提交訊息格式：

```text
feat: add background image setting
fix: correct slide export state
docs: update README
release: v2.1.0
```

## 目前版本變更

請參閱 [CHANGELOG.md](CHANGELOG.md)。

## 注意事項

- 第三方套件由 CDN 提供；CDN 無法連線時，Markdown 解析與 Reveal.js 功能可能無法使用。
- Markdown 內容會交給 marked 解析；若要讓不受信任的使用者使用，正式環境應再加入 HTML sanitization。
- 背景圖片網址必須允許瀏覽器載入；若圖片主機拒絕載入或網址失效，畫面會回到可見的版型背景。
- 本工具只適合公開或可信任的 Markdown 內容，不應直接拿來渲染未經檢查的第三方輸入。

## 授權

本 repository 目前未附加正式授權條款。如需讓他人重製、修改或再發布，請先補上適用的 LICENSE 檔案與授權內容。
