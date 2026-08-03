# Hungry Studio Meal Plan · 10F 排餐看板 Skill

开箱即用的 **Cursor / Claude / Codex Skill**：把 Hungry Studio 10F 周菜单照片转成可交互排餐看板 HTML——自动套用忌口划线，并推荐每顿去 **靠墙** 还是 **靠窗**。

## 特性

- 早 / 午 / 晚 × 周一–周五 × 靠墙·客林 / 靠窗·三福
- 忌口规则：肥肉、内脏、有刺鱼、茄子/芹菜/菌菇/南瓜/冬瓜等；可分离配料只划配料
- 每顿自动选线（主荤命中 + 面食偏好）
- Quiet / cool-indigo 单文件 HTML（对齐职能线工作台 Demo 视觉）
- 「只看我能吃的」一键过滤

## 安装

标准 `SKILL.md` 格式。把仓库放进个人 skills 目录（把 `<skills-dir>` 换成对应平台路径）：

```bash
# Claude / Claude Code： ~/.claude/skills
# Cursor：              ~/.cursor/skills
# Codex：               ~/.codex/skills
git clone https://github.com/milojellycd/hungry-studio-meal-plan.git \
  "<skills-dir>/hungry-studio-meal-plan"
```

或克隆后软链接：

```bash
git clone https://github.com/milojellycd/hungry-studio-meal-plan.git
ln -s "$(pwd)/hungry-studio-meal-plan" "<skills-dir>/hungry-studio-meal-plan"
```

重启或重新加载 AI 工具后即可识别。

## 用法

发一张本周菜单图，并说「排餐」/「生成看板」或调用 `/hungry-studio-meal-plan`。

输出：`~/Downloads/排餐看板-MMDD-MMDD.html`（浏览器打开，点日期切换，可开「只看我能吃的」）。

## 仓库结构

```
hungry-studio-meal-plan/
├── SKILL.md                 # Skill 主说明（触发词 + 工作流）
├── preferences.md           # 默认忌口与选线偏好（可按个人改）
└── references/
    └── html-shell.html      # Quiet indigo HTML 模板
```

改忌口：编辑 `preferences.md`，或对话里说「记住」让 Agent 更新。

## License

MIT
