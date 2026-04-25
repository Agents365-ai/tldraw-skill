# tldraw-skill — 从文字到白板风格图表

[English](README.md)

## 功能说明

- 根据自然语言描述生成 `.tldr` JSON 文件
- 使用 `@kitschpatrol/tldraw-cli` 将图表导出为 PNG 或 SVG
- **6 种图表类型预设**：架构图、流程图、时序图、ML/深度学习模型图、ER 图、UML 类图 —— 每种类型都有预设的形状词汇和布局规范
- **自检循环**：使用视觉能力读取导出的 PNG，自动修复重叠、文字截断和缺失箭头等问题
- **迭代评审循环**：收集反馈，应用精准 JSON 编辑，重新导出直至确认（最多 5 轮安全阀）
- **复杂度自适应布局**：间距随节点数自动放大，防止重叠
- **自动更新**：每 24 小时检查一次最新版本
- **自定义输出目录**：支持任意输出路径（如 `./artifacts/`），自动创建目录
- 当图表有助于解释复杂系统时自动触发
- 默认手绘白板风格，可切换为干净字体

## 多平台支持

支持所有兼容 [Agent Skills](https://agentskills.io) 格式的 AI 编程助手：

| 平台 | 状态 | 说明 |
|------|------|------|
| **Claude Code** | ✅ 完整支持 | 原生 SKILL.md 格式 |
| **Opencode** | ✅ 完整支持 | 通过 `skill` 工具调用，同时读取 `.claude/skills/` 路径 |
| **OpenClaw / ClawHub** | ✅ 完整支持 | `metadata.openclaw` 命名空间、依赖守卫、ClawHub 安装器 |
| **Hermes Agent** | ✅ 完整支持 | `metadata.hermes` 命名空间、tags、工具守卫 |
| **OpenAI Codex** | ✅ 兼容 | 放置在 `.agents/skills/` 即可 |
| **SkillsMP** | ✅ 已索引 | 已配置 GitHub topics |

## 对比

### vs 原生 Agent（无 skill）

| 特性 | 原生 Agent | 本 skill |
|------|-----------|---------|
| 生成 `.tldr` JSON | 部分 —— LLM 经常产出 schema 不合规的记录 | ✅ schema 正确的骨架 + 记录模板 |
| 导出后自检 | ❌ | ✅ 读取 PNG 自动修复 6 类问题 |
| 迭代评审循环 | ❌ —— 需手动反复 prompt | ✅ 精准编辑，5 轮安全阀 |
| 主动触发 | ❌ —— 只有显式要求才触发 | ✅ 检测到 3+ 组件自动建议 |
| 布局规范 | 无 —— 每次结果不一致 | 复杂度自适应间距、路由走廊、枢纽节点居中 |
| 图表类型预设 | ❌ | ✅ 6 种（架构、流程、时序、ML/DL、ERD、UML） |
| 配色方案 | 随机 / 不一致 | 10 色语义系统（蓝=服务，绿=DB，紫=认证……） |
| 箭头分布规则 | 基础 | `normalizedAnchor` 在形状边缘均匀分布，避免堆叠 |
| Index 排序规则 | 经常错误（用 `b1`、`c1`） | 严格 `a*` 格式，含 z-order 约定 |
| 多平台元数据 | ❌ | ✅ OpenClaw、Hermes、SkillsMP 命名空间 |

### vs 其他图表 skill

| 特性 | tldraw-skill | drawio-skill | mermaid-skill | excalidraw-skill |
|------|--------------|--------------|---------------|------------------|
| **风格** | 手绘白板 | 干净专业 | 自动布局文字 | 草图非正式 |
| **格式** | `.tldr` JSON | `.drawio` XML | 文本 DSL | `.excalidraw` JSON |
| **导出格式** | PNG, SVG | PNG, SVG, PDF, JPG | PNG, SVG, PDF | PNG, SVG |
| **手动布局控制** | ✅ x/y 坐标 | ✅ x/y 坐标 | ❌ 仅自动布局 | ✅ x/y 坐标 |
| **自检循环** | ✅ 基于视觉 | ✅ 基于视觉 | 部分 | 部分 |
| **图表预设** | ✅ 6 种 | ✅ 6 种 | 由文本语法驱动 | 无 |
| **样式预设** | ❌ | ✅ 用户可学习 | ❌ | ❌ |
| **适合场景** | 白板草图、轻量解释、内部文档 | 商务/学术正式图 | 文档中的快速文转图 | 手绘风格演示 |

## 支持的图表类型

- **架构图**：微服务、云架构、部署 —— 按层着色、枢纽节点居中
- **流程图**：业务流程、决策树、状态机 —— 语义化形状类型
- **时序图**：用矩形 + 水平箭头近似演员消息流
- **ML/深度学习**：按层类型着色、张量形状标注
- **ERD**：实体用多行文本矩形，箭头标注基数
- **UML 类图**：类用多行文本矩形，关系用不同箭头

## 依赖项

```bash
# 安装 tldraw-cli
npm install -g @kitschpatrol/tldraw-cli

# 验证
tldraw --version
```

需要 Node.js（npm）。所有平台安装方式完全一致 —— 无需额外配置，无需浏览器自动化。

## Skill 安装

### Claude Code

```bash
# 全局安装（所有项目可用）
git clone https://github.com/Agents365-ai/tldraw-skill.git ~/.claude/skills/tldraw-skill

# 项目级安装
git clone https://github.com/Agents365-ai/tldraw-skill.git .claude/skills/tldraw-skill
```

### Opencode

```bash
# 全局安装（Opencode 原生路径）
git clone https://github.com/Agents365-ai/tldraw-skill.git ~/.config/opencode/skills/tldraw-skill

# 项目级安装
git clone https://github.com/Agents365-ai/tldraw-skill.git .opencode/skills/tldraw-skill
```

Opencode 也会读取 `~/.claude/skills/` 和 `.claude/skills/`，所以已有 Claude Code 安装会自动被识别 —— 无需重复 clone。

### OpenClaw / ClawHub

```bash
# 通过 ClawHub CLI（推荐）
clawhub install tldraw-pro-skill

# 手动安装
git clone https://github.com/Agents365-ai/tldraw-skill.git ~/.openclaw/skills/tldraw-skill

# 项目级安装
git clone https://github.com/Agents365-ai/tldraw-skill.git skills/tldraw-skill
```

### Hermes Agent

```bash
git clone https://github.com/Agents365-ai/tldraw-skill.git ~/.hermes/skills/design/tldraw-skill
```

### OpenAI Codex

```bash
git clone https://github.com/Agents365-ai/tldraw-skill.git ~/.agents/skills/tldraw-skill
# 或项目级
git clone https://github.com/Agents365-ai/tldraw-skill.git .agents/skills/tldraw-skill
```

### 安装路径汇总

| 平台 | 全局路径 | 项目路径 |
|------|---------|---------|
| Claude Code | `~/.claude/skills/tldraw-skill/` | `.claude/skills/tldraw-skill/` |
| Opencode | `~/.config/opencode/skills/tldraw-skill/`（也读取 `~/.claude/skills/`） | `.opencode/skills/tldraw-skill/` |
| OpenClaw | `~/.openclaw/skills/tldraw-skill/` | `skills/tldraw-skill/` |
| Hermes Agent | `~/.hermes/skills/design/tldraw-skill/` | 通过 `external_dirs` 配置 |
| OpenAI Codex | `~/.agents/skills/tldraw-skill/` | `.agents/skills/tldraw-skill/` |

## 更新

skill 在每次会话首次使用时（每 24 小时一次）自动检查更新（一次 `git pull --ff-only`）。已是最新版、离线、或非 git 安装时会静默跳过 —— 不会阻塞或拖慢工作流。

手动更新：

```bash
cd <你的安装路径>/tldraw-skill && git pull
```

包管理器安装的版本由其自己管理更新：

```bash
# ClawHub
clawhub update tldraw-pro-skill
```

## 使用方式

直接描述你想要的图表：

```
画一个微服务电商架构图，包含 API Gateway、用户/订单/商品/支付服务、
Kafka 消息队列、通知服务，以及各自独立的数据库
```

Agent 会规划布局、生成 `.tldr` JSON、导出 PNG、自检，然后让你迭代。

## 示例

**提示词：**
> 画一个微服务电商架构图，包含 Mobile/Web/Admin 客户端，API Gateway，
> User/Order/Product/Payment 微服务，Kafka 事件总线，Notification 服务，
> User DB / Order DB / Product DB / Redis Cache / Stripe API

**输出效果：**

![微服务架构图](assets/example.png)

## 文件说明

- `SKILL.md` —— **唯一必需文件**。所有平台都加载此文件作为 skill 指令
- `README.md` —— 英文说明（GitHub 主页显示）
- `README_CN.md` —— 本文件（中文）
- `assets/` —— 示例图表（可安全删除以节省空间）

> 所有示例图表均由 Claude 使用本 skill 生成。

## 已知限制

- **原生 UML 标记**：tldraw 箭头有限（不支持继承所需的空心三角形）。学术论文中的严格 UML/ERD 图请使用 drawio-skill
- **无原生容器/泳道**：tldraw 的分组模型与 drawio 不同。请用颜色和间距实现视觉分组
- **不支持 PDF 导出**：tldraw-cli 仅支持 PNG 和 SVG。如需 PDF，可将 SVG 后处理转换（如 `rsvg-convert`）
- **自检需要视觉能力**：自动修复步骤通过模型的视觉能力读取 PNG。不支持视觉的模型会跳过此步

## 开源协议

MIT

## 支持作者

如果这个 skill 对你有帮助，欢迎支持作者：

<table>
  <tr>
    <td align="center">
      <img src="https://raw.githubusercontent.com/Agents365-ai/images_payment/main/qrcode/wechat-pay.png" width="180" alt="微信支付">
      <br>
      <b>微信支付</b>
    </td>
    <td align="center">
      <img src="https://raw.githubusercontent.com/Agents365-ai/images_payment/main/qrcode/alipay.png" width="180" alt="支付宝">
      <br>
      <b>支付宝</b>
    </td>
    <td align="center">
      <img src="https://raw.githubusercontent.com/Agents365-ai/images_payment/main/qrcode/buymeacoffee.png" width="180" alt="Buy Me a Coffee">
      <br>
      <b>Buy Me a Coffee</b>
    </td>
    <td align="center">
      <img src="https://raw.githubusercontent.com/Agents365-ai/images_payment/main/awarding/award.gif" width="180" alt="赏个奖励">
      <br>
      <b>赏个奖励</b>
    </td>
  </tr>
</table>

## 作者

**Agents365-ai**

- Bilibili: https://space.bilibili.com/441831884
- GitHub: https://github.com/Agents365-ai
