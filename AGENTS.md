# AGENTS.md · 项目规则

> 📌 **文档基线**：2026-10-17（commit `5c2f38f`）完成四件套重写
> **更新文档/代码后，请更新此行**（日期 + 新 commit hash），并在 CHANGELOG 追加版本

## 技术栈

- **纯静态单文件 HTML**（`index.html`，约 32 KB），无框架、无构建、无外部依赖
- CSS 内联在 `<style>`，使用 CSS 变量管理配色（`:root` 中 15 个变量）
- 目标渲染环境：Chromium / Safari / Firefox 现代版本；无 polyfill
- 验证工具：`chromium --headless=new` 截图 + PIL 裁剪（无需装 Playwright）

## 硬约束（改这个仓库必须守）

1. **单文件自包含**：不得引入 CDN、外部字体、外链 CSS/JS。报告要能在离线环境打开。
2. **深色优先**：当前主题为深色，新增元素必须用 `:root` 变量，禁止写死 `#fff` 之类浅色。
3. **证据分级不可删**：每条关键结论必须保留 `CONFIRMED` / 经验 / 推断 标注，避免把推断当事实。
4. **来源可点击**：新增断言必须附官方或权威媒体链接，禁用二手标题代替原文。
5. **中文正文**：正文、注释、提交信息用中文；文件名用中文（`index.html` 除外，保证 URL 友好）。

## 关键坑（越具体越好）

- **深色模式下散落的内联样式**：正文里的 `style="background:#fdf5e2"` 这类硬编码浅色，改 `:root` 变量时不会跟着变，会在深底上刺眼。改主题后必须全局搜一遍硬编码色值。
- **`grep` 工具在部分沙箱会 EACCES**：改用 shell 的 `grep -n -E` 兜底，不要误判为文件损坏。
- **中文文件名 ≠ 好 URL**：报告本体命名为 `index.html`，中文标题放页面内 `<h1>`。中文文件名在 GitHub Pages 会被 URL 编码，分享链接难看。
- **不要用二手标题代替原文**：媒体标题常把「加强关键能力考查」简写成「取消考试」这类失真表述。政策一律回到 `moe.gov.cn` 原文核对。
- **2026 是改革收官年，不是完成年**：29 省已新高考，西藏新疆仍「3+1」。写结论时不要说成「全国统一新模式」。

## 约定

- 提交信息格式：`类型: 简短描述`（`docs:` / `feat:` / `fix:` / `chore:`）
- 一个提交只做一件事；文档与内容改动分开提交
- 新增章节编号沿用「01—08」两位数格式，保持目录一致

## 常用命令

```bash
# 本地预览
python3 -m http.server 8000

# 渲染验证（无头截图）
chromium --headless=new --disable-gpu --no-sandbox \
  --window-size=1000,11000 --screenshot=check.png index.html

# 检查深色主题残留浅色
grep -n -E "#fff|#fbfcfe|#f8fafd|#eef" index.html

# 查看基线之后改了什么
git log --oneline <基线hash>..HEAD
```

## 详细规则（按需 @引用）

当前规模未超阈值（README < 150 行、AGENTS < 150 词核心约束、DEVELOPMENT < 200 行），暂不拆 `rules/`。
超过阈值时：AGENTS 拆 `rules/技术栈.md`、`rules/常见坑.md`，主文件保留核心约束 + @引用列表。
