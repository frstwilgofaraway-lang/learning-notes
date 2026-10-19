# Git 命令复习笔记（Day 6 实战版）

> 用途：复习 2026-10-18 这天实际用到的 Git 命令。
> 特点：**每个命令都是主人今天真的敲过的**，不是抄文档，所以特别好记。
> 建议：忘了就翻这里，比搜教程快。

---

## 一、今天实际用过的命令（按使用顺序）

### 1. `git init` —— 把文件夹变成 Git 仓库

```powershell
cd D:\code
git init
```

**作用**：在当前目录创建一个 `.git` 隐藏文件夹，从此这个目录的改动 Git 都能跟踪了。

**执行后**：目录里多了一个 `.git` 文件夹（不要手动改它里面的东西）。

**什么时候用**：一个项目**只做一次**。已经有了就不用再做。

**判断有没有做过**：
```powershell
# 看目录里有没有 .git 文件夹
Get-ChildItem -Force
# 或者直接问 git
git status
```

---

### 2. `git status` —— 查看当前状态（最常用！）

```powershell
git status
```

**作用**：告诉你三件事
1. 现在在哪个分支
2. 哪些文件改了但没暂存
3. 哪些文件暂存了等着提交

**三种典型输出**：

```
# 情况 A：有未跟踪的新文件
Untracked files:
  hello.js

# 情况 B：改动已暂存，等着提交
Changes to be committed:
  new file:   hello.js

# 情况 C：改动了但没暂存
Changes not staged for commit:
  modified:   hello.js

# 情况 D：一切干净
nothing to commit, working tree clean
```

**建议**：**每敲完一条命令就 `git status` 看一眼**，这是最快的学习方式。

**简写版**（输出更紧凑）：
```powershell
git status -s
# 输出类似：
#  A  .gitignore     ← A = 已暂存的新文件
#  M  hello.js       ← M = 已修改但未暂存
# ?? test.js         ← ?? = 未跟踪
```

---

### 3. `git add` —— 把改动放进暂存区

```powershell
git add .                    # 当前目录所有改动
git add hello.js             # 只加某一个文件
git add practice/            # 只加某个文件夹
git add *.js                 # 所有 .js 文件
```

**作用**：把工作区的改动"挑出来"，放进暂存区（staging area），准备提交。

**为什么要有这一步**：因为你可以改 10 个文件，但只想提交其中 3 个。暂存区就是让你挑的。

**`. `的含义**：当前目录及子目录下所有改动（新增 + 修改 + 删除）。

**今天实际用的**：
```powershell
git add .
```

---

### 4. `git commit` —— 存一次档

```powershell
git commit -m "我的第一个提交"
```

**拆解**：
- `commit` = 提交
- `-m` = message（说明），后面跟一句话

⚠️ **今天踩的坑**：敲成了 `git -m "..."`，漏了 `commit`，报错 `unknown option: -m`。
**记住**：`git` 和 `-m` 中间必须有子命令（`commit`）。

**提交说明怎么写**：

```
✅ 好的写法（动词开头 + 说清改了什么）
新增 hello.js 打印 Hello World
添加第一周学习计划文件
初始化项目，加入 .gitignore

❌ 坏的写法
改了
update
111
```

**为什么要写清楚**：以后回看历史时，这是唯一的线索。三个月后你会感谢自己。

**提交后**：
```
[main (root-commit) 12de23a] 我的第一个提交
 6 files changed, 1172 insertions(+)
 create mode 100644 .gitignore
 create mode 100644 hello.js
```

`12de23a` 是这次提交的**哈希值**（唯一编号），以后能靠它找回。

---

### 5. `git log` —— 查看提交历史

```powershell
git log                      # 详细版
git log --oneline            # 一行一个提交（推荐）
git log --oneline --graph    # 带分支图
git log -5                   # 只看最近 5 条
```

**今天的输出**：
```
12de23a 我的第一个提交
```

**常用组合**：
```powershell
git log --oneline --graph --all   # 看所有分支的完整历史图
```

**退出方式**：如果输出很长进了分页界面，按 `q` 退出。

---

### 6. `git branch` —— 分支操作

```powershell
git branch                   # 列出所有本地分支（* 号是当前分支）
git branch -M main           # 把当前分支重命名为 main（-M 强制）
git branch 新分支名           # 新建分支
git branch -d 分支名          # 删除分支
```

