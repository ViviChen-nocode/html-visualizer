# 配色 + 字體 token（Anthropic / Claude 官方品牌風）

每個 HTML 起手式都複製這段 CSS、不要自創顏色 / 字體。風格參考 Anthropic 官方品牌（ivory + clay + serif/sans/mono 三字體）— editorial / book / magazine 質感。

> 風格樣張見 `examples/anthropic-gallery/index.html`（原創 markup，示範這套 token 排出來的質感）。

## 完整 design tokens

放在 `<style>` 區的 `:root {}` 內。**包含 Anthropic palette + 舊變數 alias**（讓 `component-library.md` 內舊 snippet 仍能直接用）：

```css
:root {
  /* ── 暖色底 / 深近黑 ──────────── */
  --ivory:  #FAF9F5;   /* 頁面底色（不用純白）*/
  --paper:  #FFFFFF;   /* 卡片底色 */
  --slate:  #141413;   /* 主要文字 / 標題 */

  /* ── Anthropic 招牌色 ──────────── */
  --clay:   #D97757;   /* 主 accent — 連結 / 強調（換字重）/ hover */
  --clay-d: #B85C3E;   /* 深 clay — 主要 CTA */
  --oat:    #E3DACC;   /* 燕麥 — hover bg / 裝飾 */
  --olive:  #788C5D;   /* 橄欖綠 — 次強調 / 成功 / 採納 */

  /* ── Warm gray scale ─────────── */
  --g100:   #F0EEE6;   /* 最淺 — section bg */
  --g200:   #E6E3DA;   /* 卡片次層 */
  --g300:   #D1CFC5;   /* 邊線色（最常用）*/
  --g500:   #87867F;   /* 次要文字 / mono 標籤 */
  --g700:   #3D3D3A;   /* 內文淺色 */

  /* ── 字體三套（中英分工，見下方說明）──────────────────── */
  --serif: Georgia, "Times New Roman", Times, "Noto Sans TC", serif;
  --sans:  Calibri, Carlito, "Noto Sans TC", system-ui, -apple-system, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
  --mono:  ui-monospace, "SF Mono", Menlo, Monaco, Consolas, monospace;

  /* ── 兼容舊變數名（讓通用元件 snippet 不需修改）─────── */
  --bg:           var(--ivory);
  --bg-card:      var(--paper);
  --bg-soft:      var(--g100);
  --bg-soft-2:    var(--g200);
  --border:       var(--g300);
  --border-strong:var(--g500);
  --text:         var(--slate);
  --text-muted:   var(--g700);
  --text-soft:    var(--g500);
  --accent:       var(--clay);
  --accent-soft:  #FBE9DF;
  --accent-strong:var(--clay-d);
}
```

## 字型分工（中英）⭐

Vivi 偏好的中英分工，已內建在上方 token：

| 角色 | 英文 | 中文 |
|---|---|---|
| 標題（`--serif`）| Georgia serif（editorial 質感）| 思源黑體 Noto Sans TC（中文無對應免費 serif，黑體標題更俐落）|
| 內文（`--sans`）| Calibri → Carlito（開源替身）→ system | 思源黑體 Noto Sans TC |

運作原理：英文字體（Georgia / Calibri / Carlito）不含中文字符，中文會自動 fallback 到 stack 後面的 `"Noto Sans TC"`，達成中英分流。

**⭐ 必做：每份 HTML 的 `<head>` 一定要引入這兩個 web font**（否則 Calibri 在沒裝 Office 的 Mac／分享出去會掉漆、思源黑體也載不到）：

```html
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
<link href="https://fonts.googleapis.com/css2?family=Carlito:wght@400;700&family=Noto+Sans+TC:wght@400;500;700;900&display=swap" rel="stylesheet" />
```

- Calibri 是 Office 專有字體（macOS 非預設）→ 用 metric 相容的開源 **Carlito** 當 fallback，保證跨裝置一致。
- Noto Sans TC ＝ 思源黑體（Google Fonts 版）。

