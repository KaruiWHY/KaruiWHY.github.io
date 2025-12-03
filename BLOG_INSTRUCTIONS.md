如何添加博客文章

有两种简单方式把文章加入到这个静态站点：

方法 A（推荐，适合多人或使用简单自动化）：在 `posts/posts.json` 中添加条目

- 打开 `posts/posts.json`，文件包含一个 JSON 对象，形如:

  {
    "posts": [
      { "title": "...", "date": "YYYY-MM-DD", "slug": "my-first-post", "excerpt": "短摘要", "content": "<p>HTML 正文</p>" },
      ...
    ]
  }

- 新增一个对象到 `posts` 数组。字段说明：
  - `title`：文章标题（字符串）。
  - `date`：发布日期（字符串，建议 ISO 格式 YYYY-MM-DD）。
  - `slug`：短标识（用于将来若想生成独立页面或链接）。
  - `excerpt`：在列表中显示的短摘要（可含少量 HTML）。
  - `content`：完整文章内容，当前实现直接将字符串插入为 HTML（可以包含段落、标题、内联图片标签等）。

- 保存后（若你用本地静态服务器）刷新 `blog.html` 即可看到新文章。

方法 B（把每篇文章做成单独 HTML 文件并在 `blog.html` 手动链接）

- 在 `posts/` 目录新建 `slug.html`，写入完整 HTML（或局部 HTML 片段），然后在 `blog.html` 中手动添加一个链接到该文件的条目。

注意：
- 直接用浏览器打开 `blog.html`（file://）时，脚本通过 fetch 加载 `posts/posts.json` 可能被浏览器阻止（跨域/本地文件限制）。推荐用下面的本地静态服务器方式之一来预览：

在 PowerShell 中（项目根目录）：

```powershell
# 方法 1: Python (如果已安装)
python -m http.server 8000
# 打开 http://localhost:8000/blog.html

# 方法 2: 使用 VS Code 的 Live Server 插件
# 在 VS Code 中右键 index.html -> Open with Live Server
```

安全提示：当前实现会把 `content` 字段直接作为 HTML 插入页面，因此请仅放入受信任的内容，避免不受信任的第三方 HTML/脚本以免 XSS 风险。

进阶建议：
- 如果你想使用 Markdown 写作，可以在本地把 Markdown 转成 HTML（例如用 pandoc、markdown-it 等），或在 `blog.html` 中加入一个轻量的 Markdown 渲染器并把 markdown 文件转换为字符串后渲染。该改动需要额外脚本。
- 若希望每篇文章有独立页面，我可以继续帮你：生成每篇的 `posts/slug.html` 文件并在 `blog.html` 自动创建链接。