**今天的情况**：因为之前配过 `init.defaultBranch = main`，`git init` 时分支**直接就叫 main** 了，所以 `git branch -M main` 是**多余的**（敲了也没坏处）。

**验证当前分支**：
```powershell
git branch --show-current    # 只输出分支名，脚本里好用
```

---

### 7. `git remote` —— 管理远端仓库

```powershell
git remote add origin <URL>      # 添加远端
git remote -v                    # 查看已配置的远端
git remote remove origin         # 删除远端
git remote set-url origin <URL>  # 修改远端地址
```

**今天的实际命令**：
```powershell
git remote add origin https://github.com/frstwilgofaraway-lang/my-first-repo.git
```

**拆解**：
- `remote` = 远端
- `add` = 添加
- `origin` = 给这个远端起的名字（**约定俗成**，可以改但没必要）
- URL = 仓库地址

⚠️ **今天踩的坑**：用户名写成了 `mas che`（有空格），Git 把地址拆成两个参数，报错 `usage: git remote add ...`。
**教训**：GitHub 用户名**只能是字母、数字、短横线**，不能有空格。真实用户名是 `frstwilgofaraway-lang`。

**`git remote -v` 的正确输出**：
```
origin  https://github.com/frstwilgofaraway-lang/my-first-repo.git (fetch)
origin  https://github.com/frstwilgofaraway-lang/my-first-repo.git (push)
```
两行（fetch 拉取、push 推送），说明配好了。

---

### 8. `git push` —— 推送到 GitHub

```powershell
git push -u origin main    # 第一次推送
git push                   # 以后都这样
```

**拆解**：
- `push` = 推送
- `-u` = 记住这个对应关系（**只需第一次**）
- `origin` = 推到哪个远端
- `main` = 推哪个分支

**`-u` 的作用**：建立本地 `main` 和远端 `origin/main` 的追踪关系。之后直接 `git push` 就行，不用再写 `origin main`。

**验证追踪关系**：
```powershell
git branch -vv
# 输出：* main 12de23a [origin/main] 我的第一个提交
#                      ↑ 这个方括号就是追踪关系
```

⚠️ **今天踩的坑**：前面 commit 没成功，所以 push 报错：
```
error: src refspec main does not match any
error: failed to push some refs to 'origin'
```
**翻译**：本地 `main` 分支是空的，没东西可推。
**解法**：先把 commit 做成功，再 push。

**推送时会要密码**：
- 用户名：GitHub 用户名
- 密码：⚠️ **PAT 令牌**（`ghp_...`），**不是账号密码！**

**为什么**：GitHub 2021 年就废除了密码认证，必须用令牌。

---

## 二、命令速查表（贴墙版）

| 命令 | 作用 | 频率 |
|---|---|---|
| `git status` | 看当前状态 | ⭐⭐⭐⭐⭐ 最高 |
| `git add .` | 暂存所有改动 | ⭐⭐⭐⭐⭐ |
| `git commit -m "说明"` | 提交存档 | ⭐⭐⭐⭐⭐ |
| `git push` | 推到 GitHub | ⭐⭐⭐⭐ |
| `git pull` | 从 GitHub 拉下来 | ⭐⭐⭐⭐ |
| `git log --oneline` | 看提交历史 | ⭐⭐⭐ |
| `git diff` | 看具体改了哪几行 | ⭐⭐⭐ |
| `git branch` | 看/建分支 | ⭐⭐ |
| `git switch` | 切换分支 | ⭐⭐ |
| `git remote -v` | 看远端配置 | ⭐ |
| `git clone` | 克隆别人的仓库 | ⭐⭐ |
| `git restore` | 撤销工作区改动 | ⭐⭐ |

---

## 三、日常三连（最常用的循环）

每天写代码，重复这个循环：

```powershell
# 1. 看看改了什么
git status

# 2. 暂存 + 提交
git add .
git commit -m "完成登录页面"

# 3. 推到 GitHub
git push
```

**记法**：**status → add → commit → push**，四个词走一遍就是一次完整的存档。

---

## 四、今天踩的四个坑（重要！）

### 坑 1：`git -m` 漏了 `commit`

```powershell
❌ git -m "说明"
✅ git commit -m "说明"
```

**报错**：`unknown option: -m`

---

### 坑 2：用户名有空格

```powershell
❌ git remote add origin https://github.com/mas che/my-first-repo.git
✅ git remote add origin https://github.com/frstwilgofaraway-lang/my-first-repo.git
```

