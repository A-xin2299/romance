# 部署文档 - GitHub Pages + 自定义域名

## 前置条件

- 已创建 GitHub 仓库（如 `yourname/romance`）
- 项目代码已推送到仓库的 `main` 或 `master` 分支
- 已有自定义域名（如 `xinyu168.cn`）

---

## 第一步：本地构建

在项目根目录执行：

```bash
pnpm install
pnpm build
```

构建产物在 `dist/` 目录下，包含 `index.html`、`assets/` 等静态文件。

---

## 第二步：创建 gh-pages 分支

```bash
# 确保当前在 main/master 分支且工作区干净
git checkout --orphan gh-pages

# 删除所有文件，只保留 dist/ 内容
git rm -rf .

# 将 dist/ 下的所有文件移动到根目录
cp -r dist/* .

# 创建 CNAME 文件（绑定自定义域名用）
echo "xinyu168.cn" > CNAME

# 提交并推送
git add -A
git commit -m "Deploy to GitHub Pages"
git push origin gh-pages
```

> 注意：`xinyu168.cn` 替换为你的实际域名。

---

## 第三步：开启 GitHub Pages

1. 打开仓库 → **Settings** → **Pages**（左侧菜单）
2. **Source** 选择 `Deploy from a branch`
3. **Branch** 选择 `gh-pages`，目录选 `/ (root)`
4. 点击 **Save**

等待 1~3 分钟，GitHub Pages 会自动部署，页面地址为：
`https://yourname.github.io/romance/`

---

## 第四步：配置 DNS 解析

在你的域名服务商处（阿里云/腾讯云等），添加以下 DNS 记录：

| 类型 | 主机记录 | 记录值 | 说明 |
|------|---------|--------|------|
| CNAME | www | `yourname.github.io` | 绑定 www 子域名 |
| A | @ | `185.199.108.153` | 绑定根域名 |
| A | @ | `185.199.109.153` | 绑定根域名 |
| A | @ | `185.199.110.153` | 绑定根域名 |
| A | @ | `185.199.111.153` | 绑定根域名 |

> `yourname` 替换为你的 GitHub 用户名。

---

## 第五步：确认 GitHub Pages 自定义域名

1. 回到仓库 → **Settings** → **Pages**
2. 在 **Custom domain** 输入框中填写你的域名：`xinyu168.cn`
3. 点击 **Save**
4. 勾选 **Enforce HTTPS**（等待证书生效后勾选）

---

## 第六步：验证

- 浏览器访问 `https://xinyu168.cn` 确认页面正常显示
- 检查地图、二维码、轮播等功能是否正常

---

## 后续更新流程

每次修改代码后，执行以下步骤即可更新线上页面：

```bash
# 1. 在 main/master 分支构建
git checkout main
pnpm build

# 2. 切换到 gh-pages 分支，替换文件
git checkout gh-pages
git rm -rf assets index.html  # 清理旧文件
cp -r ../main/dist/* .        # 复制新文件（假设 main 分支在上级目录）

# 3. 提交并推送
git add -A
git commit -m "Update"
git push origin gh-pages
```

或者更简单的做法：使用 GitHub Actions 自动部署。
