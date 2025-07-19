## 附录：Git教程与规范

### 1. 基本概念

- 版本控制：版本控制是指对文件的修改历史进行记录和管理，使得每次更改都可以追溯和恢复。Git 是一种分布式版本控制系统，与集中式版本控制（如 SVN）相比，每个开发者都有一个完整的代码仓库副本，能更好地支持离线工作和协作。

- 仓库 (Repository)：一个项目的存储库，包含了所有的文件、历史记录和提交信息。Git 仓库可以是本地的，也可以是远程的。

- 提交 (Commit)：每次修改文件后的存档记录。每个提交都会有一个唯一的哈希值标识，可以查看这个提交的具体内容，甚至回到历史某个版本。

- 分支 (Branch)：Git 允许你在同一仓库中创建多个分支。分支用于开发不同的功能或修复不同的问题，分支之间相互独立，可以独立地进行提交，不会影响主分支（通常是 `main` 或 `master`）。

- 合并 (Merge)：将不同分支的内容合并到一起，通常是在完成某个功能开发后，合并回主分支。

- 远程仓库 (Remote Repository)：存储在远程服务器上的仓库，多个开发者可以推送 (push) 和拉取 (pull) 代码，以便共享和同步工作进度。

### 2. 常用 Git 命令

- git init：初始化一个新的 Git 仓库，创建 `.git` 目录，用于跟踪项目中的文件变化。

  ``` sourceCode
  git init
  ```

- git clone \<repo_url\>：从远程仓库克隆一个 Git 仓库到本地。

  ``` sourceCode
  git clone https://github.com/user/repo.git
  ```

- git status：查看工作区文件的当前状态，查看哪些文件被修改、哪些文件处于暂存区等。

  ``` sourceCode
  git status
  ```

- git add ：将文件添加到暂存区，准备提交。

  ``` sourceCode
  git add filename
  ```

- git commit -m "message"：将暂存区的修改提交到本地仓库，并附上提交信息。

  ``` sourceCode
  git commit -m "Fixed a bug in the login module"
  ```

- git log：查看提交历史，按时间顺序列出所有提交记录。

  ``` sourceCode
  git log
  ```

- git diff：查看文件与版本库中上一个提交之间的差异。

  ``` sourceCode
  git diff
  ```

- git pull：从远程仓库拉取最新的代码，并将其合并到当前分支。

  ``` sourceCode
  git pull origin main
  ```

- git push：将本地仓库的提交推送到远程仓库。

  ``` sourceCode
  git push origin main
  ```

- git branch：查看当前的分支。

  ``` sourceCode
  git branch
  ```

- git checkout ：切换到另一个分支。

  ``` sourceCode
  git checkout feature-branch
  ```

- git merge ：将某个分支的修改合并到当前分支。

  ``` sourceCode
  git merge feature-branch
  ```

### 3. 分支管理与合并

在 Git 中，分支是一个非常重要的概念，它使得你能够在不干扰主分支的情况下进行独立的开发工作。通过分支，你可以在不同的开发任务之间切换，例如修复 bug、新功能开发、实验性开发等。

创建分支：

``` sourceCode
git branch new-feature
```

切换分支：

``` sourceCode
git checkout new-feature
```

创建并切换分支：

``` sourceCode
git checkout -b new-feature
```

合并分支：

假设你在 `feature-branch` 分支上开发了一个新功能，现在你希望将其合并到 `main` 分支：

1. 首先切换到 `main` 分支：

   ``` sourceCode
   git checkout main
   ```

2. 然后合并 `feature-branch` 分支：

   ``` sourceCode
   git merge feature-branch
   ```

解决冲突：

如果在合并过程中出现了冲突，Git 会提示冲突文件。此时，开发者需要手动编辑文件并解决冲突，解决后再执行：

``` sourceCode
git add <resolved-file>
git commit -m "Resolved merge conflict"
```

### 4. Git 工作流

常见的 Git 工作流有：

- 集中式工作流：所有开发者都在 `main` 或 `master` 分支上工作，常见于小团队或者单人开发。

- 功能分支工作流：每个功能或修复问题都在独立的分支上进行开发，开发完成后再合并回 `main` 分支。这是团队协作中常见的工作流。

- Git Flow 工作流：Git Flow 是一种较为复杂的工作流，通常包括 `master`、`develop`、`feature`、`release`、`hotfix` 等多个分支，适用于中大型团队。

- Fork-Branch 工作流：适用于开源项目，开发者从主仓库 `fork` 一个副本，进行开发后通过 pull request 提交回主仓库。

### 5.Git初始配置：

设置作者信息

    git config --global user.name "Your Name"
    git config --global user.email you@example.com

设置默认分支名字

``` sourceCode
git config --global init.defaultBranch dev
```

### 6.本次开发的分支策略

`main`（发布分支）

- 只保留稳定可运行的版本。

- 所有 release 均从此分支打 tag。

`dev`（开发主干，默认分支）

- 所有功能合并到此分支测试后再进入 `main`。

- 保持干净、有序的提交历史。

`feature/*`（功能开发分支）

- 每个模块/子功能单独开分支，开发完成后合并到 `dev`。

### 7.Git 提交模板

``` text
类型(范围): 简要说。/明

说明：
- 简要说明修改的动机和背景（可选）

变更内容：
- [ ] 简述修改点1
- [ ] 简述修改点2

备注：
- 关联问题：#编号
- 是否为破坏性变更：否
```

#### 示例提交

``` text
feat(auth): 新增 JWT 登录支持

说明：
- 实现用户登录后返回 JWT，替代原有 session 机制
- 整合认证流程，统一登录逻辑

变更内容：
- [x] 新增 jwt_middleware.go
- [x] 移除旧版 session 验证逻辑

备注：
- 关联问题：#42
- 是否为破坏性变更：是
```

