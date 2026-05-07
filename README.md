# 圖像說明框編輯器 ｜ Annotation Editor

為圖片加上可拖曳的說明框，輸出 HTML / SVG / PNG / JPG / JSON 五種格式。單檔自包含，無外部相依（除了 Google Fonts），可直接放上 GitHub Pages。

---

## 功能

**框的編輯**
- 拖曳移動、八向縮放手把
- 每個框獨立設定：底色 + 透明度、邊框（顏色 / 粗細 / 圓角）、文字色、字級
- 內容結構可選：預設單欄內文，可勾選加上粗體標題列
- 引線可選：每個框獨立決定要不要從框邊指向圖上某點，端點可拖曳，可開關箭頭

**底圖來源（兩種都支援）**
- **上傳檔案**：圖片以 base64 內嵌進輸出檔，完全離線可用
- **貼網址**：HTML / SVG 輸出參照原網址（檔案小，但需網路）。PNG / JPG 輸出需要圖片來源允許 CORS

**輸出格式**

| 格式 | 用途 | 特性 |
|---|---|---|
| `HTML` | 貼進 CMS 的代碼片段 | 受限標籤白名單；inline style；無 SVG |
| `SVG` | 向量檔，可任意縮放 | foreignObject 保留 CJK 換行 |
| `PNG` / `JPG` | 點陣圖 | canvas 渲染，含 CJK 折行 |
| `JSON` | 設定檔，可日後匯入再編 | |

**操作**
- 點選框 → 屬性面板出現
- `Del` / `Backspace` 刪除 · `Esc` 取消選取 · `⌘ / Ctrl + D` 複製
- 自動存進 localStorage，重整不掉資料
- 開瀏覽器 console 可用 `__editor` 物件檢查內部狀態

---

## HTML 代碼輸出規格

點輸出 → HTML 會跳出預覽視窗，可複製到剪貼簿或下載成檔案。輸出的代碼遵循以下限制（適合貼進有標籤白名單的 CMS）：

**標籤白名單**：`<div>` `<span>` `<p>` `<hr>` `<b>` `<i>` `<img>` `<br>`

**樣式**：全部 inline style；不產生 `<style>` 標籤、不引用外部 CSS。

**單位**
- `font-size` / `padding` / `margin` / `text-shadow` 一律 `rem`
- 其餘（`width` / `height` / `left` / `top` / `border-width` / `border-radius` / `transform`）使用 `px` 或 `%`

**結構**

```
<div>                      ← 容器（relative + inline-block）
  <img ...>                ← 底圖（width:100% height:auto，會跟容器縮放）
  <div ...></div>          ← 引線（旋轉的細長 div）
  <div ...></div>          ← 箭頭（CSS triangle）
  <div>                    ← 說明框
    <p><b>標題</b></p>     ← 可選
    <p>內文<br>第二行</p>
  </div>
  ...更多說明框
</div>
```

**為什麼不用 `<svg>` 畫線**：SVG 不在白名單裡。改用 `transform: rotate()` 旋轉的 div 畫線、用 border trick 做箭頭，純 CSS 實作。引線長度用 `%` 表示，所以容器縮放時引線也會等比縮放。

---

## 部署到 GitHub Pages

```bash
# 1. 建一個新 repo，把 index.html 放到 root
git init
git add index.html README.md
git commit -m "init annotation editor"
git remote add origin https://github.com/<你的帳號>/<repo-name>.git
git push -u origin main

# 2. 到 repo Settings → Pages
#    Source: Deploy from a branch
#    Branch: main / (root)
#    儲存後等一兩分鐘
```

完成後網址是 `https://gentuoman.github.io/Annotation_Editor/`

> 也可以丟到任何靜態主機（Netlify / Cloudflare Pages / 自己的伺服器），單純把 `index.html` 放到 web root 即可。

---

## 注意事項

- **localStorage 容量**：上傳很大的圖（>5MB base64）可能超過瀏覽器配額而存檔失敗，但編輯期間不影響。
- **CORS 與點陣圖輸出**：`PNG` / `JPG` 會在 canvas 重繪一次，跨網域圖片若伺服器沒給 `Access-Control-Allow-Origin` header，輸出會失敗（會跳訊息）。如果是上傳的本地圖則無此問題。
- **HTML 代碼貼進 CMS**：產生的 snippet 每個主要元素一行，多數 CMS 編輯器在「原始碼」模式下可正常處理。若 CMS 用 wysiwyg 自動排版，建議切到原始碼／HTML 模式貼上。
- **字型**：UI 用 Google Fonts (Newsreader + IBM Plex Sans/Mono + Noto Sans/Serif TC)，第一次載入需要連網，之後瀏覽器會快取。

---

## 結構

```
annotation-editor/
├── index.html    ← 全部都在這一個檔案（HTML + CSS + JS）
└── README.md
```
