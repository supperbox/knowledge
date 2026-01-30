# Git 关联远程 GitHub 仓库

本文介绍如何将本地新项目关联到远程 GitHub 仓库。

## 场景 1：本地已有项目，关联到远程空仓库

如果你在本地已经创建了项目文件夹并有一些代码，但还没有关联远程仓库：

1. **初始化本地仓库**（如果尚未初始化）：
   ```bash
   git init
   ```

2. **添加文件到暂存区**：
   ```bash
   git add .
   ```

3. **提交更改**：
   ```bash
   git commit -m "initial commit"
   ```

4. **创建并切换到主分支**（通常为 `main`）：
   ```bash
   git branch -M main
   ```

5. **关联远程仓库**：
   前往 GitHub 创建一个新仓库，获取其 URL（如 `https://github.com/username/repo-name.git`），然后运行：
   ```bash
   git remote add origin <远程仓库URL>
   ```

6. **推送到远程仓库**：
   ```bash
   git push -u origin main
   ```
   `-u` 参数会将本地 `main` 分支与远程 `origin/main` 分支关联，以后只需运行 `git push` 即可。

## 场景 2：从远程仓库克隆（推荐方式）

如果你还没有开始写代码，最简单的方法是先在 GitHub 上创建仓库，然后克隆到本地：

1. **克隆仓库**：
   ```bash
   git clone <远程仓库URL>
   ```

2. **进入目录并开始工作**：
   ```bash
   cd <项目名称>
   ```

## 常见问题

### 1. 修改远程仓库地址
如果你想更换关联的远程仓库地址：
```bash
git remote set-url origin <新的远程仓库URL>
```

### 2. 查看当前关联的远程仓库
```bash
git remote -v
```

### 3. SSH 与 HTTPS 选择
- **HTTPS**: 每次可能需要输入凭据（或使用凭据管理器）。
- **SSH**: 配置公钥后无需输入密码，更推荐。
