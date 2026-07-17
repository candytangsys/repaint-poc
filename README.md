# Repaint(暫定名)— 可編輯現實系統

一個在消費級手機瀏覽器上運行的即時場景理解系統,能對真實環境重新上色、並在實體物件上錨定互動介面(規劃中),長期願景是 AI 眼鏡時代的可編輯現實(Mediated Reality)。

本 repo 是研究計畫的技術原型(PoC),完整規格見《Repaint 規格書 v0.2》。

## 線上 Demo

部署於 GitHub Pages,手機瀏覽器（需允許相機權限）直接開啟:

- **主要成果**:[`m4-continuous.html`](m4-continuous.html) — 點擊分割 + 即時上色 + 連續模式
- **最初概念驗證**:[`index.html`](index.html) — AR.js 標記式 Demo(旋轉方塊)

> 第一次載入 `m4-continuous.html` 會下載分割模型,約需十餘秒,請耐心等待「✅ 就緒」提示。

## 檔名對照表(重要:避免與規格書的 Phase 混淆)

repo 裡的 `m1-camera.html`〜`m4-continuous.html` 是規格書「**Phase 1**」*內部*的開發里程碑,**不是**規格書的 Phase 0/1/2/3(這四個檔案原本命名為 `phase1.html`〜`phase4.html`,容易被誤認成規格書的 Phase 1〜4,已改為里程碑命名)。對照關係如下:

| repo 檔案 | 里程碑 | 對應規格書階段 | 內容 | 狀態 |
|---|---|---|---|---|
| `index.html` | — | Phase 0 | AR.js + A-Frame 標記式 Web AR | ✅ 完成 |
| `m1-camera.html` | M1 | Phase 1 | 取得相機串流,畫面全螢幕鋪滿 | ✅ 完成 |
| `m2-segment.html` | M2 | Phase 1 | 加入 MediaPipe Interactive Segmenter,點擊分割並畫出遮罩 | ✅ 完成(座標換算為簡化版,M3 已修正) |
| `m3-color.html` | M3 | Phase 1 | 加入 Multiply 混色上色、四色選單、修正 `object-fit: cover` 的座標對位 | ✅ 完成 |
| `m4-continuous.html` | M4 | Phase 1(含部分 Phase 2 元素) | 整合以上所有功能 + 連續模式(每 300ms 重新分割)+ 效能 HUD | ✅ 完成,為目前主要展示版本 |
| — | — | Phase 2 | 三指定物件(牆/斑馬線/桌面)精細分割、效能優化 | ⏳ 規劃中 |
| — | — | Phase 3 | WebXR 空間錨定 + 物件旁互動介面卡片 | ⏳ 規劃中 |

簡言之:**`m4-continuous.html` 已完整實現規格書定義的 Phase 1**(點擊即分割 + Multiply 上色 + 誠實延遲記錄),`m1-camera.html`〜`m3-color.html` 是留存的開發過程紀錄。規格書的 Phase 2、3(多物件、效能優化、物件錨定 UI)尚未開始。

## 技術棧

- 前端:純 HTML/CSS/JS(無框架),`<script type="module">` + ES import
- 相機:`getUserMedia`(`facingMode: 'environment'`)
- 分割:[MediaPipe Tasks Vision](https://developers.google.com/mediapipe/solutions/vision/interactive_segmenter) — `InteractiveSegmenter`(`magic_touch` 模型,瀏覽器端 WASM 執行,無需伺服器)
- 上色:Canvas 2D `getImageData`/`putImageData` 手動 Multiply 混色,保留原始明暗光影
- 部署:GitHub Pages(HTTPS,`getUserMedia` 硬性需求)
- 開發/測試環境:Windows + iPhone Safari

## 已知限制(如實記載)

- 分割延遲穩定在 130–135ms(iPhone + Safari + Wi-Fi,零優化),未達規格書 ≦100ms 目標,差距約 30%。
- 連續模式(`setInterval` 300ms)延遲微降但追蹤穩定性下降、機身微溫,詳見延遲量測紀錄表。
- 貼合度依物件特性而定:大而輪廓分明者最佳,小型/反光/背景雜亂者失效。
- MediaPipe 遮罩前景/背景值的極性為實測校準結果(見 `m4-continuous.html` 內註解),更換模型版本時需重新驗證。

## 引用與參考

技術基線參考 [XR-Objects](https://github.com/google/xr-objects)(UIST 2024)與 [XR Blocks](https://github.com/google/xrblocks)。