## 配色語意（Anthropic 風）

| 角色 | 顏色 | 用法 |
|---|---|---|
| 頁面底 | `--ivory` | body 背景、不用純白 |
| 卡片底 | `--paper` | 卡片 / 主要區塊 |
| 主要文字 | `--slate` | h1 h2 標題、主要文字 |
| 次要文字 | `--g700` | 內文、副說明 |
| 弱化文字 | `--g500` | metadata / mono label / 註腳 |
| 主 accent | `--clay` | h1 強調（換字重、不用斜體）、連結、section index、hover |
| Hover 區塊底 | `--oat` | card thumbnail hover、輕微 emphasis |
| 次 accent | `--olive` | 圖示分色、次強調、成功 |
| 邊線 | `--g300` | 1.5px 細邊（不是 1px、不是 2px）|
| 弱底色 | `--g100` | section 弱化背景 / 卡片次層 |
| 強調底 | `--g200` | thumbnail bg / deep card |

### 狀態 semantic（語意保留、低調用）

預設 Anthropic warm palette 不太使用 cool 綠 / 紅。但有 status 強對比需求時（例：成功 / 失敗 / 警告 chip）：

```css
:root {
  /* 對齊 warm palette、不刺眼 */
  --green:        #788C5D;  /* = olive */
  --green-soft:   #EFF1E8;
  --green-border: #C8D2B8;

  --red:          #B85C3E;  /* = clay-d，deep rust */
  --red-soft:     #FAEBE3;
  --red-border:   #E8C4AF;

  --yellow:       #C49845;
  --yellow-soft:  #F8EFD9;
  --yellow-border: #E8D4A0;

  /* 元件庫 snippet 會用到的延伸組（olive = green 別名、clay-soft = accent-soft 別名） */
  --olive-soft:   #EFF1E8;
  --olive-border: #C8D2B8;
  --clay-soft:    #FBE9DF;
  --orange:       #C9740A;
  --orange-soft:  #FBEEDC;
  --orange-border: #ECCFA3;
}
```

→ Status badge 用這套 token、保持 warm palette 一致性、不破壞 editorial 風格。

## 字體規則（最重要的差異）

### 標題用 serif，但強調不用斜體

```css
h1, h2, h3 {
  font-family: var(--serif);
  font-weight: 500;       /* 不是 700、Anthropic 風偏細 */
  letter-spacing: -0.012em;
  color: var(--slate);
}

h1 {
  font-size: clamp(38px, 5.4vw, 62px);  /* 響應式大標 */
  line-height: 1.06;
  letter-spacing: -0.018em;
}

h2 {
  font-size: 27px;
  letter-spacing: -0.012em;
}

h3 {
  font-size: 19px;
  letter-spacing: -0.008em;
}

/* B · 換字重（預設強調手法） */
h1 em, h2 em, h3 em {
  font-style: normal;
  font-weight: 700;
  color: var(--clay);
}

/* C · 螢光筆（語氣更重、一頁最多一兩次） */
.hl {
  background: linear-gradient(transparent 58%, #f7d9a0 58%);
  font-style: normal;
}

/* E · 保險絲（全域、擋掉所有合成字形） */
body { font-synthesis: none; }
```

```html
<h1>造工廠，還是寫<em>配方</em>？</h1>
<h1>能用，但大半<span class="hl">不該</span>從這裡拿</h1>
```

🔴 **中文沒有斜體**（2026-09-20 定案：B＋C＋E 三招並用）。漢字字形沒有義大利體傳統，
瀏覽器遇到 `font-style: italic` 只能機械傾斜漢字，筆畫變形。本規範原本寫「serif +
italic 強調」、範本也內建 `h1 em { font-style: italic }`——**實測當月 38／45 份產出的
主標題都在犯這個**，因為範本的示範文字就是這樣寫的。

**強調手段對照表**（中文替代西文的 italic；參考 huashu-design 的排印分冊）：

