# tldraw-skill

用于在 Claude Code 中生成 tldraw 白板风格图表并本地导出为 PNG/SVG 的 skill。

[English](README.md)

## 功能说明

- 根据自然语言描述生成 `.tldr` JSON 文件
- 使用 `@kitschpatrol/tldraw-cli` 将图表导出为 PNG 或 SVG
- 生成现代手绘白板风格图表，支持丰富的形状库
- 当图表有助于解释复杂系统时自动触发

## 依赖项

| 工具 | 用途 |
|------|------|
| `@kitschpatrol/tldraw-cli` | CLI 工具，用于将 `.tldr` 导出为 PNG/SVG |

需要 Node.js（npm）。无需浏览器自动化。

## 安装

### macOS / Windows / Linux

```bash
# 安装 tldraw-cli
npm install -g @kitschpatrol/tldraw-cli

# 验证安装
tldraw --version
```

所有平台安装方式完全一致，无需额外配置。

## 使用方式

直接描述你想要的图表：

```
画一个微服务电商架构图，包含 API Gateway、用户/订单/商品/支付服务、
Kafka 消息队列、通知服务，以及各自独立的数据库
```

Claude 会自动生成 `.tldr` 文件并导出为 PNG。

## 示例

**提示词：**
> 画一个微服务电商架构图，包含 Mobile/Web/Admin 客户端，API Gateway，
> User/Order/Product/Payment 微服务，Kafka 事件总线，Notification 服务，
> User DB / Order DB / Product DB / Redis Cache / Stripe API

**输出效果：**

![微服务架构图](assets/example.png)

## 文件说明

- `SKILL.md` — Claude Code 加载的 skill 指令文件
- `README.md` — 英文说明
- `README_CN.md` — 本文件（中文）
- `assets/` — 示例图表

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
  </tr>
</table>

## 作者

**Agents365-ai**

- Bilibili: https://space.bilibili.com/441831884
- GitHub: https://github.com/Agents365-ai
