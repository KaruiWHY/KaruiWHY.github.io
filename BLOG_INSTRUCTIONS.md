# 博客文章上传与编辑说明

本博客使用 **Markdown + LaTeX** 写作文章，并通过 `posts/posts.json` 管理文章列表。下面步骤假设你已经将本仓库克隆到本地，并在根目录下操作（即 `e:\KaruiWHY.github.io`）。

## 1. 本地预览方式

由于浏览器在 `file://` 协议下会阻止 `fetch` 读取本地文件，**请使用本地静态服务器** 来预览博客页面，例如：

```bash
cd e:\KaruiWHY.github.io
python -m http.server 8000
```

然后在浏览器中访问：

```text
http://localhost:8000/blog.html
```

## 2. 文章文件结构约定

- 所有文章的 Markdown 文件统一放在 `posts/` 目录下，例如：
  - `posts/first-note.md`
  - `posts/2025-01-01-my-first-post.md`
- 文章的元数据（标题、日期、标签等）集中保存在：
  - `posts/posts.json`

浏览器打开 `blog.html` 时，会按以下流程加载文章：

1. 先 `fetch('posts/posts.json')` 获取文章列表。
2. 对于每篇文章，再通过 `fetch(mdPath)` 读取对应的 Markdown 文件。
3. 使用 `marked` 将 Markdown 渲染为 HTML。
4. 使用 `KaTeX` 渲染 Markdown 中的数学公式。

## 3. posts/posts.json 格式

`posts/posts.json` 的基本结构如下：

```json
{
  "posts": [
    {
      "id": "first-note",
      "title": "Getting started with this blog",
      "date": "2025-01-01",
      "excerpt": "A short note about how this blog is organized, how Markdown and LaTeX are rendered, and how to add new posts.",
      "mdPath": "posts/first-note.md",
      "tags": ["meta", "markdown", "latex"]
    }
  ]
}
```

每篇文章的字段说明：

- `id`：文章的唯一标识符（推荐使用短横线分隔的小写英文），仅前端使用，不会直接展示。
- `title`：文章标题，展示在博客列表中。
- `date`：日期字符串，例如 `2025-01-01`。
- `excerpt`：可选，文章摘要。若省略，则会自动用 Markdown 内容前几百个字符生成预览。
- `mdPath`：**相对于站点根目录的 Markdown 文件路径**，例如 `posts/my-first-post.md`。
- `tags`：可选，字符串数组，会在文章卡片中展示为小标签。

> 注意：`posts` 数组的顺序会直接影响页面上的显示顺序。你可以将最新的文章放在数组最前面。

## 4. 新建一篇文章的完整流程

假设要新建一篇 ID 为 `my-first-post` 的文章：

### 步骤 1：创建 Markdown 文件

在 `posts/` 目录下新建文件 `posts/my-first-post.md`，示例内容：

```markdown
## My First Post

This is my *first* post on this site. I can write in **Markdown** and also include equations like \( e^{i\pi} + 1 = 0 \).

Here is a block equation:

$$
\\int_{0}^{1} x^2 \\, dx = \\frac{1}{3}
$$
```

说明：

- 使用普通 Markdown 语法写正文。
- 行内公式推荐使用 `\( ... \)` 或 `$...$`。
- 块级公式推荐使用 `$$ ... $$` 或 `\[ ... \]`。

### 步骤 2：在 posts/posts.json 中注册文章

打开 `posts/posts.json`，在 `posts` 数组中追加一个对象，例如：

```json
{
  "id": "my-first-post",
  "title": "My First Post",
  "date": "2025-02-01",
  "excerpt": "My first note written in Markdown with LaTeX equations.",
  "mdPath": "posts/my-first-post.md",
  "tags": ["life", "note"]
}
```

确保：

- JSON 语法正确（逗号不要多或少）。
- `mdPath` 与实际文件路径一致。

### 步骤 3：本地预览与检查

1. 启动或重新启动本地静态服务器（例如 `python -m http.server 8000`）。
2. 在浏览器中打开 `http://localhost:8000/blog.html`。
3. 检查：
   - 新文章是否出现在列表中。
   - 「Read more」按钮是否能展开全文。
   - Markdown 渲染是否正常（标题、列表、代码块等）。
   - LaTeX 公式是否由 KaTeX 正确渲染。

## 5. LaTeX 公式支持说明

本博客通过 KaTeX 的 `auto-render` 脚本支持多种定界符，你可以使用：

- 行内公式：
  - `\( ... \)` 或 `$...$`
- 块级公式：
  - `$$ ... $$` 或 `\[ ... \]`

建议：

- 避免在普通文本中频繁使用单独的 `$`，以免和公式定界冲突。
- 复杂公式建议使用 `$$ ... $$` 块级形式，排版更清晰。

## 6. 将更改推送到 GitHub Pages

在本地通过浏览器确认页面效果后：

```bash
git status
git add posts/posts.json posts/my-first-post.md
git commit -m "Add new blog post: my-first-post"
git push
```

当 GitHub Pages 部署完成后，即可通过 `https://karuiwhy.github.io/blog.html` 在线访问最新内容。