| 西文慣例 | 中文替代 | 怎麼寫 | 什麼時候用 |
|---|---|---|---|
| italic 強調 | **換字重**（預設） | `font-weight: 700` ＋ clay 色 | 多數情況。安靜、不打斷閱讀 |
| italic 書名／引用 | **螢光筆底色** | `.hl` 的線性漸層 | 語氣要重、想讓人停一下。一頁最多一兩次 |
| italic 專名 | **著重號** | `text-emphasis: dot; text-emphasis-position: under` | 中文原生手法、最雅緻，但小字級幾乎看不見 |
| —（防呆） | **保險絲** | `body { font-synthesis: none }` | 一律加。寫錯了也不會變形，自動退回正體 |

⚠️ **保險絲的副作用：內文的 `<em>` 會失去強調**（2026-09-21 實測）。瀏覽器預設讓 `em` 斜體，保險絲擋掉合成斜體後，中文 em 會跟正文**一模一樣**——不再歪，但也看不出來了。所以範本另加 `em { font-weight: 600 }`：中文靠字重被看見，英文仍用真的義大利體（字重加上去也不衝突）。只改標題 em 不改內文 em，等於把「歪但看得到」換成「正但看不到」。

⚠️ **混字體強調是外行**（taste-skill §4.1）：要強調標題裡的字，用**同一個字體**的
字重或斜體，不要在無襯線標題裡插一個襯線字（或反過來）。本規範的 B 就是同字體換字重。

> Serif 當預設字體在別的情境是有爭議的——taste-skill 把它列為「最常被測出來的 AI
> 破綻」。但它自己列的兩個例外（品牌明訂 serif、定位真的是 editorial）**本規範兩條都
> 符合**：serif 來自 Anthropic 官方品牌，產出定位就是 editorial。所以 serif 留著，
> 只拿掉斜體。

### 排印細節（白拿的品質）

```css
/* 段落尾行不要留孤字；標題不要斷得難看 */
p, li, td { text-wrap: pretty; }
h1, h2, h3 { text-wrap: balance; }
```

`text-wrap` 現代瀏覽器都支援，成本為零、效果直接（參考 huashu-design：「白拿的排印
品質」）。本庫原本 0 處使用。`pretty` 避免段落最後一行只剩一兩個字，`balance` 讓多行
標題的每一行長度接近。

### 內文用 system sans

```css
body {
  font-family: var(--sans);
  font-size: 16.5px;       /* 內文略大、editorial 感 */
  line-height: 1.55;
  color: var(--slate);
}

.intro {
  font-size: 16.5px;
  color: var(--g700);
  max-width: 620px;
}

.desc, .body-text {
  font-size: 13.5px;
  color: var(--g700);
  line-height: 1.5;
}
```

### Eyebrow / metadata / file path 用 mono uppercase

```css
.eyebrow {
  font-family: var(--mono);
  font-size: 12px;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--g500);
  display: flex;
  align-items: center;
  gap: 12px;
}

/* 前面加一條 clay 短線裝飾 */
.eyebrow::before {
  content: "";
  width: 24px;
  height: 1.5px;
  background: var(--clay);
}
```

```html
<div class="eyebrow">— Companion to the blog post</div>
```

→ Eyebrow 是 Anthropic 風的 signature 元素、給 section / hero 一個 mono uppercase 的 metadata header。

### Mono 用法

| 用法 | 範例 |
|---|---|
| Eyebrow / 副標 | `Companion to the blog post` |
| Section index 編號 | `01` / `02` (mono 13px clay 色) |
| File path / 檔名 | `01-exploration-code-approaches.html` |
| Pill count | `3 demos` (mono 11px g500) |
| Code inline | `<code>.html</code>` |

## 字體大小階層（Anthropic 風）