**报错**：`usage: git remote add [<options>] <name> <url>`

**原因**：GitHub 用户名只能是字母数字短横线。

---

### 坑 3：没提交就 push

```powershell
❌ 直接 git push -u origin main
✅ 先 git commit，再 git push
```

**报错**：`error: src refspec main does not match any`

**原因**：`main` 分支还是空的。

---

### 坑 4：LF/CRLF 警告

```
warning: LF will be replaced by CRLF the next time Git touches it
```

**这不是错误，只是提示**，说明 Git 准备转换换行符。

- Windows 用 `CRLF`（`0D 0A`，回车+换行）
- Linux/Mac 用 `LF`（`0A`，只有换行）
- `core.autocrlf = true` 时：**提交时** CRLF→LF 存进仓库，**检出时** LF→CRLF 放回磁盘

**警告出现的原因**：文件里原本是 LF 换行，但配置说"检出时转成 CRLF"，Git 提醒你"我要改你的文件了"。

**⚠️ 纠正一个常见误解**：`git config --global core.autocrlf true` **不能关掉这个警告**。
因为 Windows 版 Git 安装时**默认就是 `true`**（主人机器实测就是 true），再设一遍等于没变。

**想真正消掉警告**（可选，新手不必折腾）：
```powershell
# 让仓库里的换行符重新规范化
git add --renormalize .
git commit -m "规范化换行符"
```

**最省事的做法**：**直接无视警告**，它不影响任何功能。

**怎么查当前值**：
```powershell
git config --global core.autocrlf
```

---

## 五、两个「名字」别搞混

| 名字 | 值 | 用在哪 |
|---|---|---|
| **Git 提交署名** | `mas che` | `git log` 里显示"谁提交的" |
| **GitHub 用户名** | `frstwilgofaraway-lang` | 仓库地址、登录 |

这两个可以不一样，没问题。但**填地址时要认准 GitHub 用户名**。

想统一的话：
```powershell
git config --global user.name "frstwilgofaraway-lang"
```

---

## 六、实用小技巧

### 1. 命令太长记不住？用 Tab 补全
Git 支持命令补全，敲 `git com` 然后按 `Tab`，会自动补成 `commit`。

### 2. 翻历史命令
按 **`↑`** 方向键，能翻出刚才敲过的命令，不用重新打。

### 3. `git status` 随时看
不确定当前状态时，**先 `git status`**，它会告诉你下一步该干什么。

### 4. 后悔药：撤销暂存
```powershell
git restore --staged 文件名
```
把文件从暂存区拿回工作区（不删改动）。

### 5. 后悔药：撤销文件改动
```powershell
git restore 文件名
```
⚠️ 这会**丢弃**这个文件的所有改动，谨慎用。

### 6. 看某个提交改了什么
```powershell
git show 12de23a
```

### 7. 想知道某行代码是谁写的
```powershell
git blame 文件名
```

---

## 七、配置相关

### 7.1 身份配置（今天配过的）

```powershell
# 配置提交署名（只做一次）
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"

# 默认分支名设为 main（只做一次）
git config --global init.defaultBranch main

# 查看所有配置
git config --global --list

# 查看某一项
git config --global user.name
```

**`--global` 的意思**：对这台电脑上所有仓库生效。不加 `--global` 就只对当前仓库生效。

---

### 7.2 `core.quotepath` —— 中文文件名不乱码（强烈建议设）

```powershell
git config --global core.quotepath false
```

**作用**：让 Git 在终端里**直接显示中文文件名**，而不是显示八进制转义码。

#### 实测对比（2026-10-18 主人机器上的真实输出）

**默认 `true`（不设）**：
```
D  "plan/\345\267\245\344\275\234\345\214\272\351\227\256\351\242\230\350\257\212\346\226\255\344\270\216\346\226\271\346\241\210.md"
M  "plan/\347\254\254\344\270\200\345\221\250\351\200\220\346\227\245\344\273\273\345\212\241\350\241\250.md"
```
↑ 完全看不懂是哪个文件 😵

**设为 `false` 之后**：
```
D  plan/工作区问题诊断与方案.md
M  plan/第一周逐日任务表.md
```
↑ 一眼看清 ✧*｡٩(ˊᗜˋ*)و✧*｡

#### 原理

Git 早期对非 ASCII 字符（中文、日文、emoji）不友好，默认行为是：
> 遇到非英文字符就用**八进制转义**表示，并用引号包起来。

