# Git 命令详解

> 从零开始学习 Git 常用命令，配有大量实例

## 目录
- [安装和配置](#安装和配置)
- [创建仓库](#创建仓库)
- [基本操作](#基本操作)
- [分支管理](#分支管理)
- [远程操作](#远程操作)
- [高级操作](#高级操作)
- [实战案例](#实战案例)

---

## 安装和配置

### 安装 Git

**Windows**:
```bash
# 下载安装包
https://git-scm.com/download/win

# 或使用包管理器
winget install Git.Git
```

**Mac**:
```bash
brew install git
```

**Linux**:
```bash
# Ubuntu/Debian
sudo apt-get install git

# CentOS/Fedora
sudo yum install git
```

### 首次配置

```bash
# 配置用户名（会显示在 commit 记录中）
git config --global user.name "Chang Yuanhang"

# 配置邮箱
git config --global user.email "cyhang@mail.ustc.edu.cn"

# 配置默认编辑器
git config --global core.editor "code"  # VS Code
git config --global core.editor "vim"   # Vim

# 配置默认分支名为 main
git config --global init.defaultBranch main

# 查看配置
git config --list

# 查看特定配置
git config user.name
```

### 配置文件位置

```bash
# 全局配置（对所有仓库生效）
~/.gitconfig

# 本地配置（仅对当前仓库生效）
.git/config

# 查看配置来源
git config --list --show-origin
```

---

## 创建仓库

### 方法 1：从零开始

```bash
# 创建新目录
mkdir my-project
cd my-project

# 初始化 Git 仓库
git init

# 查看状态
git status
```

**输出示例**：
```
Initialized empty Git repository in /path/to/my-project/.git/
```

### 方法 2：克隆现有仓库

```bash
# 克隆远程仓库
git clone https://github.com/username/repository.git

# 克隆到指定目录
git clone https://github.com/username/repository.git my-folder

# 克隆指定分支
git clone -b develop https://github.com/username/repository.git
```

---

## 基本操作

### 1. 查看状态

```bash
# 查看工作区状态
git status

# 简洁输出
git status -s
```

**输出示例**：
```
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  modified:   README.md

Untracked files:
  new-file.py
```

**状态解读**：
- `Untracked`: 新文件，Git 还没追踪
- `Modified`: 已修改但未暂存
- `Staged`: 已暂存，等待提交

### 2. 添加文件到暂存区

```bash
# 添加单个文件
git add README.md

# 添加所有文件
git add .

# 添加所有 Python 文件
git add *.py

# 添加指定目录
git add src/

# 交互式添加（可以选择部分内容）
git add -p
```

### 3. 提交更改

```bash
# 提交暂存区的文件
git commit -m "添加用户登录功能"

# 添加并提交（跳过 git add）
git commit -am "修复登录 bug"

# 多行提交信息
git commit -m "添加支付功能" -m "集成微信支付和支付宝"

# 使用编辑器编写详细提交信息
git commit
```

**好的 Commit Message 规范**：
```bash
# 格式：<type>: <subject>

# 类型（type）：
feat:     新功能
fix:      修复 bug
docs:     文档更新
style:    代码格式（不影响代码运行）
refactor: 重构
test:     测试
chore:    构建过程或辅助工具的变动

# 示例：
git commit -m "feat: 添加用户注册功能"
git commit -m "fix: 修复购物车计算错误"
git commit -m "docs: 更新 API 文档"
```

### 4. 查看历史

```bash
# 查看提交历史
git log

# 单行显示
git log --oneline

# 图形化显示分支
git log --oneline --graph --all

# 查看最近 3 条
git log -3

# 查看某个文件的历史
git log README.md

# 查看详细改动
git log -p

# 查看统计信息
git log --stat

# 搜索提交信息
git log --grep="登录"

# 查看某人的提交
git log --author="Chang"
```

**输出示例**：
```
* a1b2c3d (HEAD -> main) feat: 添加用户注册
* e4f5g6h fix: 修复登录 bug
* i7j8k9l docs: 更新 README
```

### 5. 查看差异

```bash
# 查看工作区 vs 暂存区的差异
git diff

# 查看暂存区 vs 仓库的差异
git diff --staged

# 查看两个提交之间的差异
git diff commit1 commit2

# 查看某个文件的差异
git diff README.md

# 统计差异
git diff --stat
```

### 6. 撤销操作

#### 6.1 撤销工作区的修改

```bash
# 撤销单个文件的修改（恢复到最后一次 commit）
git checkout -- file.txt

# Git 2.23+ 新命令
git restore file.txt

# 撤销所有修改
git checkout -- .
git restore .
```

#### 6.2 撤销暂存区的文件

```bash
# 取消暂存（文件回到工作区）
git reset HEAD file.txt

# Git 2.23+ 新命令
git restore --staged file.txt
```

#### 6.3 撤销提交

```bash
# 撤销最后一次提交，保留更改在工作区
git reset --soft HEAD~1

# 撤销最后一次提交，保留更改在暂存区
git reset --mixed HEAD~1

# 撤销最后一次提交，完全删除更改（危险！）
git reset --hard HEAD~1

# 修改最后一次提交（添加遗漏的文件或修改 message）
git add forgotten-file.py
git commit --amend

# 只修改提交信息
git commit --amend -m "新的提交信息"
```

**⚠️ 警告**：
- `--hard` 会永久删除更改，使用前请确认
- 不要修改已经推送的提交（会影响团队成员）

### 7. 删除和移动文件

```bash
# 删除文件（同时删除工作区和暂存区）
git rm file.txt

# 只从暂存区删除，保留工作区文件
git rm --cached file.txt

# 移动/重命名文件
git mv old-name.txt new-name.txt

# 等价于：
mv old-name.txt new-name.txt
git rm old-name.txt
git add new-name.txt
```

---

## 分支管理

### 1. 创建和切换分支

```bash
# 查看所有分支
git branch

# 查看远程分支
git branch -r

# 查看所有分支（本地+远程）
git branch -a

# 创建分支
git branch feature/login

# 切换分支
git checkout feature/login

# 创建并切换分支（一步完成）
git checkout -b feature/login

# Git 2.23+ 新命令
git switch feature/login
git switch -c feature/login  # 创建并切换
```

### 2. 合并分支

```bash
# 切换到目标分支
git checkout main

# 合并指定分支到当前分支
git merge feature/login

# 不使用 Fast-forward 模式（保留分支历史）
git merge --no-ff feature/login -m "Merge feature/login"

# 合并时压缩成一个提交
git merge --squash feature/login
```

**合并冲突处理**：
```bash
# 1. 合并时遇到冲突
git merge feature/login
# Auto-merging file.txt
# CONFLICT (content): Merge conflict in file.txt

# 2. 查看冲突文件
git status

# 3. 手动编辑冲突文件
# <<<<<<< HEAD
# 当前分支的内容
# =======
# 要合并分支的内容
# >>>>>>> feature/login

# 4. 解决冲突后标记为已解决
git add file.txt

# 5. 完成合并
git commit -m "解决合并冲突"

# 取消合并
git merge --abort
```

### 3. 删除分支

```bash
# 删除已合并的分支
git branch -d feature/login

# 强制删除分支（即使未合并）
git branch -D feature/login

# 删除远程分支
git push origin --delete feature/login
```

### 4. 变基（Rebase）

```bash
# 将当前分支变基到 main
git rebase main

# 交互式变基（可以修改、合并、删除提交）
git rebase -i HEAD~3

# 继续变基
git rebase --continue

# 中止变基
git rebase --abort
```

**Merge vs Rebase**：
```
Merge（保留分支历史）:
main:     A---B---C-------M
                   \     /
feature:            D---E

Rebase（线性历史）:
main:     A---B---C
feature:              D'---E'
```

---

## 远程操作

### 1. 远程仓库管理

```bash
# 查看远程仓库
git remote -v

# 添加远程仓库
git remote add origin https://github.com/username/repo.git

# 修改远程仓库 URL
git remote set-url origin https://github.com/username/new-repo.git

# 删除远程仓库
git remote remove origin

# 重命名远程仓库
git remote rename origin upstream
```

### 2. 推送（Push）

```bash
# 推送到远程仓库
git push origin main

# 首次推送，设置上游分支
git push -u origin main

# 推送所有分支
git push origin --all

# 推送标签
git push origin --tags

# 强制推送（危险！会覆盖远程历史）
git push -f origin main
```

### 3. 拉取（Pull/Fetch）

```bash
# 拉取并合并
git pull origin main

# 等价于：
git fetch origin
git merge origin/main

# 使用 rebase 方式拉取
git pull --rebase origin main

# 只拉取不合并
git fetch origin

# 拉取所有远程分支
git fetch --all
```

**Pull vs Fetch**：
- `git pull` = `git fetch` + `git merge`
- `fetch` 只下载，不合并（更安全）
- `pull` 下载并自动合并

### 4. 跟踪远程分支

```bash
# 查看跟踪关系
git branch -vv

# 设置上游分支
git branch --set-upstream-to=origin/main main

# 推送并设置上游
git push -u origin feature/login

# 从远程分支创建本地分支
git checkout -b feature/login origin/feature/login
```

---

## 高级操作

### 1. 储藏（Stash）

```bash
# 储藏当前工作
git stash

# 储藏并添加说明
git stash save "工作进行到一半"

# 查看储藏列表
git stash list

# 应用最近的储藏
git stash apply

# 应用并删除储藏
git stash pop

# 应用指定的储藏
git stash apply stash@{1}

# 删除储藏
git stash drop stash@{0}

# 清空所有储藏
git stash clear

# 从储藏创建分支
git stash branch new-branch
```

**使用场景**：
```
场景：你在 feature 分支开发到一半，突然需要修复 main 分支的紧急 bug

1. git stash              # 储藏当前工作
2. git checkout main      # 切换到 main
3. 修复 bug 并提交
4. git checkout feature   # 切回 feature
5. git stash pop          # 恢复之前的工作
```

### 2. 标签（Tag）

```bash
# 查看所有标签
git tag

# 创建轻量标签
git tag v1.0

# 创建附注标签（推荐）
git tag -a v1.0 -m "版本 1.0 发布"

# 给特定提交打标签
git tag -a v0.9 commit_hash -m "版本 0.9"

# 查看标签信息
git show v1.0

# 推送标签到远程
git push origin v1.0
git push origin --tags  # 推送所有标签

# 删除标签
git tag -d v1.0                    # 删除本地标签
git push origin --delete v1.0     # 删除远程标签

# 检出标签
git checkout v1.0
```

### 3. Cherry-pick

```bash
# 挑选某个提交应用到当前分支
git cherry-pick commit_hash

# 挑选多个提交
git cherry-pick commit1 commit2

# 挑选但不自动提交
git cherry-pick -n commit_hash
```

**使用场景**：
```
场景：feature 分支有一个 bug 修复，main 分支也需要这个修复

main:     A---B---C
feature:  A---B---D---E (D 是 bug 修复)

git checkout main
git cherry-pick <D的hash>

结果：
main:     A---B---C---D'
```

### 4. 子模块（Submodule）

```bash
# 添加子模块
git submodule add https://github.com/user/repo.git path/to/submodule

# 克隆包含子模块的项目
git clone --recursive https://github.com/user/repo.git

# 初始化子模块
git submodule init
git submodule update

# 更新子模块
git submodule update --remote

# 删除子模块
git submodule deinit path/to/submodule
git rm path/to/submodule
```

### 5. 搜索和查找

```bash
# 在代码中搜索
git grep "function_name"

# 查找某行代码的修改历史
git blame file.txt

# 查找引入 bug 的提交（二分查找）
git bisect start
git bisect bad           # 当前版本有 bug
git bisect good v1.0     # v1.0 版本没有 bug
# Git 会自动检出中间版本，测试后标记
git bisect good/bad
# 重复直到找到问题提交
git bisect reset
```

### 6. 重写历史

```bash
# 修改最后一次提交
git commit --amend

# 交互式变基（修改多个提交）
git rebase -i HEAD~3

# 在交互界面可以：
pick    # 保留提交
reword  # 修改提交信息
edit    # 修改提交内容
squash  # 合并到上一个提交
drop    # 删除提交

# 过滤分支（批量修改历史，如删除敏感文件）
git filter-branch --tree-filter 'rm -f passwords.txt' HEAD
```

**⚠️ 警告**：重写已推送的历史会影响其他协作者！

---

## 实战案例

### 案例 1：创建新项目并推送到 GitHub

```bash
# 1. 创建项目目录
mkdir my-awesome-project
cd my-awesome-project

# 2. 初始化 Git
git init

# 3. 创建文件
echo "# My Awesome Project" > README.md
echo "print('Hello World')" > main.py

# 4. 添加并提交
git add .
git commit -m "feat: 初始化项目"

# 5. 连接远程仓库（先在 GitHub 创建仓库）
git remote add origin https://github.com/username/my-awesome-project.git

# 6. 推送
git push -u origin main
```

### 案例 2：修复 Bug 的完整流程

```bash
# 1. 从 main 分支创建 bugfix 分支
git checkout main
git pull origin main
git checkout -b bugfix/fix-login-error

# 2. 修改代码并测试

# 3. 提交修复
git add .
git commit -m "fix: 修复登录验证错误"

# 4. 推送分支
git push -u origin bugfix/fix-login-error

# 5. 在 GitHub 创建 Pull Request

# 6. 合并后删除分支
git checkout main
git pull origin main
git branch -d bugfix/fix-login-error
git push origin --delete bugfix/fix-login-error
```

### 案例 3：协作开发新功能

```bash
# A 同学：
git checkout -b feature/user-profile
# 开发代码...
git add .
git commit -m "feat: 添加用户资料页面"
git push -u origin feature/user-profile

# B 同学：
git fetch origin
git checkout -b feature/user-profile origin/feature/user-profile
# 继续开发...
git add .
git commit -m "feat: 完善用户资料编辑"
git pull --rebase origin feature/user-profile
git push origin feature/user-profile
```

### 案例 4：解决合并冲突

```bash
# 1. 尝试合并
git checkout main
git merge feature/new-feature
# CONFLICT (content): Merge conflict in app.py

# 2. 查看冲突文件
git status
# both modified:   app.py

# 3. 编辑 app.py，解决冲突
# 删除冲突标记 <<<<<<<, =======, >>>>>>>
# 保留需要的代码

# 4. 标记为已解决
git add app.py

# 5. 完成合并
git commit -m "Merge feature/new-feature, 解决冲突"
```

### 案例 5：回滚错误的提交

```bash
# 方法 1：revert（推荐，创建新提交撤销）
git revert commit_hash
git push origin main

# 方法 2：reset（重写历史，危险）
git reset --hard commit_hash
git push -f origin main  # 强制推送
```

---

## 常用命令速查表

| 功能 | 命令 |
|------|------|
| 初始化仓库 | `git init` |
| 克隆仓库 | `git clone <url>` |
| 查看状态 | `git status` |
| 添加文件 | `git add <file>` |
| 提交 | `git commit -m "message"` |
| 查看历史 | `git log --oneline` |
| 创建分支 | `git branch <name>` |
| 切换分支 | `git checkout <name>` |
| 合并分支 | `git merge <branch>` |
| 删除分支 | `git branch -d <name>` |
| 推送 | `git push origin <branch>` |
| 拉取 | `git pull origin <branch>` |
| 查看远程 | `git remote -v` |
| 储藏工作 | `git stash` |
| 恢复储藏 | `git stash pop` |
| 撤销修改 | `git checkout -- <file>` |
| 查看差异 | `git diff` |

---

## Git 工作流程图

```
工作区               暂存区              本地仓库            远程仓库
(Working)          (Staging)           (Local)            (Remote)

  编辑文件    →    git add    →    git commit    →    git push

  git checkout  ←   git reset  ←   git reset    ←    git pull
```

---

## 下一步

- [03-GitHub-CLI使用.md](./03-GitHub-CLI使用.md) - 学习 GitHub CLI 工具
- [04-Claude-Code最佳实践.md](./04-Claude-Code最佳实践.md) - 结合 Claude Code 开发

---

**提示**：
- 多练习才能熟练掌握
- 遇到问题先 `git status` 看状态
- 不确定的操作先在测试仓库尝试
- 善用 `git help <command>` 查看文档
