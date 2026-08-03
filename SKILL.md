---
name: hungry-studio-meal-plan
description: >-
  Generates Hungry Studio 10F weekly meal-plan kanban HTML from cafeteria menu
  photos. Applies the user's food avoid list, recommends 靠墙 vs 靠窗 per meal,
  and writes a Quiet/indigo interactive HTML to Downloads. Use when the user
  sends a weekly menu image, or mentions 排餐、菜单看板、Hungry Studio、靠墙/靠窗、
  客林/三福、meal plan, or asks to update this week's cafeteria plan.
---

# Hungry Studio 10F 排餐看板

从本周菜单图生成可交互 HTML 排餐看板（忌口划线 + 每顿推荐靠墙/靠窗）。

## 输出

- 路径：`~/Downloads/排餐看板-MMDD-MMDD.html`（日期取菜单周区间）
- 写完后 `open` 该文件
- 用「靠墙 / 靠窗」，**不要**写线一/线二（用户反应不过来）
  - 靠墙＝餐线一·客林
  - 靠窗＝餐线二·三福

## 工作流

1. **读菜单图**：按 早/午/晚 × 周一–周五 × 靠墙/靠窗 逐格提取菜名。密图可裁切后识读；认不准的菜名后加 `?`，回复里点名让用户核对。
2. **套用忌口**：见下方规则；详情可对 [preferences.md](preferences.md)。
3. **每顿选线**：对比两条线——可吃的主荤/半荤越多越好；踩雷整道越多越差；小吃偏好 面/炒面/凉皮 > 饼 > 包子饺 > 杂粮 > 米饭/馒头。
4. **生成 HTML**：复制 [references/html-shell.html](references/html-shell.html)，填入本周 `DAYS` 数据与标题日期；**保留现有 CSS**（Quiet / cool-indigo，对齐职能线工作台 Demo）。不要改回马卡龙/高饱和分区配色。
5. **回复**：给「每顿去哪吃」总表 + 2–4 个亮点顿 + 文件路径。不确定的菜名单独列出。

## 忌口（默认）

整道避开或按规则部分划掉：

| 类 | 内容 |
|---|---|
| 肥肉 | 五花、红烧肉、白肉、梅花肉、腊肉/腊肠、回锅肉、猪蹄、扣肉 |
| 内脏 | 毛血旺、肥肠、牛杂；杀猪菜默认整道避开 |
| 鱼 | 有刺的（清蒸/烤鱼/鱼头/酸汤鱼/瓦块鱼/小黄鱼等）；无刺可吃（巴沙、鳕鱼块） |
| 蔬果配料 | 茄子、芹菜、苦瓜/苦菜、菌菇（香菇/杏鲍菇/平菇/鲍菇/杂菌等）、韭菜、南瓜/贝贝瓜/金瓜、冬瓜 |
| 辣度 | 全能吃，不构成限制 |
| 默认可吃 | 香菜、木耳、海带、豆腐/豆制品、胡萝卜、红薯/紫薯、葱姜蒜、虾 |

用户本轮若更新忌口，以本轮为准，并提醒是否写入 preferences。

## 划线规则

数据用三种标记（与 shell 里 `y` / `x` / `p` 一致）：

- `y("菜名")`：能吃
- `x("菜名")`：忌口且**分不开** → 整道划掉（如芹菜包子、蒸鱼头、香菇肉包、黄金南瓜饼）
- `p("菜名", "配料")`：能吃，但把配料划掉（如 `p("芹菜炒肉丝","芹菜")`、`p("青笋木耳炒培根","培根")`、`p("香菇炒油菜","香菇")`）

原则：**可挑出的配料只划配料，整道仍算能吃；只有分不开才整道划掉。**  
「只看我能吃的」过滤保留 `ok:true`（含部分划线），隐藏 `ok:false`。

## HTML 结构要求

Shell 已实现，勿破坏交互：

- 顶部「每顿去哪吃」总表（推荐显示为 indigo soft pill）
- 忌口 callout
- 日期 Pill 切换 +「只看我能吃的」
- 每顿卡片：左右列靠墙·客林 / 靠窗·三福；推荐列 `is-rec` +「（推荐）」badge

`DAYS` 形状：

```js
{ id: "周一", label: "周一 M/D", meals: [
  { label: "早餐"|"午餐"|"晚餐", rec: "靠墙"|"靠窗",
    靠墙: [{ c: "品类", items: [y()/x()/p()] }],
    靠窗: [/* 同结构，品类顺序与靠墙对齐 */] }
]}
```

早餐品类常用：主食/点心、油炸/鸡蛋、杂粮、小菜、流食。  
午/晚常用：主荤、半荤、素菜、主食/面食、小吃、汤/水果或汤/饮品。

## 设计约束

- 视觉对齐 `职能线工作台Demo`：paper 底、细 rule 线、cool-indigo 强调、Inter + JetBrains Mono
- **禁止**：彩虹/马卡龙渐变表头、靠墙黄/靠窗绿硬色块分区、高饱和多色背景
- 表头三列同一 `paper-2` 底；推荐仅用文字色 + 底边 accent 指示

## 完成后自检

- [ ] 用语全是靠墙/靠窗
- [ ] 每顿都有 `rec`，且与忌口逻辑一致
- [ ] 可分离配料用了 `p`，分不开用了 `x`
- [ ] 文件在 Downloads，已 open
- [ ] 未改坏 Quiet indigo CSS
