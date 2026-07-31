# 向上訓練

個人化的訓練與飲食計畫網頁 App，資料與畫面分離，改課表只需要編輯 JSON，不用碰 HTML。

🔗 **線上版本**：[training-plan-orpin.vercel.app](https://training-plan-orpin.vercel.app)

## 功能

- **六個頁籤**：總覽（週期目標與里程碑）／訓練（互動式動作庫，點選加入當日菜單並即時算肌群與熱量）／訓練模板（每週固定課表）／爆發力（垂直跳目標、跳躍菜單、助跑摸高技巧）／飲食（熱量目標、三餐架構、球後補給）／注意（恢復、睡眠、deload 提醒）
- **中英雙語切換**：右上角按鈕即時切換，選擇會存在 `localStorage`，下次開啟記住上次語言
- **手機優先設計**：底部 tab bar 導覽、支援加入 iOS 主畫面（apple-touch-icon、favicon 已內建）

## 檔案結構

```
.
├── index.html          # 版面與互動邏輯（純 HTML/CSS/JS，無 build step）
├── plan.json            # 中文版所有資料（目標、課表、飲食、恢復等）
├── plan.en.json         # 英文版資料，結構與 plan.json 一一對應
├── apple-touch-icon.png # iOS 加入主畫面圖示
└── favicon.ico
```

## 更新課表 / 資料

不需要修改 `index.html`，直接編輯對應的 JSON 即可，畫面會自動依照資料重新渲染：

- 改中文內容 → 編輯 `plan.json`
- 改英文內容 → 編輯 `plan.en.json`（務必維持跟 `plan.json` 相同的欄位結構，只翻譯文字內容）

常用調整位置：

| 想改什麼 | 對應欄位 |
|---|---|
| 每月里程碑 | `profile.milestones` |
| 總覽每 15 日體重與體脂目標／實際紀錄 | `profile.progressHistory.records`、`profile.progressHistory.bodyFatRecords` |
| 每週固定課表 | `trainingTemplates.days` |
| 訓練頁可點選的動作庫 | `training.exerciseGroups` |
| 垂直跳目標與測驗紀錄 | `jumpTraining.target`、`jumpTraining.testHistory` |
| 飲食熱量與補給 | `diet` |
| 恢復／睡眠提醒 | `recovery.sections` |

改完存檔、`git push` 上去，Vercel 會自動重新部署。

## 本機預覽

這個專案會用 `fetch()` 讀取 `plan.json` / `plan.en.json`，直接雙擊開啟 `index.html`（`file://` 協定）會因為瀏覽器安全限制讀不到資料，需要透過本機伺服器開啟，例如：

```bash
npx serve .
# 或
python3 -m http.server 8000
```

再用瀏覽器打開對應的 `localhost` 網址即可。

## 技術

純前端 Vanilla HTML / CSS / JS，沒有框架、沒有 build 流程，部署在 [Vercel](https://vercel.com)。
