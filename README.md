# My OpenCode Skills

个人 OpenCode 技能仓库，用于存放和管理自定义 AI 技能 (Skills)。

## 仓库结构

```
my-skills/
├── README.md                 # 本文件：仓库说明
├── .gitignore               # Git 忽略规则
├── env-checker/             # 环境检测技能
│   └── SKILL.md
├── <future-skill-1>/        # 未来新增的技能
│   └── SKILL.md
└── <future-skill-2>/
    └── SKILL.md
```

## 现有技能

### env-checker

- **功能**：全面检测本地开发环境配置
- **覆盖范围**：Python、Node.js、Git、系统代理、Docker、Go 等常见开发工具
- **特点**：
  - 自动检测国内镜像配置（清华、阿里云、npm 淘宝镜像等）
  - 识别 Clash/V2Ray/Shadowsocks 代理配置
  - 生成结构化的检测报告
  - 提供代理排错指南
- **适用场景**：
  - 新机器环境搭建
  - 排查包安装失败问题
  - 验证代理配置是否正确

## 使用方法

### 本地使用

将本仓库克隆到 OpenCode 的 skills 目录：

```bash
# 克隆到 OpenCode skills 目录
git clone https://github.com/<your-username>/my-skills.git ~/.config/opencode/skills

# 或在 Windows 上
git clone https://github.com/<your-username>/my-skills.git %USERPROFILE%\.config\opencode\skills
```

### 在 OpenCode 中调用

安装后，在对话中直接提及技能名称即可触发：

```
检查一下我的开发环境
/env-checker
```

## 添加新技能

1. 创建新的技能目录：`mkdir my-new-skill`
2. 编写 `SKILL.md`，参考 [OpenCode Skill 规范](https://docs.opencode.ai/skills)
3. 在 README 中添加技能说明
4. 提交并推送到 GitHub

### SKILL.md 模板

```markdown
---
name: skill-name
description: 简短描述该技能的作用
license: MIT
compatibility: opencode
metadata:
  category: your-category
  platform: cross-platform
  extensible: true
---

## What I do

详细描述技能的功能和使用场景。

## How to use

使用示例和说明。
```

## 贡献

本仓库为个人技能集合，欢迎通过 Issue 或 PR 提出改进建议。

## License

MIT License - 详见各技能目录中的 `SKILL.md` 文件。
