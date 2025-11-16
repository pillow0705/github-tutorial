# GitHub CLI (gh) 使用指南

> 使用命令行高效管理 GitHub 仓库、PR、Issue 等

## 目录
- [什么是 GitHub CLI](#什么是-github-cli)
- [安装和配置](#安装和配置)
- [仓库管理](#仓库管理)
- [Pull Request 管理](#pull-request-管理)
- [Issue 管理](#issue-管理)
- [其他功能](#其他功能)
- [实战案例](#实战案例)

---

## 什么是 GitHub CLI

**GitHub CLI (gh)** 是 GitHub 官方命令行工具，让你无需打开浏览器就能管理 GitHub。

### 为什么使用 gh？

**传统方式**：
```
1. 打开浏览器
2. 登录 GitHub
3. 找到仓库
4. 点击创建 Issue/PR
5. 填写表单
6. 提交
```

**使用 gh**：
```bash
# 一行命令搞定
gh issue create --title "Bug: 登录失败" --body "描述..."
```

### gh vs git

| 工具 | 作用 | 示例 |
|------|------|------|
| `git` | 版本控制 | `git commit`, `git push` |
| `gh` | GitHub 平台操作 | `gh pr create`, `gh issue list` |

两者**互补**，不是替代关系！

---

## 安装和配置

### 安装

**Windows**:
```bash
# 使用 winget
winget install GitHub.cli

# 使用 scoop
scoop install gh

# 下载安装包
https://cli.github.com/
```

**Mac**:
```bash
brew install gh
```

**Linux**:
```bash
# Ubuntu/Debian
sudo apt install gh

# Fedora/CentOS
sudo dnf install gh
```

### 验证安装

```bash
gh --version
# gh version 2.40.0 (2024-01-15)
```

### 认证登录

```bash
# 登录 GitHub
gh auth login

# 会提示选择：
# 1. GitHub.com 还是 GitHub Enterprise
# 2. HTTPS 还是 SSH
# 3. 认证方式（浏览器或 Token）
# 4. Git 协议偏好

# 查看登录状态
gh auth status

# 输出示例：
# github.com
#   ✓ Logged in to github.com as pillow0705
#   ✓ Git operations protocol: https
#   ✓ Token: ghp_****
```

### 退出登录

```bash
gh auth logout
```

---

## 仓库管理

### 1. 创建仓库

```bash
# 基本创建
gh repo create my-project

# 创建并初始化
gh repo create my-project --public --clone

# 完整选项
gh repo create my-project \
  --public \
  --description "项目描述" \
  --gitignore Python \
  --license MIT \
  --clone

# 从当前目录创建
cd existing-project
gh repo create --source=. --public --push
```

**选项说明**：
- `--public` / `--private`：公开/私有
- `--description`：项目描述
- `--gitignore`：添加 .gitignore 模板
- `--license`：选择开源协议
- `--clone`：创建后自动克隆到本地
- `--push`：推送现有代码

### 2. 查看仓库

```bash
# 查看当前仓库信息
gh repo view

# 查看指定仓库
gh repo view owner/repo

# 在浏览器打开
gh repo view --web

# 列出我的仓库
gh repo list

# 列出别人的仓库
gh repo list torvalds

# 限制数量
gh repo list --limit 10

# 按语言筛选
gh repo list --language Python
```

### 3. 克隆仓库

```bash
# 克隆自己的仓库
gh repo clone my-repo

# 克隆别人的仓库
gh repo clone owner/repo

# 等价于：
git clone https://github.com/owner/repo.git
```

### 4. Fork 仓库

```bash
# Fork 仓库
gh repo fork owner/repo

# Fork 并克隆
gh repo fork owner/repo --clone

# Fork 到组织
gh repo fork owner/repo --org my-org
```

### 5. 删除仓库

```bash
# 删除仓库（需要确认）
gh repo delete owner/repo

# 跳过确认
gh repo delete owner/repo --yes
```

### 6. 仓库设置

```bash
# 重命名仓库
gh repo rename new-name

# 修改默认分支
gh repo edit --default-branch main

# 启用/禁用功能
gh repo edit --enable-wiki=false
gh repo edit --enable-issues=true
```

---

## Pull Request 管理

### 1. 创建 PR

```bash
# 交互式创建
gh pr create

# 指定标题和内容
gh pr create --title "添加登录功能" --body "实现了用户登录"

# 从文件读取内容
gh pr create --title "新功能" --body-file description.md

# 指定审查者
gh pr create --reviewer username1,username2

# 指定标签
gh pr create --label bug,urgent

# 草稿 PR
gh pr create --draft

# 完整示例
gh pr create \
  --title "feat: 添加用户认证" \
  --body "实现 JWT 认证和权限控制" \
  --reviewer alice,bob \
  --label feature,backend \
  --assignee @me
```

### 2. 查看 PR

```bash
# 列出 PR
gh pr list

# 查看我创建的 PR
gh pr list --author @me

# 查看已合并的 PR
gh pr list --state merged

# 按标签筛选
gh pr list --label bug

# 查看 PR 详情
gh pr view 123

# 在浏览器查看
gh pr view 123 --web

# 查看 PR 差异
gh pr diff 123

# 查看 PR 审查状态
gh pr checks 123
```

### 3. 检出 PR

```bash
# 检出 PR 到本地
gh pr checkout 123

# 会自动创建分支并切换
```

### 4. 审查 PR

```bash
# 审查 PR
gh pr review 123

# 批准 PR
gh pr review 123 --approve

# 请求修改
gh pr review 123 --request-changes --body "需要添加测试"

# 评论
gh pr review 123 --comment --body "代码看起来不错"
```

### 5. 合并 PR

```bash
# 合并 PR
gh pr merge 123

# 指定合并方式
gh pr merge 123 --merge      # 普通合并
gh pr merge 123 --squash     # 压缩合并
gh pr merge 123 --rebase     # 变基合并

# 合并后删除分支
gh pr merge 123 --delete-branch

# 自动合并（等待 CI 通过）
gh pr merge 123 --auto
```

### 6. 关闭 PR

```bash
# 关闭 PR
gh pr close 123

# 重新打开
gh pr reopen 123
```

### 7. PR 模板

创建 `.github/pull_request_template.md`：

```markdown
## 变更说明
<!-- 描述你的修改 -->

## 变更类型
- [ ] Bug 修复
- [ ] 新功能
- [ ] 代码重构
- [ ] 文档更新

## 测试
- [ ] 已添加单元测试
- [ ] 已通过所有测试
- [ ] 已在本地验证

## 截图
<!-- 如有 UI 变更，请添加截图 -->

## 相关 Issue
Closes #issue_number
```

---

## Issue 管理

### 1. 创建 Issue

```bash
# 交互式创建
gh issue create

# 指定标题和内容
gh issue create --title "Bug: 无法登录" --body "点击登录按钮无反应"

# 指定标签和负责人
gh issue create \
  --title "添加暗黑模式" \
  --body "用户需要暗黑模式" \
  --label enhancement \
  --assignee @me

# 从模板创建
gh issue create --template bug_report.md
```

### 2. 查看 Issue

```bash
# 列出 Issue
gh issue list

# 查看已关闭的
gh issue list --state closed

# 按标签筛选
gh issue list --label bug

# 按负责人筛选
gh issue list --assignee @me

# 查看 Issue 详情
gh issue view 42

# 在浏览器查看
gh issue view 42 --web
```

### 3. 编辑 Issue

```bash
# 添加标签
gh issue edit 42 --add-label bug,urgent

# 删除标签
gh issue edit 42 --remove-label urgent

# 分配负责人
gh issue edit 42 --add-assignee alice

# 修改标题
gh issue edit 42 --title "新标题"
```

### 4. 关闭/重开 Issue

```bash
# 关闭 Issue
gh issue close 42

# 重新打开
gh issue reopen 42

# 关闭并添加评论
gh issue close 42 --comment "已修复"
```

### 5. 评论 Issue

```bash
# 添加评论
gh issue comment 42 --body "我来处理这个问题"

# 从文件读取
gh issue comment 42 --body-file comment.md
```

### 6. Issue 模板

创建 `.github/ISSUE_TEMPLATE/bug_report.md`：

```markdown
---
name: Bug Report
about: 报告一个 bug
title: '[BUG] '
labels: bug
assignees: ''
---

## Bug 描述
<!-- 清晰简洁地描述 bug -->

## 复现步骤
1. 进入 '...'
2. 点击 '...'
3. 滚动到 '...'
4. 看到错误

## 期望行为
<!-- 描述你期望发生什么 -->

## 实际行为
<!-- 描述实际发生了什么 -->

## 截图
<!-- 如果适用，添加截图 -->

## 环境
- OS: [e.g. Windows 11]
- 浏览器: [e.g. Chrome 120]
- 版本: [e.g. 1.0.0]

## 附加信息
<!-- 其他相关信息 -->
```

---

## 其他功能

### 1. Gist 管理

```bash
# 创建 Gist
gh gist create file.txt

# 创建私有 Gist
gh gist create file.txt --secret

# 创建多文件 Gist
gh gist create file1.txt file2.py

# 列出 Gist
gh gist list

# 查看 Gist
gh gist view <gist-id>

# 编辑 Gist
gh gist edit <gist-id>

# 删除 Gist
gh gist delete <gist-id>
```

### 2. Release 管理

```bash
# 创建 Release
gh release create v1.0.0

# 上传文件
gh release create v1.0.0 dist/*.zip

# 指定标题和说明
gh release create v1.0.0 \
  --title "Version 1.0.0" \
  --notes "首次正式发布"

# 从文件读取说明
gh release create v1.0.0 --notes-file CHANGELOG.md

# 列出 Release
gh release list

# 查看 Release
gh release view v1.0.0

# 下载 Release 资源
gh release download v1.0.0

# 删除 Release
gh release delete v1.0.0
```

### 3. Workflow 管理

```bash
# 列出 Workflow
gh workflow list

# 查看 Workflow 运行
gh run list

# 查看运行详情
gh run view <run-id>

# 查看日志
gh run view <run-id> --log

# 重新运行
gh run rerun <run-id>

# 取消运行
gh run cancel <run-id>

# 触发 Workflow
gh workflow run build.yml
```

### 4. 浏览和搜索

```bash
# 在浏览器打开仓库
gh browse

# 打开 Issue 页面
gh browse --issues

# 打开 PR 页面
gh browse --pulls

# 打开设置页面
gh browse --settings

# 搜索仓库
gh search repos "machine learning" --language Python

# 搜索 Issue
gh search issues "bug" --repo owner/repo

# 搜索代码
gh search code "function login" --repo owner/repo
```

### 5. 别名（Alias）

```bash
# 创建别名
gh alias set pv 'pr view'
gh alias set co 'pr checkout'

# 使用别名
gh pv 123
gh co 456

# 列出别名
gh alias list

# 删除别名
gh alias delete pv
```

---

## 实战案例

### 案例 1：完整的功能开发流程

```bash
# 1. Fork 并克隆项目
gh repo fork awesome-project/main --clone
cd main

# 2. 创建功能分支
git checkout -b feature/dark-mode

# 3. 开发功能...
git add .
git commit -m "feat: 添加暗黑模式"

# 4. 推送分支
git push -u origin feature/dark-mode

# 5. 创建 PR
gh pr create \
  --title "feat: 添加暗黑模式支持" \
  --body "实现了完整的暗黑模式，包括颜色主题切换和本地存储" \
  --label enhancement \
  --reviewer maintainer1

# 6. 查看 CI 状态
gh pr checks

# 7. 根据反馈修改
git add .
git commit -m "fix: 修复暗黑模式下的对比度问题"
git push

# 8. PR 被批准后合并
gh pr merge --squash --delete-branch
```

### 案例 2：Bug 修复流程

```bash
# 1. 创建 Issue
gh issue create \
  --title "Bug: 搜索功能返回错误结果" \
  --body "当搜索包含特殊字符时返回 500 错误" \
  --label bug

# 输出：https://github.com/owner/repo/issues/42

# 2. 创建修复分支
git checkout -b bugfix/search-special-chars

# 3. 修复并提交
git add .
git commit -m "fix: 修复搜索特殊字符的问题"
git push -u origin bugfix/search-special-chars

# 4. 创建 PR 并关联 Issue
gh pr create \
  --title "fix: 修复搜索特殊字符问题" \
  --body "修复了 #42 中描述的问题" \
  --label bug

# 5. 合并后自动关闭 Issue（通过 PR 中的 "fixes #42"）
gh pr merge --squash
```

### 案例 3：发布新版本

```bash
# 1. 更新版本号和 CHANGELOG
# 编辑 package.json, CHANGELOG.md 等

# 2. 提交版本更新
git add .
git commit -m "chore: 发布 v1.2.0"
git push

# 3. 创建 Tag
git tag -a v1.2.0 -m "版本 1.2.0"
git push origin v1.2.0

# 4. 创建 Release
gh release create v1.2.0 \
  --title "Version 1.2.0" \
  --notes-file CHANGELOG.md \
  dist/app-v1.2.0.zip

# 5. 查看 Release
gh release view v1.2.0 --web
```

### 案例 4：管理开源项目

```bash
# 查看待处理的 PR
gh pr list --label "needs-review"

# 批量处理 PR
for pr in $(gh pr list --json number --jq '.[].number'); do
  echo "审查 PR #$pr"
  gh pr view $pr
  gh pr review $pr --approve
done

# 查看最近的 Issue
gh issue list --limit 10

# 关闭过时的 Issue
gh issue list --label stale --json number --jq '.[].number' | \
  xargs -I {} gh issue close {} --comment "由于长时间无活动，自动关闭"

# 查看项目统计
gh api repos/:owner/:repo \
  --jq '{stars: .stargazers_count, forks: .forks_count, issues: .open_issues_count}'
```

---

## 常用命令速查表

### 仓库操作
```bash
gh repo create <name>          # 创建仓库
gh repo clone <repo>           # 克隆仓库
gh repo fork <repo>            # Fork 仓库
gh repo view                   # 查看仓库
gh repo list                   # 列出仓库
```

### PR 操作
```bash
gh pr create                   # 创建 PR
gh pr list                     # 列出 PR
gh pr view <num>              # 查看 PR
gh pr checkout <num>          # 检出 PR
gh pr merge <num>             # 合并 PR
gh pr review <num>            # 审查 PR
```

### Issue 操作
```bash
gh issue create               # 创建 Issue
gh issue list                 # 列出 Issue
gh issue view <num>           # 查看 Issue
gh issue close <num>          # 关闭 Issue
gh issue comment <num>        # 评论 Issue
```

### 其他操作
```bash
gh browse                     # 在浏览器打开
gh gist create <file>         # 创建 Gist
gh release create <tag>       # 创建 Release
gh workflow list              # 列出 Workflow
gh run list                   # 列出运行记录
```

---

## 配置文件

### config.yml 位置

```bash
# Linux/Mac
~/.config/gh/config.yml

# Windows
%APPDATA%\GitHub CLI\config.yml
```

### 示例配置

```yaml
# 默认编辑器
editor: code

# Git 协议
git_protocol: ssh

# 提示偏好
prompt: enabled

# 别名
aliases:
  co: pr checkout
  pv: pr view
  il: issue list
```

---

## 高级技巧

### 1. 使用 JSON 输出

```bash
# 获取 JSON 格式输出
gh pr list --json number,title,author

# 使用 jq 处理
gh pr list --json number,title --jq '.[] | select(.title | contains("bug"))'

# 获取 PR 编号列表
gh pr list --json number --jq '.[].number'
```

### 2. 脚本自动化

```bash
#!/bin/bash
# 自动审查和合并 PR

# 获取所有待审查的 PR
prs=$(gh pr list --label "ready-for-review" --json number --jq '.[].number')

for pr in $prs; do
  # 检查 CI 状态
  if gh pr checks $pr | grep -q "All checks have passed"; then
    # 自动批准
    gh pr review $pr --approve --body "LGTM! 自动批准"

    # 合并
    gh pr merge $pr --squash --delete-branch

    echo "已合并 PR #$pr"
  fi
done
```

### 3. 自定义格式化

```bash
# 使用 --template 自定义输出
gh pr list --json number,title,author \
  --template '{{range .}}{{.number}} - {{.title}} (@{{.author.login}}){{"\n"}}{{end}}'
```

---

## 下一步

学习如何将 GitHub CLI 与 Claude Code 结合使用：
- [04-Claude-Code最佳实践.md](./04-Claude-Code最佳实践.md)

---

**提示**：
- 使用 `gh <command> --help` 查看详细帮助
- 多用 `--web` 在浏览器查看详情
- 善用别名提高效率
- 结合 `jq` 处理 JSON 输出
