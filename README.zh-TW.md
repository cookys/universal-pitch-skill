# universal-pitch-skill

將任何 SaaS 專案的程式碼，轉化為投資級 pitch deck 和高轉換率 landing page 的 Claude Code skill。

## 功能

四階段多 agent pipeline：

| 階段 | Agent | 輸入 | 輸出 |
|------|-------|------|------|
| 1. **探索** | Project Scanner | 你的 codebase | 產品本質（JSON） |
| 2. **分析** | UX Architect | Scanner 輸出 | Persona、痛點、JTBD |
| 3. **製作** | Marketing Pro | UX 分析 | Pitch deck + landing page |
| 4. **審查** | Quality Gate | 草稿 | Go/no-go 評估 |

## 為什麼需要這個

市面上所有 pitch 工具（Gamma、Beautiful.ai、Slidebean）都需要你手動描述產品。沒有任何工具能讀你的 codebase，自動理解你做了什麼、給誰用、為什麼重要。

這個 skill 用 YC 方法論（Seibel 七問）、JTBD（Jobs to Be Done）和 Value Proposition Canvas 來橋接這個 gap。

## 支援有 UI 和無 UI 的產品

- **Web app / 儀表板**：Playwright 自動截圖 + 匿名化 + 隱藏導覽列
- **CLI / API / 基礎設施**：Input → Magic → Output 視覺公式（code snippet、terminal 錄製、架構圖）

## 安裝

### Claude Code Marketplace（推薦）

```bash
claude install github:cookys/universal-pitch-skill
```

### 手動安裝

```bash
# 方法 1: Clone 為 plugin
git clone https://github.com/cookys/universal-pitch-skill.git \
  ~/.claude/plugins/pitch-craft

# 方法 2: 只複製 skill 到專案
cp -r universal-pitch-skill/skills/pitch-craft/ \
  your-project/.claude/skills/pitch-craft/
```

## 使用方式

安裝後，以下對話會自動觸發 skill：

- 「幫這個專案做 pitch」
- 「把這個新功能加到 pitch deck」
- 「怎麼跟投資人介紹這個？」
- 「更新 landing page」
- 「重截產品截圖」
- 「我們的目標用戶是誰？」

也可以單獨跑某個階段：

- **只探索**：「這個產品到底在做什麼？」
- **更新既有**：「把新的搜尋功能加到 pitch」
- **只審查**：「檢查 pitch deck 準備好了沒」

## 檔案結構

```
.claude-plugin/
├── plugin.json                   # Plugin metadata（名稱、版本、作者）
└── marketplace.json              # Claude Code marketplace 註冊
skills/
└── pitch-craft/
    ├── SKILL.md                  # 主 skill — 四階段 pipeline
    └── references/
        ├── frameworks.md         # YC 七問、JTBD、VPC、Mom Test、2x2 矩陣
        ├── screenshot-automation.md  # Playwright 模板、匿名化、同步
        └── visual-assets.md      # 非 UI：code snippet、terminal GIF、架構圖
```

## 包含的框架

| 框架 | 來源 | 用於 |
|------|------|------|
| **Seibel 七問** | Y Combinator | Phase 2 — pitch 驗證 |
| **JTBD（三層）** | Clayton Christensen | Phase 2 — 痛點分析 |
| **Value Proposition Canvas** | Strategyzer | Phase 2 — product-market fit |
| **Mom Test** | Rob Fitzpatrick | Phase 2 — 訪談轉 pitch |
| **三層提煉** | 原創 | Phase 3 — 功能 → 能力 → 情感 |
| **Input-Magic-Output** | Stripe/Twilio 模式 | Phase 3 — 非 UI 視覺策略 |

## 系統需求

- [Claude Code](https://claude.ai/code)（CLI、桌面 app、或 IDE 插件）
- 無其他依賴 — skill 是純 Markdown 指引

選用（截圖自動化）：
- Playwright MCP plugin
- 運行中的 web app 實例（GUI 產品）

## 授權

MIT

## 致謝

研究來源：Y Combinator（Michael Seibel、Dalton Caldwell）、The Mom Test（Rob Fitzpatrick）、Jobs to Be Done（Clayton Christensen）、Value Proposition Canvas（Strategyzer）、CrewAI multi-agent 架構。

Built with [Claude Code](https://claude.ai/code)。
