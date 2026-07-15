# 🐾 不一书桌宠生成器

`bys-codex-pet` 是一个面向 AI 新手的 Codex Skill：把真人照片、宠物照片、已有角色图或一句文字想法，分阶段制作成可以安装和分享的 Codex v2 桌宠。

它不会重新实现桌宠底层协议，而是负责更友好的中文引导，并复用 OpenAI 的 `hatch-pet` 与 `imagegen`：

- 默认使用简体中文，除非用户明确要求其他语言
- 一张图片也能开始，无图片时可制作原创角色
- 角色定稿、标准动作、最终桌宠三次确认
- 依赖缺失时给出恢复方案并保存进度
- Codex bundled Python 缺失时，可在用户明确同意后自动发现并验证本机 Python/Pillow 兼容运行时
- 支持 Windows 和 macOS 安装
- 导出离线 ZIP，可选生成 `codex://pets/install` 一键安装链接
- 默认不把真人照片、生成缓存或密钥放入分享包

## 安装

把本仓库上传到 GitHub 后，可以在 Codex 中让 `$skill-installer` 安装仓库内的 Skill：

```text
使用 $skill-installer 从下面的 GitHub 路径安装 Skill：
https://github.com/qkgecn93/bys-codex-pet/tree/main/skills/bys-codex-pet
```

也可以把 `skills/bys-codex-pet` 复制到个人 Skill 目录。Codex 支持从 GitHub 仓库路径安装 Skill；安装后如果没有立即出现，请重启 Codex。

## 使用

安装后可以直接说：

```text
初始化不一书桌宠生成器
```

也可以使用：

```text
使用 $bys-codex-pet 把这张照片做成 Codex 桌宠。
使用 $bys-codex-pet 帮我继续上次的桌宠项目。
使用 $bys-codex-pet 把最终桌宠导出成朋友可以安装的分享包。
```

默认开场：

> 🐾 欢迎来到「不一书桌宠生成器」
>
> 把你喜欢的人、宠物或原创角色，变成一只陪你工作与学习的 Codex 桌宠。
>
> ✨ 你只需要提供一张照片，或者描述你的角色想法。
> 🧩 我会完成角色设计、动作制作与质量检查。
> ✅ 每个关键阶段都会先请你确认，满意后再继续。
> 📦 完成后，我还会帮你安装桌宠，并生成可分享给朋友的安装包。

## 依赖

- 支持 Pets 的 Codex/ChatGPT 桌面端
- OpenAI curated Skill：`hatch-pet`
- Codex system Skill：`imagegen`

在桌面端打开 **设置 > Pets > Create your own pet** 时，应用会安装捆绑的 `hatch-pet`。如果依赖仍缺失，Skill 会进入准备模式并保存项目状态，而不是假装完成。

`hatch-pet` 默认要求 Codex bundled Python。若当前桌面端没有配置该运行时，本 Skill 会枚举每台电脑自己的 Python 路径，验证 Python/Pillow 版本和 hatch-pet 脚本兼容性，然后请求一次明确授权。授权后始终使用验证通过的绝对解释器路径，不使用容易指向不同环境的裸 `python` 命令。

第三方模型或 API 会话可能缺少 Codex 内置 `image_gen` 工具。Python 兼容模式只能解决本地处理脚本，不能替代图片生成能力；Skill 会把两项能力分开检查。

## 三阶段确认

1. **角色定稿**：确认脸或主体、发型/毛色、服装、配饰与画风。
2. **标准动作**：确认待机、跑动、挥手、跳跃、失败、等待、工作和检查等状态。
3. **最终桌宠**：确认 v2 动画、观察方向和最终质检结果，然后才安装、导出。

## 分享方式

默认导出离线 ZIP，包含：

- `pet.json` 和 `spritesheet.webp`
- 中文安装说明
- Windows PowerShell 安装/卸载脚本
- macOS Shell 安装/卸载脚本
- 校验清单

如果最终精灵图已经放在公开 HTTPS 地址，还可以生成官方支持的：

```text
codex://pets/install?name=...&imageUrl=https%3A...&spriteVersionNumber=2
```

Skill 不会未经授权上传或公开用户文件。

## 仓库结构

```text
bys-codex-pet/
├── README.md
├── LICENSE
├── .gitignore
├── skills/bys-codex-pet/
│   ├── SKILL.md
│   ├── agents/openai.yaml
│   ├── scripts/
│   ├── references/
│   └── assets/
├── examples/
├── tests/
└── .github/workflows/validate.yml
```

## 本地验证

```powershell
py C:\path\to\skill-creator\scripts\quick_validate.py .\skills\bys-codex-pet
py -m unittest discover -s .\tests -v
```

CI 会在 Windows、macOS 和 Linux 上运行标准库测试。真实桌宠的视觉生成与最终 Pets UI 仍需要在 Codex 桌面端验收。

## 隐私与安全

- 默认不分享原始照片
- 不保存或输出完整 `OPENAI_API_KEY`
- 安装脚本只写入单个 Codex pet 目录
- 卸载脚本要求明确确认并校验目标路径
- 已安装同 id 桌宠默认不会被覆盖

## 相关官方文档

- [Codex Pets](https://learn.chatgpt.com/docs/pets)
- [Build skills](https://learn.chatgpt.com/docs/build-skills)
- [Pet install deep link](https://learn.chatgpt.com/docs/reference/commands#pets)

## License

[MIT](LICENSE) © 2026 不一书
