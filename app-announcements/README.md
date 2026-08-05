# app-announcements

公告走 **GitHub raw**（非 jsdelivr）：檔案小、快取僅約 5 分鐘、永遠指向最新 commit，**免 purge**。

```
https://raw.githubusercontent.com/Aaron-Tsai-iosDeveloper/NamoAmitabha/main/app-announcements/manifest.json
https://raw.githubusercontent.com/Aaron-Tsai-iosDeveloper/NamoAmitabha/main/app-announcements/<id>/<lang>.md
```

App 內也內建了同一份（`amitabha/assets/announcements/`）作為離線／首次啟動的預設；
偵測到遠端 `version` 較新時就會抓下新版覆蓋本地快取。

> 推上 GitHub 後約 5 分鐘內生效，不需要清任何快取。
> （念佛背景是大檔，仍走 jsdelivr CDN，與公告不同。）

## 結構

```
app-announcements/
  manifest.json          # 公告清單（中繼資料 + 各語系標題）
  <id>/
    zh.md                # 各語系內文（Markdown）
    en.md
    ...
```

## manifest.json 欄位

```jsonc
[
  {
    "id": "dev-intro",        // 唯一 ID，同時是內文資料夾名
    "version": 1,             // 版本號；改內容時 +1，App 才會重抓並亮紅點
    "date": "2026-08-01",     // 顯示日期 yyyy-MM-dd
    "pinned": true,           // true = 列表頂端直接展開全文；一般公告省略或設 false
    "title": {                // 各語系標題（短，直接放這裡）
      "zh": "開發初衷",
      "en": "A Note from the Developer"
    },
    "langs": ["zh"]           // 有提供內文檔的語系；決定 fallback 落點（當前→en→zh）
  }
]
```

## 發佈新公告的步驟

1. 在 `manifest.json` **最前面**新增一筆（新的公告排在上面）。
2. 建一個 `<id>/` 資料夾，放各語系的 `.md`（至少放 `zh.md`），並在 `langs` 列出。
3. 想置頂展開就設 `"pinned": true`；一般更新公告不設，會以「標題＋日期」列出、點進看內文。
4. commit / push 到 GitHub（raw 約 5 分鐘內生效，免 purge）。

## 更新既有公告

改內容後，記得把該筆的 `version` **加 1**，App 才會重新抓取並在設定頁亮紅點。

## 補翻譯

之後要補其他語系，只要在該 `<id>/` 資料夾加上 `en.md`、`ja.md` 等，
並把語系碼加進 `langs` 即可。缺的語系會自動 fallback：當前語系 → en → zh。
