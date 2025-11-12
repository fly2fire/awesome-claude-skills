# Git 工作流程

本仓库采用三层同步策略，用于管理开源技能和公司内部私有技能。

## Remote 配置

```bash
upstream → https://github.com/ComposioHQ/awesome-claude-skills (上游开源仓库)
origin   → https://github.com/fly2fire/awesome-claude-skills (个人 GitHub fork)
gitlab   → https://gitlab.agrolinking.cn/ai-tools/awesome-claude-skills (公司内部 GitLab)
```

## 分支策略

- **master 分支**：保持与上游同步的公开技能，推送到 GitHub
- **company 分支**：包含公司私有技能，只推送到 GitLab

## 工作流程

### 1. 从上游同步最新的开源技能

```bash
# 切换到 master 分支
git checkout master

# 拉取上游更新
git fetch upstream
git merge upstream/master

# 推送到个人 GitHub fork
git push origin master
```

### 2. 更新公司分支（合并最新的开源技能）

```bash
# 切换到 company 分支
git checkout company

# 合并 master 分支的更新
git merge master

# 推送到 GitLab
git push gitlab company
```

### 3. 添加公司私有技能

```bash
# 确保在 company 分支
git checkout company

# 创建新的私有技能目录
mkdir -p company-skill-name/
# 添加 SKILL.md 和其他文件...

# 提交更改
git add company-skill-name/
git commit -m "Add company private skill: skill-name"

# 推送到 GitLab（仅推送到 GitLab，不推送到 GitHub）
git push gitlab company
```

### 4. 贡献开源技能到上游

如果您创建了可以公开的技能，想贡献回上游：

```bash
# 在 master 分支创建功能分支
git checkout master
git checkout -b add-public-skill

# 添加技能
mkdir -p public-skill-name/
# 添加 SKILL.md 等...

# 提交
git add public-skill-name/
git commit -m "Add [Skill Name] skill"

# 推送到您的 GitHub fork
git push origin add-public-skill

# 然后在 GitHub 上创建 PR 到 ComposioHQ/awesome-claude-skills
```

## 重要原则

✅ **允许的操作**：
- upstream → master → origin (同步开源技能)
- master → company → gitlab (将开源技能同步到公司仓库)
- company → gitlab (添加私有技能到公司仓库)

❌ **禁止的操作**：
- company → origin (不要将私有技能推送到 GitHub)
- company → upstream (不要将私有技能推送到上游)

## 快速参考

```bash
# 查看当前分支
git branch -v

# 查看 remote 配置
git remote -v

# 查看当前状态
git status

# 切换分支
git checkout master    # 公开技能分支
git checkout company   # 公司私有技能分支
```

## 配置 GitLab 认证

推荐使用 SSH 方式：

```bash
# 1. 生成 SSH 密钥（如果还没有）
ssh-keygen -t ed25519 -C "your_email@example.com"

# 2. 将公钥添加到 GitLab
cat ~/.ssh/id_ed25519.pub
# 复制到 GitLab Settings > SSH Keys

# 3. 测试连接
ssh -T git@gitlab.agrolinking.cn

# 4. 可选：将 GitLab remote 改为 SSH URL
git remote set-url gitlab git@gitlab.agrolinking.cn:ai-tools/awesome-claude-skills.git
```

或使用 Personal Access Token (HTTPS 方式)：

```bash
# 配置 Git 凭据存储
git config --global credential.helper osxkeychain  # macOS

# 第一次推送时会提示输入用户名和 token
# 在 GitLab Settings > Access Tokens 创建 token
```