比如「工」的 UTF-8 是 `E5 B7 A5`，八进制是 `345 267 245`，Git 就显示成 `\345\267\245`。

`quotepath = false` 告诉 Git：别转义了，直接按 UTF-8 显示原字符。

#### 重要：这只影响「显示」

**不影响**：
- 文件本身的名字（一直是正常中文）
- 提交到 GitHub 的文件名（一直是正常中文）
- 代码内容

纯粹是终端里的**显示样式**。

#### 建议

✅ **强烈建议设置**，因为主人的文件全是中文名（`第一周逐日任务表.md` 这种），不设的话 `git status`、`git diff`、`git log` 全是乱码，容易看错文件名提交错东西。

---

### 7.3 `core.autocrlf` —— 换行符自动转换（Windows 默认已开）

```powershell
git config --global core.autocrlf true
```

**⚠️ 注意**：Windows 版 Git **安装时就默认设成 `true` 了**（主人机器实测：`git config --global core.autocrlf` 输出 `true`）。
所以这条命令对主人来说**是"确认"而不是"新设"**。

#### 作用

| 时机 | 做什么 |
|---|---|
| **提交时** | 把 Windows 的 `CRLF` 转成 `LF` 存进仓库 |
| **检出时** | 把仓库的 `LF` 转回 `CRLF` 放到你磁盘上 |

#### 为什么要这么麻烦

不同系统的换行符不一样：

| 系统 | 换行符 | 字节 |
|---|---|---|
| Windows | `CRLF` | `0D 0A`（回车+换行） |
| Linux / macOS | `LF` | `0A`（只有换行） |

如果 Windows 和 Linux 用户一起改同一个文件，Git 会认为"**每一行都变了**"，代码审查没法看 —— 明明只改一行却显示整个文件全变了。

#### 实测证据（主人机器上的 hello.js）

**工作区文件**（磁盘上）：
```
... 29 0D 0A 63 6F 6E ...
       ↑↑
       CR LF  ← Windows 风格
```

**仓库里的文件**（提交进去的）：
```
... 29 0A 63 6F 6E ...
       ↑
       LF  ← Unix 风格
```

**完美验证**：提交时 CRLF → LF 的转换真的发生了。

#### 三种取值

| 值 | 提交时 | 检出时 | 适合谁 |
|---|---|---|---|
| **`true`** | CRLF → LF | LF → CRLF | ✅ **Windows 用户（主人）** |
| `input` | CRLF → LF | 不转换 | Linux/Mac 用户，或 Windows 上跑 WSL |
| `false` | 不转换 | 不转换 | 不推荐，容易乱 |

#### 建议

✅ **保持 `true` 就好**，不用改。

---

### 7.4 两条配置对比速查

| 配置 | 作用 | 主人当前状态 | 要设吗 |
|---|---|---|---|
| `core.quotepath` | 中文文件名是否转义显示 | 未设置（= `true`） | ✅ **要设成 false** |
| `core.autocrlf` | 换行符自动转换 | 已是 `true` | ✅ 不用动 |

**一次设完这两条**：
```powershell
git config --global core.quotepath false
git config --global core.autocrlf true
```

**验证**：
```powershell
git config --global --list | Select-String "core"
```

---

---

## 八、`.gitignore` —— Git 的「黑名单」

### 8.1 它是干什么的（一句话）

**告诉 Git：「这些文件你不用管，装看不见。」**

文件名字拆开看：
- **git** = Git
- **ignore** = 忽略
- 合起来 = **Git 的忽略清单**

---

### 8.2 为什么需要它

Git 有个特点：**默认会跟踪文件夹里的所有文件**。

假设项目长这样：

```
D:\code\
├── hello.js
├── node_modules\        ← 巨大！几千个文件夹、几百 MB
├── .env                 ← 里面有数据库密码、API 密钥！
└── debug.log            ← 日志文件，天天变
```

如果直接敲 `git add .`，Git 会**把上面所有东西全塞进去**：

| 文件 | 问题 |
|---|---|
| `node_modules/` | 几万个文件、几百 MB，GitHub 单文件限 100MB，直接拒绝 |
| `.env` | ⚠️ **里面是密码、密钥**，传上去就泄露了 |
| `debug.log` | 日志天天变，传了没意义，还占空间 |

`.gitignore` 就是来解决这个问题的。

---

### 8.3 核心价值（三条）

