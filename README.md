# 子杰的博客

> 前端开发 / AI Agent / 技术随笔

---

## 🚀 本地运行

```bash
# 安装依赖
pnpm install

# 启动本地服务器
hexo server

# 访问 http://localhost:4000
```

## 📝 写文章

```bash
# 创建新文章
hexo new post "文章标题"

# 编辑文件：source/_posts/文章标题.md

# 生成静态文件
hexo generate

# 部署
git add .
git commit -m "new post: 文章标题"
git push
```

## 🌐 部署

博客已配置自动部署到 Vercel：

1. 在 GitHub 创建新仓库
2. 推送代码：
   ```bash
   git remote add origin https://github.com/你的用户名/你的仓库.git
   git push -u origin main
   ```
3. 在 [Vercel](https://vercel.com) Import 此仓库
4. 构建配置：
   - **Build Command:** `hexo generate`
   - **Output Directory:** `public`

---

**Powered by Hexo**
**Theme: Landscape**
