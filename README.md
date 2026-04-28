# cocos-ai-mcp

让 Claude / Codex / Cursor 等 AI 端到端开发 Cocos Creator 3.8 游戏。

42 个 `cocos.*` MCP 工具 + 一份 Skill，把 AI 和 Cocos 编辑器打通：场景搭建、节点 / 组件操作、脚本生成、资源生成（GPT 图像 + 透明底处理）、预览截图自我迭代。从一句话需求到能跑的游戏，AI 可以无人工参与地完成。

## 一句话安装

```bash
curl -fsSL https://github.com/chenShengBiao/cocos-ai-mcp/releases/latest/download/install.sh \
  | bash -s -- ~/my-game
```

目标目录可以是空的（自动 scaffold 一个 Cocos 3.8 项目），也可以是已有的 Cocos 项目。

装完会自动：

- 解压 release 到 `~/.cocos-ai-mcp/`
- symlink 进目标项目的 `extensions/cocos-ai-mcp/`
- Skill 装到 `~/.claude/skills/cocos-dev/`
- 注册 MCP server 到 `~/.claude.json`（检测到 `claude` CLI 时）

## 装完之后

1. Cocos Creator 3.8+ 打开目标目录（Dashboard → Add Project）
2. 顶部菜单 `Extension → Cocos AI 助手 → 启动`，面板状态变 `running → http://127.0.0.1:3000/mcp`
3. 终端里 `cd ~/my-game && claude`，AI 自动看到 42 个 `cocos.*` 工具

## 系统要求

- macOS / Linux / WSL
- Cocos Creator 3.8.0+
- Node.js 20+
- Google Chrome（预览截图工具用）

## 工具家族（42 个 / 12 个家族）

- **scene / node / component** — 场景、节点、组件全套增删改查
- **asset / script / prefab** — 资源管理、TS 脚本生成、预制体
- **tween / physics** — 动画、物理
- **preview** — 启动预览、截图（puppeteer + 系统 Chrome 浏览器池）
- **console / editor / debug** — 编辑器日志、IPC 透传、反射健康检查

## 图像生成

支持任何 OpenAI 兼容 API（含中转）。在 Cocos 面板 `图像生成 API` 卡片填 base URL / model / key 即可。本地用 sharp 色键去背，吐透明底 sprite。

源码里不留默认 key — 全部从面板配置，开箱即用。

## 升级

重跑同一条 install 命令，`~/.cocos-ai-mcp/` 会被原子替换（带回滚），不影响目标项目里的 symlink。

## 反馈

直接提 Issue 到本仓库。

---

构建说明：本仓只发布二进制（esbuild minify 后的 bundle + Skill markdown + install scripts），不含 TypeScript 源码。
