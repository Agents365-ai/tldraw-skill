# tldraw-skill

Claude Code skill for generating tldraw whiteboard diagrams and exporting to PNG/SVG locally.

[中文文档](README_CN.md)

## What it does

- Generates `.tldr` JSON files from natural language descriptions
- Exports diagrams to PNG or SVG using `@kitschpatrol/tldraw-cli`
- Produces modern hand-drawn whiteboard aesthetic with rich shape library
- Triggers automatically when diagrams would help explain complex systems

## Dependencies

| Tool | Purpose |
|------|---------|
| `@kitschpatrol/tldraw-cli` | CLI to export `.tldr` → PNG/SVG |

Requires Node.js (npm). No browser automation required.

## Install

### macOS / Windows / Linux

```bash
# Install tldraw-cli
npm install -g @kitschpatrol/tldraw-cli

# Verify
tldraw --version
```

Works identically on all platforms — no extra setup required.

## Usage

Just describe what you want:

```
Create a microservices e-commerce architecture with API Gateway, auth/user/order/product/payment services,
Kafka message queue, notification service, and separate databases for each service
```

Claude will generate the `.tldr` JSON file and export it to PNG automatically.

## Example

**Prompt:**
> Create a microservices e-commerce architecture with Mobile/Web/Admin clients, API Gateway,
> User/Order/Product/Payment services, Kafka event bus, Notification service,
> and User DB / Order DB / Product DB / Redis Cache / Stripe API

**Output:**

![Microservices Architecture](assets/example.png)

## Files

- `SKILL.md` — skill instructions loaded by Claude Code
- `README.md` — this file (English)
- `README_CN.md` — Chinese documentation
- `assets/` — example diagrams

## License

MIT

## Support

If this skill helps you, consider supporting the author:

<table>
  <tr>
    <td align="center">
      <img src="https://raw.githubusercontent.com/Agents365-ai/images_payment/main/qrcode/wechat-pay.png" width="180" alt="WeChat Pay">
      <br>
      <b>WeChat Pay</b>
    </td>
    <td align="center">
      <img src="https://raw.githubusercontent.com/Agents365-ai/images_payment/main/qrcode/alipay.png" width="180" alt="Alipay">
      <br>
      <b>Alipay</b>
    </td>
    <td align="center">
      <img src="https://raw.githubusercontent.com/Agents365-ai/images_payment/main/qrcode/buymeacoffee.png" width="180" alt="Buy Me a Coffee">
      <br>
      <b>Buy Me a Coffee</b>
    </td>
  </tr>
</table>

## Author

**Agents365-ai**

- Bilibili: https://space.bilibili.com/441831884
- GitHub: https://github.com/Agents365-ai
