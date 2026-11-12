# Dark++ for Zed

将原来的 [Dark++ VS Code 主题](https://github.com/codetin/DarkPlusPlus)移植到 Zed，保留深灰背景、亮绿色字符串、青色数字和紫红色类型名的配色风格。

## 安装

### 方式一：本地主题（推荐）

在本仓库根目录运行（macOS / Linux）：

```sh
mkdir -p ~/.config/zed/themes
cp themes/darkplusplus.json ~/.config/zed/themes/darkplusplus.json
```

Windows 用户将 `themes/darkplusplus.json` 放入 `%USERPROFILE%\AppData\Roaming\Zed\themes\`。

重新启动 Zed，打开命令面板，执行 `theme selector: toggle`，选择 **Dark++**。

也可以在 Zed 的 `settings.json` 中设置：

```json
{
  "theme": "Dark++"
}
```

### 方式二：开发扩展

1. 打开 Zed 的扩展页面（命令面板执行 `zed: extensions`）。
2. 点击 **Install Dev Extension**，选择本仓库根目录（包含 `extension.toml` 的目录，而非 `darkplusplus/`）。
3. 在主题选择器中选择 **Dark++**。

这是纯主题扩展，不包含 Rust / WebAssembly 代码，也不需要构建 VSIX。以上均为本地安装方式，不代表主题已经发布到 Zed 扩展商店。

## 配色

| 元素 | 颜色 |
| --- | --- |
| 编辑器背景 / 前景 | `#1E1E1E` / `#D4D4D4` |
| 标题栏 | `#004583` |
| 选区 | `#A5005B` |
| 字符串 | `#95FB79` |
| 数字 | `#9FFCEA` |
| 函数 / 方法 | `#FCCA5E` |
| 类型 / 构造函数 | `#D802BB` |
| 内置类型 | `#569CD6` |
| 变量 | `#9CDCFE` |
| 参数 | `#4EC9B0`，斜体 |
| 属性 | `#FFF3CD` |
| 命名空间 | `#00CCFF` |
| 关键字 | `#F5BA78` |
| 运算符 | `#EE68AF`，粗体 |
| 注释 | `#6A9955`，斜体 |

同时配置了面板、标签页、行号、缩进线、搜索匹配、诊断、Git 状态和终端 ANSI 配色。原主题未指定的界面颜色按 VS Code Dark+ 的风格补齐。

## C# XML 文档注释

主题为 `/// <summary>...</summary>` 这类 XML 文档注释提供 `comment.doc.xml` 样式（`#6A9955`，斜体）。Roslyn 会将正文、标签、属性、分隔符等报告为独立的 `xmlDocComment*` 语义 token；仅在主题中定义颜色不会自动建立映射。

使用 Roslyn 语义高亮时，将 [`zed-csharp-settings.json`](zed-csharp-settings.json) 中的 `global_lsp_settings.semantic_token_rules` 合并到 Zed 用户设置的同名数组中。不要用该文件覆盖整个用户设置，也不要重复添加 `global_lsp_settings` 字段。

这 10 条规则覆盖 Roslyn 的 XML 文档注释 token，支持 `semantic_tokens: "full"` 和 `"combined"`，不需要关闭语义高亮。映射只匹配 `xmlDocComment*` 类型，不改变普通 HTML/XML 的标签颜色；若切换到其他主题，会依次回退到该主题的 `comment.doc` 或 `comment` 样式。

主题扩展不能自动修改用户的语义 token 设置，因此首次安装时需同时合并此配置。更新本地主题后也需重新复制 `themes/darkplusplus.json` 到 Zed 用户主题目录。

Token 名称依据 [Roslyn 的 CustomLspSemanticTokenNames](https://github.com/dotnet/roslyn/blob/main/src/LanguageServer/Protocol/Handler/SemanticTokens/CustomLspSemanticTokenNames.cs)。

## 移植说明

- 原 VS Code 主题位于 `darkplusplus/themes/dark++-color-theme.json`，保留不变。Zed 使用根目录下的 `themes/darkplusplus.json`，不能直接加载 VS Code 的主题 JSON 或 `.vsix` 文件。
- VS Code 的 TextMate scope / semantic token 与 Zed 的语法捕获名称并不一一对应。函数、属性、变量等优先采用原主题的语义配色；类型统一使用原主题的紫红色，以便在未区分类与类型的语言中保留主要视觉特征。
- 各语言实际使用的捕获名称由 Zed 或相应语言扩展决定。类与结构体的细分、Python 浮点数的独立颜色、docstring、装饰器及部分语言专属规则，不能仅靠主题保证与 VS Code 完全一致。
- 原主题的活动标签页顶部黄线、当前行边框和选区边框没有在所用 Zed theme schema 中找到直接对应字段。这里用选中边框 / 聚焦窗格边框的金黄色和当前行背景近似表达，不声称逐像素复刻。
- 原 VS Code 截图及历史版本仍保留在 `darkplusplus/` 中，不作为 Zed 效果预览。

## 项目结构

```text
extension.toml              Zed 扩展清单
themes/darkplusplus.json     Zed 主题
zed-csharp-settings.json     C# XML 文档注释的语义 token 映射
darkplusplus/               原 VS Code 主题、说明、截图及历史安装包
```

## License

[MIT](LICENSE)
