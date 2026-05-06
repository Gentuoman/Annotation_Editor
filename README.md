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

**輸出**
- `HTML`：自包含網頁，含原圖 + 框 + 引線（用 SVG 畫線確保箭頭精準）
- `SVG`：向量檔，文字以 `foreignObject` 保留 CJK 換行
- `PNG` / `JPG`：以 canvas 渲染，含手動 CJK 折行
- `JSON`：完整設定檔，可日後匯入再編輯

**操作**
- 點選框 → 屬性面板出現
- `Del` / `Backspace` 刪除 · `Esc` 取消選取 · `⌘ / Ctrl + D` 複製
- 自動存進 localStorage，重整不掉資料
- 開瀏覽器 console 可用 `__editor` 物件檢查內部狀態

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

完成後網址是 `https://<你的帳號>.github.io/<repo-name>/`。

> 也可以丟到任何靜態主機（Netlify / Cloudflare Pages / 自己的伺服器），單純把 `index.html` 放到 web root 即可。

---

## 注意事項

- **localStorage 容量**：上傳很大的圖（>5MB base64）可能超過瀏覽器配額而存檔失敗，但編輯期間不影響。
- **CORS 與點陣圖輸出**：`PNG` / `JPG` 會在 canvas 重繪一次，跨網域圖片若伺服器沒給 `Access-Control-Allow-Origin` header，輸出會失敗（會跳訊息）。如果是上傳的本地圖則無此問題。
- **字型**：UI 用 Google Fonts (Newsreader + IBM Plex Sans/Mono + Noto Sans/Serif TC)，第一次載入需要連網，之後瀏覽器會快取。

---

## 結構

```
annotation-editor/
├── index.html    ← 全部都在這一個檔案（HTML + CSS + JS）
└── README.md
```

沒有 build step，沒有依賴管理，沒有後端。改 CSS / 調預設值直接編輯 `index.html` 即可。
