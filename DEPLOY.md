# GitHub Pages 部署指南

## 部署步骤

### 方法一：使用 GitHub Actions 自动部署（推荐）

1. **推送代码到 GitHub**
   ```bash
   git add .
   git commit -m "配置 GitHub Pages 部署"
   git push origin main
   ```

2. **在 GitHub 仓库中启用 Pages**
   - 进入您的 GitHub 仓库
   - 点击 `Settings` 选项卡
   - 在左侧菜单中找到 `Pages`
   - 在 `Source` 部分选择 `GitHub Actions`
   - 保存设置

3. **更新 package.json 中的 homepage**
   - 将 `package.json` 中的 `your-username` 替换为您的 GitHub 用户名
   - 格式：`https://your-username.github.io/web-quote`

4. **自动部署**
   - 每次推送到 `main` 或 `master` 分支时，GitHub Actions 会自动构建和部署
   - 在仓库的 `Actions` 选项卡中可以查看部署状态

### 方法二：手动部署

1. **安装依赖**
   ```bash
   npm install
   ```

2. **手动部署**
   ```bash
   npm run deploy
   ```

## 重要配置说明

### 1. gitignore 配置 ✅
- `node_modules/` 已正确忽略
- `dist/` 构建产物已忽略
- 其他临时文件和缓存文件已忽略

### 2. Vite 配置
- `base` 路径已配置为 `/web-quote/`（用于 GitHub Pages）
- 生产环境和开发环境路径分离

### 3. package.json 配置
- 添加了 `predeploy` 和 `deploy` 脚本
- 添加了 `gh-pages` 依赖用于手动部署

## 注意事项

1. **分支名称**：确保您的主分支名称是 `main` 或 `master`

2. **仓库名称**：如果仓库名称不是 `web-quote`，需要修改：
   - `vite.config.js` 中的 `base` 路径
   - `package.json` 中的 `homepage` 地址

3. **首次部署**：可能需要等待几分钟才能访问

4. **访问地址**：部署成功后，访问地址为：
   `https://your-username.github.io/web-quote`

## 故障排除

1. **构建失败**：检查 GitHub Actions 日志
2. **页面无法访问**：确保 GitHub Pages 设置正确
3. **资源加载失败**：检查 `base` 路径配置

## 本地测试

构建前可以本地测试：
```bash
npm run build
npm run preview
``` 