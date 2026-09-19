# DevFlow

**A general-purpose AI workflow skill for closed-loop execution, verification, evidence, and handoff.**

讓 AI 養成一套工作方式：**做完要驗、驗完要留證據、失敗要修正再驗、需要真人確認的不能自己宣布完成。**

不是專案管理工具，不是狀態機軟體，沒有程式要跑。這個 repo 裡全部都是文字規則——安裝＝把它放進 AI 的 skills 資料夾。

---

## 這個 Skill 解決什麼

AI 助理最常見的六種「假完成」：

| 症狀 | DevFlow 的對策 |
|---|---|
| 做到一半就說完成 | 回報用語必須對齊實際進度，做到哪只能說到哪 |
| 測試通過就說已上線 | 測試通過 ≠ 已部署 ≠ 真人使用正常，三段分開講 |
| 寫出檔案卻沒確認能不能打開 | 文件類工作一定要實際開檔確認 |
| 改完設定沒有實際讀回 | 設定類工作分兩層：值讀得回來、行為真的變了 |
| 不確定卻用推測補答案 | 不知道就標 `UNKNOWN`，驗不了就標 `UNVERIFIED` |
| 換聊天室或換 Agent 後進度消失 | 中斷前留十項交接資訊，接手方先讀再動手 |

核心紀律只有四句：

1. 執行完成 ≠ 驗證完成
2. 驗證完成 ≠ 交付成功
3. 測試通過 ≠ 已部署
4. 部署成功 ≠ 真人使用正常

加一條紅線：**AI 不得冒充真人驗收。**

---

## 適用範圍

全領域，不只寫程式：程式開發、網站修改、文件製作、企畫與內容、資料整理、研究分析、系統設定、部署、自動化、系統維護。

通用層只有一個閉環：

```
執行 → 驗證 → 留證據 → 判斷是否通過
  ↑                        │
  └──── 修正 ←── 未通過 ────┘
                           │ 通過
         必要時交付／部署 → 真人驗收 → 結案
```

不同類型的工作**只在「怎麼算驗過」上不同**，不會被硬塞工程術語。只有 coding 類工作才額外套 `Implemented → Tested → Deployed → Human Verified`。

---

## 安裝

需求：一個支援 `SKILL.md` 格式的 AI 工具（Claude Code、Codex 等）。不需要 Node、Python 或任何執行環境，不改 PATH，不需要管理員權限。

把這個 repo clone 進 skills 資料夾，命名為 `devflow`：

```bash
# Claude Code
git clone https://github.com/ITOKORABBIT/devflow-skill.git ~/.claude/skills/devflow

# Codex
git clone https://github.com/ITOKORABBIT/devflow-skill.git ~/.codex/skills/devflow
```

Windows（PowerShell）：

```powershell
git clone https://github.com/ITOKORABBIT/devflow-skill.git "$env:USERPROFILE\.claude\skills\devflow"
```

不用 git 也可以：下載 ZIP，解壓後把整個資料夾放進 skills 目錄並命名為 `devflow`。

重開 AI 工具後生效。確認方式：問它「你有 devflow skill 嗎？講一下它的四條不等於」。

### 更新

```bash
cd ~/.claude/skills/devflow && git pull
```

### 讓它變成預設行為（選用）

Skill 通常由 AI 依情境自行判斷是否套用。想讓它**每次都套用**，在你的全域指示檔（例如 `~/.claude/CLAUDE.md` 或 `~/.codex/AGENTS.md`）加一行：

```markdown
所有實際工作預設套用 devflow skill：做完要驗、驗完留證據、失敗修正再驗、真人驗收不得由 AI 代簽。
```

這一步是你自己決定要不要做，Skill 本身不會去改你的設定檔。

---

## 移除

刪掉那個資料夾就好，不留任何殘留：

```bash
rm -rf ~/.claude/skills/devflow
```

如果你有加上面那行全域指示，把那一行刪掉。

---

## 內容

| 檔案 | 用途 |
|---|---|
| `SKILL.md` | **Skill 本體**。AI 讀的就是這份，定義它的工作行為 |
| `references/verification.md` | 各類工作怎麼算「驗過」：判準、證據、常見假完成 |
| `references/evidence.md` | 證據規則：什麼算證據、怎麼記、UNKNOWN 與 UNVERIFIED |
| `references/human-verification.md` | 真人驗收規則與 AI 的紅線 |
| `references/handoff.md` | 交接十項規格、接手方義務、多層交接 |
| `references/work-records.md` | 需要時在專案內留的輕量紀錄 `.ai-workflow/` |
| `templates/` | 狀態與交接的空白模板 |
| `examples/` | 實際跑過的案例，含刻意留下的「驗不了」情況 |

`references/` 由 AI 在需要時才讀取，不會一次全部佔用上下文。

---

## 案例

- [程式開發：測試全過，安裝測試才抓到兩個 bug](examples/01-coding.md)
- [研究分析：先有來源才有結論](examples/02-research.md)
- [系統設定：讀得回值，但行為這一層當下驗不了](examples/03-system-config.md)
- [文件製作：內容、事實、格式要分三次檢查](examples/04-document.md)
- [交接：換 Agent 前留下什麼](examples/05-handoff.md)

---

## 設計取捨

- **沒有程式**。規則要能被讀懂、被質疑、被改，不需要執行環境。
- **不強制留檔**。小事不建檔，只有跨 session、有交付物或會中斷的工作才留 `.ai-workflow/`。
- **不取代既有流程**。它管的是「怎麼算做完」，不管你用什麼工具追蹤任務。
- **真人驗收靠紀律而非技術**。這是一套工作方式，不是防竄改系統；它擋的是 AI 自己越線，不是惡意偽造。

---

## 授權

MIT
