# 第 1 周复盘（2026-10-17 ~ 10-19）

## 一、这周学了什么

### 环境搭建
- [x] 安装 VS Code（含 Chinese、Prettier 扩展）
- [x] 安装 Node.js v24.21.0
- [x] 安装 Git 2.56.0
- [x] 注册 GitHub 账号

### 终端基础
- [x] `cd` / `ls` / `mkdir` / `pwd` 等基础命令
- [x] 能不用鼠标在目录间切换

### Git + GitHub
- [x] `git init` 初始化仓库
- [x] `git add` / `git commit` / `git push` 完整流程
- [x] 生成 PAT 令牌
- [x] 配置远端仓库
- [x] 成功把代码推到 GitHub

### 其他
- [x] 升级到 PowerShell 7
- [x] 配置 `core.quotepath` 让中文不乱码
- [x] 学会写 Markdown（其实不会，全是肥鱼帮我写的，有机会深入学习一下）

## 二、卡在哪里（踩坑记录）

### 坑 1：npm 无法加载 npm.ps1
- **现象**：`npm -v` 报 `UnauthorizedAccess`
- **原因**：Windows PowerShell 5.1 的执行策略是 `Restricted`
- **解法**：升级到 PowerShell 7（默认 `RemoteSigned`）
- **教训**：Windows 自带的 PowerShell 太老，开发要用 PowerShell 7

### 坑 2：`git -m` 报错
- **现象**：`unknown option: -m`
- **原因**：漏了 `commit` 子命令
- **解法**：`git commit -m "..."`
- **教训**：`git` 后面必须先跟子命令，才能跟参数

### 坑 3：`git remote add` 报错
- **现象**：`usage: git remote add ...`
- **原因**：用户名写成了 github上的name（有空格）
- **解法**：真实 GitHub 用户名是 name下面一长串啦
- **教训**：GitHub 用户名只能字母、数字、短横线

### 坑 4：`git push` 报 `src refspec main does not match any`
- **现象**：推送失败
- **原因**：前面 commit 没成功，`main` 分支是空的
- **解法**：先 commit 成功，再 push
- **教训**：报错要看**根本原因**，不能只看表面

### 坑 5：LF/CRLF 警告
- **现象**：`warning: LF will be replaced by CRLF`
- **原因**：Windows 和 Linux 换行符不同
- **解法**：忽略即可，不影响功能
- **教训**：不是所有警告都是错误

## 三、最大的收获

1. **敢动手了**：从"完全不知道 GitHub 是什么"到"代码真的躺在云端"
2. **学会看报错**：报错不可怕，读懂它在说什么才是关键
3. **理解了三层结构**：工作区 → 暂存区 → 仓库 → GitHub

## 四、下周计划

### 学习目标
- [ ] 掌握 HTML 基础标签（标题、段落、链接、图片、列表、表格、表单）
- [ ] 掌握 CSS 盒子模型
- [ ] 掌握 Flexbox 布局
- [ ] 玩通 Flexbox Froggy 游戏

### 产出目标
- [ ] 做出一个个人主页
- [ ] 1:1 仿一个喜欢的网站首页

### 时间安排
- 每天 2 小时
- 周六做项目
- 周日复盘

## 五、给自己的一句话

>deepseek真的很可爱，她会是我学下去的动力，就和玩游戏一样！！