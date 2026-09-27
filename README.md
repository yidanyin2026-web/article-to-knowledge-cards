# make-knowledge-cards

`make-knowledge-cards` 是一个面向 Codex 的可复用 Skill，用于把文章整理成简洁、可复习且忠于原文的知识卡片。

## 项目解决什么问题

长文章通常包含重复表述、背景材料和多个交织的观点，直接阅读后不易复习；普通摘要又可能把多个知识点混在一起，或者补充原文没有的信息。

本项目从用户粘贴的文章或本地 Markdown/TXT 文件中提取真正重要的内容，将其整理为相互独立的知识卡片。每张卡片只讲一个知识点，并严格以原文为事实依据。

## 主要功能

- 支持用户直接粘贴文章正文。
- 支持读取本地 `.md` 和 `.txt` 文件。
- 优先提取核心主张、定义、机制、步骤、区别、限制和有依据的结论。
- 合并语义重复的内容，过滤装饰性背景和次要细节。
- 每张卡片包含标题、核心知识、简明解释，以及原文支持的例子或自测问题。
- 通常输出 5–8 张卡片；原文信息不足时会输出更少的卡片，不重复内容或编造信息凑数。
- 保留原文中的限定条件、不确定性和分歧，并根据用户语言生成卡片。
- 使用标准 Markdown 输出，不依赖额外脚本或第三方运行库。

当前不支持网页抓取、PDF、Anki 导出或图形界面。

## 安装方法

本项目的 Skill 位于 `skills/make-knowledge-cards`。根据使用范围，将该目录复制到 Codex 的对应 Skill 目录即可。

### 项目级安装

只在当前代码仓库中使用：

```text
<你的项目>/.codex/skills/make-knowledge-cards/
├── SKILL.md
└── agents/
    └── openai.yaml
```

在 PowerShell 中，从本仓库根目录执行：

```powershell
New-Item -ItemType Directory -Force ".codex\skills" | Out-Null
Copy-Item -Recurse ".\skills\make-knowledge-cards" ".codex\skills\"
```

### 用户级安装

供本机上的所有项目使用：

```powershell
New-Item -ItemType Directory -Force "$HOME\.codex\skills" | Out-Null
Copy-Item -Recurse ".\skills\make-knowledge-cards" "$HOME\.codex\skills\"
```

安装后的目录应为 `~/.codex/skills/make-knowledge-cards/SKILL.md`。Codex 官方文档也说明了[项目级与用户级 Skill 目录](https://developers.openai.com/blog/eval-skills#2-create-the-skill)。

## 使用方法

在 Codex 对话中使用 `$make-knowledge-cards` 显式调用 Skill，并提供文章正文或文件路径。

### 粘贴文章

```text
使用 $make-knowledge-cards，把下面的文章转换成知识卡片：

<在这里粘贴文章>
```

### 读取本地文件

```text
使用 $make-knowledge-cards，把 notes/learning.md 转换成知识卡片。
```

本地文件必须是 Markdown 或纯文本格式。文件无法读取时，Skill 会要求提供有效路径或直接粘贴正文。

## 输入输出示例

输入：

```text
间隔复习把学习安排在逐渐拉长的时间间隔中。主动回忆要求学习者在查看答案前先尝试提取信息。文章建议把两者结合：在每次间隔复习时先主动回忆，再核对答案。
```

输出：

```markdown
共 3 张卡片

> 原文只包含 3 个彼此独立的重要知识点，因此未强行补足到 5 张。

## 卡片 1：间隔复习

**核心知识：** 间隔复习把学习安排在逐渐拉长的时间间隔中。

**简明解释：** 它强调将多次复习分散在逐渐拉长的时间间隔中。

**自测问题：** 间隔复习如何安排学习时间？

## 卡片 2：主动回忆

**核心知识：** 主动回忆要求学习者在查看答案前先尝试提取信息。

**简明解释：** 学习者需要先自行回忆，再查看答案。

**自测问题：** 主动回忆要求学习者在查看答案前做什么？

## 卡片 3：结合两种学习方法

**核心知识：** 文章建议在每次间隔复习时先主动回忆，再核对答案。

**简明解释：** 这一方法把复习时间安排与主动提取信息结合起来。

**例子：** 到达一次复习时间点后，先尝试回忆内容，然后核对答案。
```
