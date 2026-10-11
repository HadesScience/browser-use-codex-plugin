# Browser Use for Codex

将现有 Browser Use CLI 技能打包为 Codex 插件，通过 CDP 操作本机 Chrome，复用浏览器已有的登录状态。

这是由 [HadesScience](https://github.com/HadesScience) 维护的独立适配封装。浏览器控制能力来自上游 [Browser Use](https://github.com/browser-use/browser-use) 和 [Browser Harness](https://github.com/browser-use/browser-harness)，本仓库不包含它们的运行时。

## 工作方式

Codex 加载插件中的技能，通过终端将 Python 代码传给 `browser-use` 的标准输入。插件没有附加 MCP 服务，也不会自动安装、升级或重启 Browser Use。

包含 Windows PowerShell 调用说明，以及共享 daemon 和终端沙箱连接限制的处理规则。

## 前置条件

- 已安装支持 stdin Python 工作流的 `browser-use` CLI，并加入 PATH。本封装基于 `browser-use 0.13.8` 随附的技能整理。
- Chrome 已按[上游连接说明](https://github.com/browser-use/browser-harness/blob/main/install.md)启用远程调试。
- Codex 支持本地插件市场和 `codex plugin` 命令。

已有安装可直接复用；需要安装 Browser Use 时，请遵循[上游仓库文档](https://github.com/browser-use/browser-use)。

## 安装插件

```text
codex plugin marketplace add HadesScience/browser-use-codex-plugin
codex plugin add browser-use@browser-use-codex
codex plugin list --marketplace browser-use-codex --json
```

重新打开 Codex 或新建会话，使插件技能被加载。若之前已独立注册同名技能，确认插件安装成功后再移除重复项。

也可以克隆仓库后从本地注册：

```text
git clone https://github.com/HadesScience/browser-use-codex-plugin.git
codex plugin marketplace add ./browser-use-codex-plugin
codex plugin add browser-use@browser-use-codex
```

## 验证连接

在 PowerShell 中执行：

```powershell
@'
print(cdp("Browser.getVersion")["product"])
'@ | browser-use
```

成功时会输出当前 Chrome 版本。终端必须能够访问本机 Chrome 和共享 daemon；如果 Codex 沙箱无法连接 localhost，通过正常的工具权限流程使用本机执行环境。不要仅凭沙箱内的检查失败就删除运行数据或重启共享 daemon。

## 来源与许可

- 上游项目：[browser-use/browser-use](https://github.com/browser-use/browser-use)
- 技能来源：[Browser Use SKILL.md](https://github.com/browser-use/browser-use/blob/main/browser_use/skills/browser-use/SKILL.md)
- 连接实现与文档：[browser-use/browser-harness](https://github.com/browser-use/browser-harness)

技能保留了上游工作流，并增加 Codex / Windows 调用说明。本仓库按 MIT 许可证发布，保留 Browser Use 的原始版权声明，详见 [LICENSE](LICENSE)。
