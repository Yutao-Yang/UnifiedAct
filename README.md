# UnifiedAct

协作仓库：https://github.com/Yutao-Yang/UnifiedAct

开发机集成目录：`/project/fileset/xts0083/public/UnifiedAct`。
当前仅提交本 README。研究代码、数据和既有实验历史不自动导入；后续逐项审查后再加入。仓库当前公开，push 的文件及提交历史会对外可见。

## 协作约定

- `main` 保存双方确认、可以交付的版本。自己的工作先在个人分支完成，自查后才 commit；准备给对方看时才 push。草稿留在自己的工作目录，不推送临时提交。
- 两人同时开发必须使用不同工作目录。分支并不能隔离同一目录的文件、暂存区和 checkout；禁止在同一目录同时编辑、切分支或合并。
- 推荐每人独立 clone，身份、暂存区与 Git 配置也各自独立。共享集成目录由当次约定的负责人操作，操作期间另一人不修改。
- 只提交本人确认的文件与 diff，不用 `git add .` 将他人的工作一起带入。不得覆盖他人未提交的修改；不用 force push、`reset --hard` 或 `clean -fd` 清场。
- 数据、模型、权重、视频、缓存、环境、凭据、签名 URL、原始许可内容不入 Git；第三方代码、数据说明和后续开源内容先核授权。仓库公开不表示研究资产已获准公开。

## 首次建立个人工作目录（推荐）

在自己有写权限的位置运行；替换下列占位路径与姓名，不要直接复用对方目录。HTTPS 推送使用个人 GitHub 凭据管理器或个人 SSH 配置，不把 token 放进 URL、命令、脚本或文件。

```bash
git clone https://github.com/Yutao-Yang/UnifiedAct.git /你的目录/UnifiedAct
cd /你的目录/UnifiedAct
git config --local user.name "你的姓名"
git config --local user.email "你的 GitHub 已验证邮箱或 noreply 邮箱"
git checkout -b yourname/topic
```

若两人共用服务器系统账号，工作目录仍需分开；不能共用提交身份。认证凭据也可能由同一系统账号共享，需要另行配置个人认证。

## 日常开发与自查提交

```bash
git status --short --branch
git fetch origin
# 开始新任务时，从最新 main 建个人分支
git checkout -b yourname/new-topic origin/main

# 修改后，先检查全部差异，并运行与改动相关的测试
git diff --check
git diff
git status --short
git add README.md  # 示例：逐个指定本次确认的文件
git diff --cached --check
git diff --cached
git commit -m "docs: clarify collaboration workflow"

# 只有准备交给对方审核的分支才 push
git push -u origin yourname/new-topic
```

已在任务分支工作时不重复建分支。提交前确认作者身份 `git var GIT_AUTHOR_IDENT`，暂存区没有他人文件、敏感内容或大资产。未提交的内容先自行整理，不带着脏目录切分支或合并。

## Commit 规范

格式：`type: 简明描述`，也可写 `type(scope): 简明描述`。描述可中文或英文，但需说明实际改变。

| type | 用途 | 示例 |
| --- | --- | --- |
| feat | 新功能 | `feat(atoms): add boundary metadata export` |
| fix | 修复问题 | `fix: preserve missing observation masks` |
| docs | 文档 | `docs: explain sample review procedure` |
| refactor | 不改变行为的整理 | `refactor: consolidate coordinate transforms` |
| test | 测试 | `test: cover split ancestry checks` |
| chore | 配置与维护 | `chore: exclude local render outputs` |

一个提交只解决一个完整问题，避免 `update`、`temp`、`try again`。非平凡改动在提交正文说明原因、验证命令及结果、已知限制。尚未运行的验证写“未运行”，候选样本不写成已验收，几何检查不写成物理认证。

自己的本地试验提交可以整理成一个已自查提交；分支一旦分享给别人，未经协商不要改写历史。Git 不会自动证明“本人已确认”，这依赖双方执行约定；当前没有配置 GitHub 分支保护或强制审批。

## 审核与合并

推荐推个人分支后，在 GitHub 建 Pull Request；说明改动、验证与限制。由另一人看完差异并确认后，使用 Squash and merge，让 main 只保留完整结果。合并前检查最新 main 上的兼容性；若有冲突，由改动作者处理并重新验证。

如果双方已确认，也可由集成负责人在干净的共享目录操作：

```bash
cd /project/fileset/xts0083/public/UnifiedAct
git status --short  # 必须先确认干净；有改动就停下联系其作者
git fetch origin
git checkout main
git pull --ff-only origin main
git diff --stat main...origin/yourname/new-topic
git diff main...origin/yourname/new-topic
git merge --squash origin/yourname/new-topic
git diff --cached --check
git diff --cached
# 运行相关验证；确认无误后提交并推送
git commit -m "feat: describe the reviewed result"
git push origin main
```

冲突未解决或验证失败时不提交、不推送。共享目录的提交前也要设置并核对本次提交者身份。GitHub 合并后，其他人执行 `git fetch origin`，在自己的干净 main 上 `git pull --ff-only origin main`；个人任务分支可 `git merge origin/main` 后继续验证。

## 仅在服务器开发、暂不分享个人分支

可以从共享集成目录建立独立 clone，完成并自查后才由负责人取入；个人目录必须在自己有权限的位置：

```bash
git clone --no-hardlinks /project/fileset/xts0083/public/UnifiedAct /你的目录/UnifiedAct
cd /你的目录/UnifiedAct
git remote rename origin integration
git remote add origin https://github.com/Yutao-Yang/UnifiedAct.git
git config --local user.name "你的姓名"
git config --local user.email "你的邮箱"
git checkout -b yourname/topic
# 完成、自查、指定文件暂存并 commit 后，告知负责人路径与 commit SHA
git rev-parse HEAD
```

负责人在干净的共享目录 `git fetch /对方的目录/UnifiedAct yourname/topic`，核对 `git diff main...FETCH_HEAD`，双方确认后 `git merge --squash FETCH_HEAD`，检查暂存差异、验证、commit，再 push。取入会下载该分支可达的历史，因此仍需先整理个人历史。若只使用同一个目录，则必须轮流操作，前一人完成并清理工作区后再交接。
