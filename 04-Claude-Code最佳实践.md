# Claude Code + GitHub 最佳实践

> 将 Claude Code 与 GitHub 结合，打造高效开发工作流

## 目录
- [Claude Code 简介](#claude-code-简介)
- [基础集成](#基础集成)
- [开发工作流](#开发工作流)
- [最佳实践](#最佳实践)
- [实战场景](#实战场景)
- [高级技巧](#高级技巧)

---

## Claude Code 简介

### 什么是 Claude Code？

**Claude Code** 是 Anthropic 推出的 AI 编程助手，可以：
- 编写和修改代码
- 执行 Git/GitHub 命令
- 管理项目文件
- 自动化开发任务

### Claude Code 的优势

1. **自然语言交互**：用人话描述需求
2. **全栈能力**：支持多种编程语言
3. **工具集成**：直接操作 Git、GitHub CLI
4. **上下文理解**：理解项目结构和代码逻辑

---

## 基础集成

### 1. GitHub 认证

Claude Code 可以使用你已经配置好的 `gh` CLI：

```bash
# 检查 gh 登录状态
gh auth status

# 如果未登录，先登录
gh auth login
```

**验证集成**：
```
你：帮我查看我的 GitHub 仓库
Claude：使用 gh repo list 命令
```

### 2. 项目初始化

**方式 1：克隆现有项目**
```
你：帮我克隆 https://github.com/username/project.git 到桌面
Claude：
  1. 使用 gh repo clone
  2. 切换到项目目录
  3. 查看项目结构
```

**方式 2：创建新项目**
```
你：在桌面创建一个新的 Python 项目，并推送到 GitHub
Claude：
  1. 创建项目目录
  2. 初始化 Git
  3. 编写代码
  4. 创建 GitHub 仓库
  5. 推送代码
```

---

## 开发工作流

### 工作流 1：从创建到部署

```
1. 创建项目
   你：创建一个计算器项目

2. 编写代码
   你：实现基本的加减乘除功能

3. 测试
   你：帮我测试这些功能

4. Git 提交
   你：提交代码到 Git

5. 推送到 GitHub
   你：创建 GitHub 仓库并推送

6. 创建 README
   你：写一个详细的 README 文档
```

**Claude Code 会自动**：
- 创建文件和目录
- 编写代码
- 运行测试
- 执行 Git 命令
- 使用 gh CLI 创建仓库
- 推送代码

### 工作流 2：Bug 修复流程

```
1. 发现问题
   你：发现登录功能有 bug

2. 创建 Issue
   你：在 GitHub 创建一个 Issue 报告这个 bug

3. 创建分支
   你：创建一个修复分支

4. 修复代码
   你：帮我修复这个登录 bug

5. 测试
   你：测试修复是否有效

6. 提交和推送
   你：提交修复并推送

7. 创建 PR
   你：创建一个 Pull Request

8. 合并
   你：帮我合并这个 PR
```

### 工作流 3：功能开发流程

```
1. 计划
   你：我想添加一个用户注册功能，帮我规划一下

2. 创建功能分支
   你：创建 feature/user-registration 分支

3. 实现功能
   你：实现用户注册的后端和前端

4. 逐步提交
   你：分步提交代码（后端、前端、测试）

5. 创建 PR
   你：创建 PR 并添加详细说明

6. 代码审查
   你：检查代码质量和安全性

7. 修改反馈
   你：根据审查意见修改代码

8. 合并和清理
   你：合并 PR 并删除分支
```

---

## 最佳实践

### 1. 清晰的指令

**❌ 不好的指令**：
```
你：帮我弄一下代码
```

**✅ 好的指令**：
```
你：帮我创建一个 Python 项目，实现 Fibonacci 数列计算，
    包含多种算法（递归、迭代、矩阵），
    然后推送到 GitHub 公开仓库
```

### 2. 分步执行

**大型任务分解**：
```
❌ 一次性要求：
你：创建一个完整的电商网站

✅ 分步进行：
你：先创建项目结构
你：实现用户认证模块
你：实现商品管理模块
你：实现购物车功能
你：每个模块完成后提交到 Git
```

### 3. 利用 Git 最佳实践

**提交频率**：
```
✅ 经常提交小的改动
你：实现了登录功能，帮我提交
你：修复了验证 bug，再提交一次
你：添加了测试，再提交

❌ 一次提交大量改动
你：所有功能都完成了，一起提交
```

**分支策略**：
```
✅ 使用功能分支
你：创建 feature/dark-mode 分支开发暗黑模式
你：创建 bugfix/login-error 分支修复登录问题

❌ 直接在 main 分支工作
你：直接修改 main 分支的代码
```

### 4. 文档优先

```
推荐流程：
1. 你：先创建项目和 README
2. 你：实现功能
3. 你：更新 README 添加使用说明
4. 你：推送到 GitHub

这样 GitHub 上的项目从一开始就有清晰的文档
```

### 5. 代码审查

```
你：帮我审查刚才写的代码，检查：
   - 代码质量
   - 安全漏洞
   - 性能问题
   - 最佳实践
```

Claude Code 会：
- 分析代码结构
- 指出潜在问题
- 提供改进建议
- 自动修复简单问题

---

## 实战场景

### 场景 1：快速原型开发

**需求**：在 30 分钟内创建一个待办事项 Web 应用

```
你：创建一个待办事项应用，要求：
   1. 使用 Python Flask 后端
   2. 简单的 HTML/CSS/JS 前端
   3. 支持添加、删除、标记完成
   4. 数据持久化（SQLite）
   5. 推送到 GitHub

Claude 会：
1. 创建项目结构
   ├── app.py
   ├── templates/
   │   └── index.html
   ├── static/
   │   ├── style.css
   │   └── script.js
   ├── requirements.txt
   └── README.md

2. 实现所有功能
3. 测试应用
4. 初始化 Git
5. 创建 GitHub 仓库
6. 推送代码
7. 生成完整文档
```

### 场景 2：贡献开源项目

**需求**：为开源项目修复 bug 或添加功能

```
第1步：Fork 项目
你：帮我 Fork https://github.com/author/awesome-project

第2步：克隆到本地
你：克隆我 Fork 的仓库

第3步：创建分支
你：创建 fix/typo-in-readme 分支

第4步：修改
你：修复 README 中的拼写错误

第5步：提交
你：提交修复

第6步：推送
你：推送到我的 Fork

第7步：创建 PR
你：创建 PR 到原仓库

Claude 会自动：
- 使用 gh repo fork
- 创建适当的分支
- 进行修改
- 生成规范的 commit message
- 创建详细的 PR 说明
```

### 场景 3：项目迁移

**需求**：将本地项目上传到 GitHub

```
你：我有一个本地项目在 C:\Users\xxx\my-project，
   帮我推送到 GitHub

Claude 会：
1. 检查项目是否已初始化 Git
   git status

2. 如果没有，初始化
   git init

3. 创建 .gitignore
   根据项目类型添加合适的忽略规则

4. 添加文件
   git add .

5. 提交
   git commit -m "Initial commit"

6. 创建 GitHub 仓库
   gh repo create --source=. --public --push

7. 推送
   git push -u origin main
```

### 场景 4：多人协作

**团队工作流**：

```
情况1：同步团队最新代码
你：拉取最新代码

Claude：
  git pull --rebase origin main

情况2：解决合并冲突
你：合并 develop 分支时有冲突，帮我解决

Claude：
  1. 分析冲突文件
  2. 理解两边的改动
  3. 智能合并或询问你的选择
  4. 标记为已解决
  5. 完成合并

情况3：代码审查
你：帮我审查 PR #123

Claude：
  1. gh pr view 123
  2. gh pr diff 123
  3. 分析代码变更
  4. 提供审查意见
  5. gh pr review 123 --comment
```

### 场景 5：发布管理

**版本发布流程**：

```
你：准备发布 v1.0.0 版本

Claude 会：
1. 更新版本号
   - package.json
   - setup.py
   - 其他配置文件

2. 生成 CHANGELOG
   基于 git log 生成更新日志

3. 提交版本更新
   git add .
   git commit -m "chore: 发布 v1.0.0"

4. 创建标签
   git tag -a v1.0.0 -m "Version 1.0.0"

5. 推送
   git push origin main --tags

6. 创建 GitHub Release
   gh release create v1.0.0 \
     --title "Version 1.0.0" \
     --notes-file CHANGELOG.md

7. 上传构建产物
   gh release upload v1.0.0 dist/*
```

---

## 高级技巧

### 1. 自动化工作流

**创建 GitHub Actions**：

```
你：帮我创建一个 GitHub Actions 工作流，
   每次推送时自动运行测试

Claude 会创建 .github/workflows/test.yml：
```yaml
name: Run Tests

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.10'
      - name: Install dependencies
        run: pip install -r requirements.txt
      - name: Run tests
        run: pytest
```

### 2. 批量操作

**批量更新仓库**：

```
你：帮我更新所有仓库的 README，添加统一的 License 说明

Claude 会：
1. gh repo list --limit 100
2. 对每个仓库：
   - 克隆
   - 修改 README
   - 提交
   - 推送
```

### 3. 智能搜索

**在代码库中查找**：

```
你：找出所有使用了 deprecated API 的地方

Claude：
  1. gh search code "deprecated_function" --repo owner/repo
  2. 列出所有匹配的文件
  3. 提供替换建议
```

### 4. 问题诊断

**Git 问题排查**：

```
你：git push 失败了，帮我看看

Claude 会：
1. 检查错误信息
2. 诊断问题（权限、网络、冲突等）
3. 提供解决方案
4. 执行修复命令
```

### 5. 代码重构

**安全重构**：

```
你：帮我重构这个函数，但要确保：
   1. 功能不变
   2. 有对应的测试
   3. 在新分支进行
   4. 创建 PR

Claude 会：
1. git checkout -b refactor/improve-performance
2. 重构代码
3. 编写/更新测试
4. 验证测试通过
5. 提交
6. gh pr create
```

---

## 配置和优化

### 1. Git 配置优化

```
你：帮我优化 Git 配置

Claude 会设置：
```bash
# 提高性能
git config --global core.preloadindex true
git config --global core.fscache true
git config --global gc.auto 256

# 更好的日志
git config --global alias.lg "log --oneline --graph --all"

# 自动修复空白错误
git config --global apply.whitespace fix

# 默认推送当前分支
git config --global push.default current

# 使用 rebase 而不是 merge
git config --global pull.rebase true
```

### 2. GitHub CLI 配置

```
你：配置 gh CLI 的默认行为

Claude 会配置 ~/.config/gh/config.yml：
```yaml
git_protocol: ssh
editor: code
prompt: enabled
pager: less
aliases:
  pv: pr view
  co: pr checkout
  il: issue list --assignee @me
```

### 3. .gitignore 模板

```
你：创建一个 Python 项目的 .gitignore

Claude 会生成：
```gitignore
# Python
__pycache__/
*.py[cod]
*$py.class
*.so
.Python
venv/
env/

# IDE
.vscode/
.idea/
*.swp

# OS
.DS_Store
Thumbs.db

# Project specific
*.log
secrets.json
```

---

## 安全最佳实践

### 1. 敏感信息保护

```
❌ 不要提交：
- API 密钥
- 密码
- 私钥
- 数据库凭证

✅ 使用环境变量：
你：帮我创建 .env 文件管理敏感配置

Claude 会：
1. 创建 .env.example（模板）
2. 添加 .env 到 .gitignore
3. 修改代码使用环境变量
```

### 2. 代码审查检查项

```
你：审查代码时重点检查安全问题

Claude 会检查：
- SQL 注入风险
- XSS 漏洞
- 硬编码的凭证
- 不安全的加密
- 输入验证
```

### 3. 依赖管理

```
你：检查项目依赖是否有安全漏洞

Claude：
  npm audit         # Node.js
  pip-audit         # Python
  gh api repos/:owner/:repo/vulnerability-alerts
```

---

## 常见问题解决

### 问题 1：推送被拒绝

```
你：git push 提示 rejected

Claude 诊断：
1. 检查是否需要先 pull
   git pull --rebase origin main

2. 检查是否有权限问题
   gh auth status

3. 检查分支保护规则
   gh repo view --json defaultBranchRef
```

### 问题 2：合并冲突

```
你：合并时遇到冲突

Claude：
1. 显示冲突文件
   git status

2. 分析冲突内容
   git diff

3. 提供合并建议

4. 解决冲突后
   git add .
   git commit
```

### 问题 3：误提交敏感文件

```
你：我不小心提交了密码文件

Claude：
1. 从历史中移除
   git filter-branch --tree-filter 'rm -f passwords.txt' HEAD

2. 强制推送
   git push -f origin main

3. 修改密码（重要！）

4. 添加到 .gitignore
```

---

## 效率提升技巧

### 1. 使用别名

```
你：创建常用的 Git 别名

Claude 配置：
```bash
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.cm commit
git config --global alias.unstage 'reset HEAD --'
```

### 2. 批处理脚本

```
你：创建一个脚本，每天自动同步所有项目

Claude 创建 sync-all.sh：
```bash
#!/bin/bash
for repo in ~/projects/*; do
  if [ -d "$repo/.git" ]; then
    cd "$repo"
    echo "Syncing $repo"
    git pull --rebase
    git push
  fi
done
```

### 3. 模板使用

```
你：创建项目模板

Claude：
1. 创建模板仓库
2. 设置为 GitHub 模板
3. 包含常用配置：
   - .gitignore
   - README 模板
   - LICENSE
   - GitHub Actions
   - Issue/PR 模板
```

---

## 学习资源

### 推荐阅读顺序

1. [01-基础概念.md](./01-基础概念.md) - 了解 Git/GitHub 基础
2. [02-Git命令详解.md](./02-Git命令详解.md) - 掌握 Git 命令
3. [03-GitHub-CLI使用.md](./03-GitHub-CLI使用.md) - 学习 gh CLI
4. **本文档** - 结合 Claude Code 实践

### 实践建议

1. **从小项目开始**
   ```
   你：创建一个简单的 Hello World 项目
   ```

2. **逐步增加复杂度**
   ```
   你：添加测试
   你：添加 CI/CD
   你：添加文档
   ```

3. **参与开源项目**
   ```
   你：帮我找一个适合新手的开源项目
   你：帮我贡献第一个 PR
   ```

---

## 总结

### Claude Code + GitHub 的黄金组合

```
自然语言需求 → Claude Code 理解 → 自动执行 Git/GitHub 操作
     ↑                                          ↓
     └──────────── 反馈和确认 ←──────────────────┘
```

### 核心优势

1. **效率提升**：几分钟完成原本需要几小时的工作
2. **最佳实践**：自动遵循 Git/GitHub 规范
3. **减少错误**：避免常见的 Git 操作失误
4. **学习加速**：在实践中学习最佳实践

### 记住

- 清晰表达需求
- 充分利用 Claude 的上下文理解
- 保持良好的 Git 习惯
- 经常提交，详细说明
- 善用分支和 PR
- 重视代码审查和安全

---

**现在就开始使用 Claude Code + GitHub，开启高效开发之旅吧！**