1. **防止仓库爆炸** —— `node_modules` 几万个文件，传上去仓库就废了
2. **防止泄密** —— `.env` 里有数据库密码、API 密钥，这是最危险的
3. **防止噪音** —— 日志、缓存、编辑器配置，天天变，传了没意义

---

### 8.4 为什么 `.gitignore` 自己必须被提交

这是最容易想不通的一点：

> **`.gitignore` 是给「所有一起写代码的人」看的规矩，不是只给你自己看的。**

想象一下：

```
你：提交代码时自动忽略了 node_modules ✅
同事：clone 你的仓库，但他没有 .gitignore
      → 他一敲 git add .，node_modules 就传上去了
      → 仓库瞬间爆炸 💥
```

所以 `.gitignore` **必须提交**，这样：
- 谁 clone 这个项目，就自动继承这套规矩
- 大家行为一致，仓库不会乱

**结论**：把 `.gitignore` 传到 GitHub 是**完全正确**的做法，全世界所有正经项目都这么做。

---

### 8.5 语法规则

| 写法 | 含义 | 例子 |
|---|---|---|
| `文件名` | 忽略这个文件 | `.env` |
| `文件夹名/` | 忽略整个文件夹 | `node_modules/` |
| `*.后缀` | 忽略所有这个后缀的文件 | `*.log` |
| `*` | 通配任意字符 | `temp*` |
| `**/xxx` | 任意层级下匹配 | `**/dist` |
| `!开头` | **反悔**，不忽略 | `!.vscode/extensions.json` |
| `#开头` | 注释 | `# 这是注释` |
| `/开头` | 从仓库根目录开始算 | `/config.json` |

**`!` 的用法**（重要）：先忽略一大片，再把其中一个捞回来。

```gitignore
.vscode/*                    # 忽略 .vscode 下所有文件
!.vscode/extensions.json     # 但 extensions.json 要保留（团队共享推荐插件）
```

**`/` 结尾的区别**（容易混）：

```gitignore
node_modules/     # 忽略所有叫 node_modules 的文件夹（任意层级）
node_modules      # 忽略叫 node_modules 的文件或文件夹
/dist             # 只忽略根目录下的 dist，不忽略 src/dist
```

**新手记这条就够**：**文件夹加 `/`，文件不加**。

---

### 8.6 ⚠️ 最大的坑：已跟踪的文件，忽略无效

**这是新手最容易踩的坑，务必记住：**

```
如果某个文件已经被 git add 过了，
之后再把它写进 .gitignore，
Git 会继续跟踪它！
```

**为什么**：`.gitignore` 只管「**还没被跟踪**」的文件。已经被 Git 管起来的文件，忽略清单管不着。

**错误示范**：
```powershell
git add .env                    # ① 先把密码文件加进去了（糟糕）
echo ".env" >> .gitignore       # ② 再想忽略它
git status                      # ③ .env 还在里面！忽略失败
```

**正确补救**：
```powershell
git rm --cached .env            # 从 Git 移除，但本地文件保留
git commit -m "移除误提交的 .env"
```

**注意**：`--cached` 是关键，不加的话会**连本地文件一起删掉**。

**主人现在不用担心** —— 因为 `.gitignore` 在第一次 `git add` **之前**就建好了，规矩从头生效 ✅

---

### 8.7 怎么验证忽略规则生效了

**方法 1：直接问 Git**（最准确）

```powershell
git check-ignore -v 文件名
```

**有输出** = 被忽略了，并且会告诉你**是哪一行规则拦的**：
```
.gitignore:15:*.log     debug.log
```

**没输出** = 没被忽略（正常跟踪）。

**方法 2：看 `git status`**

被忽略的文件**根本不会出现**在 `git status` 里。

**方法 3：看全部被忽略的文件**

```powershell
git status --ignored
```

---

### 8.8 主人现在这份 `.gitignore` 逐条解释

```gitignore
# 依赖包目录（体积巨大，绝对不要提交）
node_modules/          ← npm 装的所有包，几万个文件，必须忽略

# 构建产物
dist/                  ← 打包后的文件，能重新生成，不用传
build/
.vite/

# 环境变量（里面可能有密码、密钥，绝对不能提交！）
.env                   ← ⚠️ 最重要的一条！密码都在这
.env.local
.env.*.local

# 日志
*.log                  ← 日志天天变，传了没意义

# 编辑器/系统杂项
.DS_Store              ← Mac 的系统文件
Thumbs.db              ← Windows 的缩略图缓存
.idea/                 ← JetBrains 编辑器的配置

# VS Code 个人配置
.vscode/*
!.vscode/extensions.json   ← 但推荐扩展列表要传，团队共享

# 缓存
.cache/
*.tmp
```