| 層級 | size | family | weight | 用途 |
|---|---|---|---|---|
| Hero | 38-62px clamp | serif | 500 | 主標題 |
| Section | 27px | serif | 500 | 區段標 |
| Card | 19px | serif | 500 | 卡片標 |
| Intro | 16.5px | sans | 400 | hero 副說明 |
| Body | 14.5-16px | sans | 400 | 內文 |
| Card desc | 13.5px | sans | 400 | 卡內次要文字 |
| Eyebrow | 12-13px | mono | 600 | metadata |
| Index | 13px | mono | 600 | section 編號 |
| Pill | 12.5px | sans | 400 | TOC pill |
| File path | 11-12px | mono | 400 | 檔名 / 註腳 |

## 邊線 / 圓角 / 陰影（Anthropic 風）

```css
/* 邊線：1.5px、不是 1px、不是 2px */
.card     { border: 1.5px solid var(--g300); border-radius: 14px; }
.thumb    { border-bottom: 1.5px solid var(--g200); }
.toc-pill { border: 1.5px solid var(--g300); border-radius: 999px; }
header.masthead { border-bottom: 1.5px solid var(--g300); }

/* Hover 邊框變深近黑 */
.card:hover { border-color: var(--slate); }

/* Hover 浮起 + 淡陰影（暖色調）*/
.card:hover {
  transform: translateY(-3px);
  box-shadow: 0 10px 30px rgba(20, 20, 19, 0.10);
}

/* hero 卡片陰影更深 */
.hero-fig .pane.html {
  box-shadow: 0 12px 32px rgba(20, 20, 19, 0.10);
}
```

## 間距系統

```
主容器寬度：min(94vw, 1760px)  ← 寬版、吃滿寬螢幕不浪費兩側留白（取代舊 1400px）
長段落護欄：max-width: 72ch     ← 文字塊限行長、寬螢幕下避免一行上百字難讀（grid/卡片/表格不受限）
頁面 padding：32px    ← 兩側（桌機）
手機 padding：14px    ← ≤640px；32px 在 390px 螢幕上吃掉 16% 寬度，卡片內距同時收到 16px
                        （範本已內建 640px 斷點）
Hero 上 padding：80px
Hero 下 padding：56px
Section 上 margin：72px
Section intro 左縮排：50px  ← 對齊 section index 編號的視覺
卡片 padding：18-20px
卡片 grid gap：20px
TOC pill gap：8px
```

## 為什麼這套 token

- **暖色不刺眼**：`#FAF9F5` ivory 比純白柔和、長閱讀不疲勞
- **clay 是 Anthropic 招牌**：對齊官方品牌、立刻識別「這是 Claude 做的」
- **Serif 標題**：editorial / book / magazine 質感、提升閱讀儀式感（強調改用字重或螢光筆，中文沒有斜體）
- **三字體分工**：serif（標題權威）/ sans（內文可讀）/ mono（metadata 機器感）— 角色清楚不混
- **Warm gray**：g100-g700 是暖灰、跟 ivory 同色系、不會出現「黑白藍」科技感
- **1.5px 邊線**：比 1px 厚實、比 2px 不刺、editorial 風的 detail
- **語意色 = warm 對齊**：成功用 olive 不用 emerald、失敗用 clay-d 不用 ruby、整體調性一致

## 風格對照（vs 純功能 / 科技風）

| 維度 | Anthropic 風（推薦）| 純功能風（過去版本）|
|---|---|---|
| 底色 | `#FAF9F5` ivory | `#fafafa` 冷白 |
| 主 accent | `#D97757` clay 赤陶 | `#2563eb` cool 藍 |
| 標題字體 | serif（Georgia）| sans only |
| Italic 強調 | h1 em + clay 色 | 不用 |
| Eyebrow | mono uppercase + clay 短線 | 不用 |
| Section 編號 | mono 數字 `01` / `02` | 圈號 ②③ 或字母 ABC |
| 卡片邊 | 1.5px g300 | 1px 冷灰 |
| 灰階 | warm gray | cool gray |
| Hover | translateY(-3px) + 浮起 | border 變深 |
| 整體 | editorial / book | dashboard / functional |

→ **預設用 Anthropic 風**。除非使用者明確要「dashboard / 工程儀表板」風格、再用純功能風。
