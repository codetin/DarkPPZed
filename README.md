# Dark++ for Zed

A Zed port of [Dark++ for VS Code](https://github.com/codetin/DarkPlusPlus), featuring a charcoal background, bright green strings, cyan numbers, magenta types, and a pink selection background.

[中文说明](README.zh-CN.md)

## Installation

This repository provides a local theme and a theme-only development extension. A GitHub release does **not** mean the extension has been accepted into the Zed extension registry; registry publication requires a separate review.

### Local theme

From the repository root (macOS / Linux):

```sh
mkdir -p ~/.config/zed/themes
cp themes/darkplusplus.json ~/.config/zed/themes/darkplusplus.json
```

On Windows, copy `themes/darkplusplus.json` to `%USERPROFILE%\AppData\Roaming\Zed\themes\`.

Restart Zed, run `theme selector: toggle` from the command palette, and select **Dark++**. Alternatively, set `"theme": "Dark++"` in your Zed settings.

### Development extension

1. Open the Extensions page with `zed: extensions`.
2. Click **Install Dev Extension** and select the directory containing `extension.toml` (the repository root or the extracted release directory).
3. Select **Dark++** in the theme selector.

The extension ID is `darkplusplus-theme`; the theme name remains **Dark++**. This is a theme-only extension with no Rust code, WebAssembly, or language servers.

## Palette

| Element | Color |
| --- | --- |
| Editor background / foreground | `#1E1E1E` / `#D4D4D4` |
| Title bar | `#004583` |
| Selection | `#A5005B` |
| Strings | `#95FB79` |
| Numbers | `#9FFCEA` |
| Functions / methods | `#FCCA5E` |
| Types / constructors | `#D802BB` |
| Built-in types | `#569CD6` |
| Variables | `#9CDCFE` |
| Parameters | `#4EC9B0`, italic |
| Properties | `#FFF3CD` |
| Namespaces | `#00CCFF` |
| Keywords | `#F5BA78` |
| Operators | `#EE68AF`, bold |
| Comments / XML documentation | `#6A9955`, italic |

The theme also styles panels, tabs, line numbers, indent guides, search matches, diagnostics, Git status, and terminal ANSI colors. UI colors not specified by the original theme follow the VS Code Dark+ style.

## C# XML documentation comments

Roslyn reports `/// <summary>...</summary>` using custom `xmlDocComment*` semantic tokens. Defining a theme color alone does not map those tokens to that color.

For Roslyn semantic highlighting, manually merge the rules from [`zed-csharp-settings.json`](zed-csharp-settings.json) into `global_lsp_settings.semantic_token_rules` in your Zed user settings. Preserve any existing rules and settings; do not replace your entire settings file or add a duplicate `global_lsp_settings` key.

The ten rules cover documentation text, XML tags, attributes, delimiters, entities, CDATA, comments, and processing instructions. They map to `comment.doc.xml`, falling back to `comment.doc` and `comment` for other themes, and work with both `semantic_tokens: "full"` and `"combined"`. They do not change regular HTML/XML highlighting.

This settings file is an optional configuration example, **not** a language extension. Installing the theme does not automatically change your settings or install these mappings. Token names follow [Roslyn's definitions](https://github.com/dotnet/roslyn/blob/main/src/LanguageServer/Protocol/Handler/SemanticTokens/CustomLspSemanticTokenNames.cs).

## Porting notes

- This repository contains only the Zed theme and its supporting configuration and documentation. The original VS Code theme, historical packages, and screenshots are available in the [original repository](https://github.com/codetin/DarkPlusPlus).
- TextMate scopes, semantic tokens, and Zed syntax captures do not map one-to-one. Functions, properties, and variables primarily follow the original semantic colors; generic types use magenta to retain the original class styling.
- Language-specific distinctions, including Python floats and docstrings, depend on the captures supplied by Zed or the language extension. Pixel-identical highlighting is not guaranteed.
- The original active-tab top border, active-line border, and selection border have no direct equivalents in the theme schema used here. Gold selected/focused-pane borders and an active-line background provide approximations.


## Publishing

See [PUBLISHING.md](PUBLISHING.md) for registry submission steps. The Zed registry builds and publishes the extension after its index pull request is reviewed and merged; it does not accept a VSIX upload.

## License

[MIT](LICENSE)