#### Git提交常用类型（type）

| 类型     | 说明                     |
| -------- | ------------------------ |
| add      | 新增功能                 |
| fix      | 修复缺陷                 |
| docs     | 修改文档                 |
| style    | 格式调整（不影响功能）   |
| refactor | 重构（无新增功能或修复） |
| perf     | 性能优化                 |
| test     | 添加或修改测试           |
| chore    | 构建配置、工具变动       |
| revert   | 回滚提交                 |
| release  | 发布新版本               |

#### 设置 Git 提交模板

1.  保存模板文件（如 `~/.gitmessage.zh`）：

``` sourceCode
cat > ~/.gitmessage.zh << 'EOF'
类型(范围): 简要说明

说明：
- 简要说明修改的动机和背景（可选）

变更内容：
- [ ] 

备注：
- 关联问题：#Nil
- 是否为破坏性变更：No
EOF
```

1.  设置 Git 使用该模板：

``` sourceCode
git config --global commit.template ~/.gitmessage.zh
```

3、使用方法

运行 `git commit`（不加 `-m`），Git 会打开编辑器并自动填入模板。你只需补充内容并保存即可。

> 转到：[Go 的场景下的专用范围（如 `core`、`api`、`cli`）]()

### 8.我的Git 别名设置

直接编辑 `~/.gitconfig` 文件

打开并编辑全局配置文件：

    nano ~/.gitconfig

添加或修改 `[alias]` 部分：

    [alias]
        st = status
        co = checkout
        cm = commit
        br = branch
        lg = log --oneline --graph --decorate --all
        psh = push
        pl = pull
        last = log -1 HEAD
        unstage = reset HEAD
        alias = config --get-regexp alias
        undo = reset --hard HEAD
        l = log --oneline --graph --decorate
        visual = log --oneline --graph --all --decorate --color
        dc = diff --cached
        ds = diff
        acp = !f() { git add . && git commit -m \"$*\" && git push; }; f

保存后即可使用。

我的这套涵盖了常用操作（分支、提交、日志、推送、撤销等）和增强型日志查看（`lg`、`visual`），适合入门新手使用。

| 别名      | 实际命令                                                 | 功能说明                           | 推荐级别 | 建议                         |
| --------- | -------------------------------------------------------- | ---------------------------------- | -------- | ---------------------------- |
| `st`      | `status`                                                 | 查看当前仓库状态                   | 推荐     | 无需修改                     |
| `co`      | `checkout`                                               | 切换分支                           | 推荐     | 可选添加 `switch` 用法       |
| `cm`      | `commit`                                                 | 提交变更                           | 推荐     | 可改为 `ci` 更常见           |
| `br`      | `branch`                                                 | 查看/新建分支                      | 推荐     | 无需修改                     |
| `psh`     | `push`                                                   | 推送变更                           | 推荐     | 也可使用 `pu` 更短           |
| `pl`      | `pull`                                                   | 拉取更新                           | 推荐     | 无需修改                     |
| `lg`      | `log --oneline --graph --decorate --all`                 | 可视化 Git 历史图谱（推荐）        | 推荐     | 无需修改                     |
| `last`    | `log -1 HEAD`                                            | 查看最后一次提交                   | 推荐     | 可重命名为 `latest` 更语义化 |
| `unstage` | `reset HEAD`                                             | 取消 `git add` 的暂存操作          | 推荐     | 可配上参数提示更完整         |
| `alias`   | `config --get-regexp alias`                              | 查看所有别名                       | 推荐     | 可添加说明文档               |
| `undo`    | `reset --hard HEAD`                                      | 撤销当前工作区与暂存区（危险操作） | ⚠️ 有风险 | 添加确认提示更安全           |
| `l`       | `log --oneline --graph --decorate`                       | 基本可视化历史（不含所有分支）     | 推荐     | 和 `lg` 做功能区分           |
| `visual`  | `log --oneline --graph --all --decorate --color`         | 带颜色的完整日志图                 | 推荐     | 视觉增强，无需修改           |
| `dc`      | `diff --cached`                                          | 查看暂存区的 diff                  | 推荐     | 可改为 `dfc` 更清晰          |
| `ds`      | `diff`                                                   | 查看工作区的 diff                  | 推荐     | 无需修改                     |
| `acp`     | `!f() { git add . && git commit -m $1 && git push; }; f` | 一键添加、提交并推送               | ⚠️ 有限制 | 建议支持空格和输入交互       |

### 9.发布 Release 流程

1.  切换到 `main` 分支

``` sourceCode
git checkout main
```

确保本地是最新的：

``` sourceCode
git pull origin main
```

1.  合并你要发布的变更（如果你是在 feature/dev 分支开发）

``` sourceCode
git merge dev
# 或者 git merge feature/xxx
```

合并后可使用 `git status` 或 `git diff` 检查内容。

1.  创建 Release 提交（可选，作为版本节点说明）

``` sourceCode
git commit -m "release: 发布 v1.2.0 版本，包含用户权限优化与API重构"
```

如果无额外改动，可跳过。

1.  打版本标签（Tag）

``` sourceCode
git tag -a v1.2.0 -m "发布 v1.2.0：优化性能，修复登录Bug"
```

其中：

- `-a`：创建附注标签（annotated tag，推荐）

- `-m`：标签说明，写清楚版本亮点

1.  推送代码和标签

``` sourceCode
git push origin main         # 推送主分支代码
git push origin v1.2.0       # 推送对应 tag
```

你也可以一次性推送所有 tag：

``` sourceCode
git push origin --tags
```

> 