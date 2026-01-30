# Git 常用命令指南

本文汇总了日常开发中最常用的 Git 命令，按场景分类以便快速查阅。

## 1. 基础配置

设置用户信息，这些信息将包含在你的提交中。

```bash
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"
git config --list # 查看当前配置
```

## 2. 获取与创建项目

```bash
git init                   # 在当前目录初始化本地仓库
git clone <url>            # 克隆远程仓库到本地
```

## 3. 基本操作（提交/暂存）

```bash
git status                 # 查看工作区状态
git add <file>             # 将文件添加到暂存区
git add .                  # 将所有改动添加到暂存区
git commit -m "提交信息"    # 提交暂存区到本地仓库
git commit -am "提交信息"   # 添加并提交（跳过 git add，仅限已跟踪文件）
```

## 4. 分支管理

```bash
git branch                 # 列出本地分支
git branch <name>          # 创建新分支
git checkout <name>        # 切换到分支
git checkout -b <name>     # 创建并切换到新分支
git merge <name>           # 将指定分支合并到当前分支
git branch -d <name>       # 删除分支
```

## 5. 远程操作

```bash
git remote -v              # 查看远程仓库信息
git pull                   # 拉取远程更改并合并到当前分支
git push                   # 将本地提交推送到远程
git push origin <branch>   # 推送特定分支到远程
git fetch                  # 从远程获取代码，但不自动合并
```

## 6. 撤销与检查历史

```bash
git log                    # 查看提交历史
git log --oneline          # 简洁显示历史
git diff                   # 查看工作区与暂存区的差异
git reset HEAD <file>      # 将文件移出暂存区，但保留工作区改动
git checkout -- <file>     # 丢弃工作区的改动（恢复到最近一次提交状态）
```

## 7. 暂存更改 (Stash)

当需要切换分支但当前工作还没准备好提交时使用。

```bash
git stash                  # 暂存当前更改
git stash pop              # 恢复最近一次暂存的更改
git stash list             # 查看暂存列表
```