**实测验证**（2026-10-18 用 `git check-ignore` 跑的）：

| 路径 | 结果 | 被哪条规则拦的 |
|---|---|---|
| `node_modules\abc\index.js` | ❌ 忽略 | 第 2 行 `node_modules/` |
| `.env` | ❌ 忽略 | 第 10 行 `.env` |
| `debug.log` | ❌ 忽略 | 第 15 行 `*.log` |
| `hello.js` | ✅ 跟踪 | 没被拦 |
| `notes\git-commands.md` | ✅ 跟踪 | 没被拦 |

---

### 8.9 通用模板（以后新项目直接抄）

**前端 / Node.js 项目**：

```gitignore
# 依赖
node_modules/

# 构建产物
dist/
build/
.vite/
.next/
out/

# 环境变量（最重要！）
.env
.env.local
.env.*.local

# 日志
*.log
npm-debug.log*
yarn-debug.log*
pnpm-debug.log*

# 编辑器 / 系统
.DS_Store
Thumbs.db
.idea/
*.swp
.vscode/*
!.vscode/extensions.json

# 缓存
.cache/
*.tmp
```

**Python 项目**：

```gitignore
__pycache__/
*.py[cod]
.venv/
venv/
*.egg-info/
.pytest_cache/
.mypy_cache/
.env
```

**Java 项目**：

```gitignore
*.class
target/
*.jar
*.war
.idea/
*.iml
```

---

### 8.10 创建 `.gitignore` 的时机

**最佳时机**：`git init` 之后、第一次 `git add` 之前。

**顺序**：
```powershell
git init                  # ① 建仓库
# ② 写 .gitignore（一定要在 add 之前！）
git add .
git commit -m "初始化项目"
```

**如果忘了写就 `git add` 了**：用 8.6 的 `git rm --cached` 补救。

---

### 8.11 速查总结

| 问题 | 答案 |
|---|---|
| `.gitignore` 是什么 | Git 的忽略清单（黑名单） |
| 要提交到 GitHub 吗 | ✅ **必须提交** |
| 传上去有影响吗 | ❌ 零影响，是标准做法 |
| 已跟踪的文件能忽略吗 | ❌ 不能，要用 `git rm --cached` |
| 文件夹怎么忽略 | `文件夹名/`（加斜杠） |
| 文件怎么忽略 | `文件名`（不加斜杠） |
| 怎么验证生效 | `git check-ignore -v 文件名` |
| 什么时候建 | `git init` 后、`git add` 前 |

---

## 九、GitHub 的两种认证方式

### HTTPS（主人现在用的）
```powershell
git remote add origin https://github.com/用户名/仓库.git
```
- 需要 **PAT 令牌**当密码
- 简单，适合新手
- 凭据可以存在 Windows 凭据管理器

### SSH（以后可以学）
```powershell
git remote add origin git@github.com:用户名/仓库.git
```
- 用密钥对认证，不用输密码
- 要先配置 SSH key
- 更安全，适合长期用

**新手先用 HTTPS**，等熟悉了再学 SSH。

---

## 十、复习自测

合上笔记，看能不能回答：

- [ ] `git init` 做了什么？
- [ ] `git status` 里 `Untracked` 和 `Changes not staged` 有什么区别？
- [ ] `git add .` 里的 `.` 是什么意思？
- [ ] `git commit -m` 的 `-m` 是什么的缩写？
- [ ] `git push -u` 里的 `-u` 有什么用？
- [ ] `origin` 是什么？为什么叫这个名字？
- [ ] 推代码时密码栏该填什么？
- [ ] `error: src refspec main does not match any` 是什么原因？
- [ ] `git log --oneline` 和 `git log` 有什么区别？
- [ ] 你的 GitHub 用户名是什么？
- [ ] `.gitignore` 是干什么的？为什么它自己要被提交？
- [ ] 已经被 `git add` 过的文件，还能用 `.gitignore` 忽略吗？怎么补救？
- [ ] 文件夹和文件的忽略写法有什么区别？
- [ ] 怎么验证某条忽略规则生效了？

**答不上来的，回去翻对应章节～**
