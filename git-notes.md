#  Git 与 Linux 终端实战避坑全指南

本指南总结了从零开始搭建环境、在 Linux 终端操作、使用 Git 管理本地代码，以及将代码同步到 GitHub 的完整知识体系。

---

## 第一部分：Linux 终端核心命令（WSL Ubuntu）

在 WSL (Ubuntu) 环境下，以下命令是日常开发和文件管理的基础。

### 1. 文件与目录操作
- `pwd`：显示当前所在的绝对路径。
- `ls`：列出当前目录下的文件。
- `ls -ah`：列出所有文件，包括以 `.` 开头的隐藏文件（如 `.git` 目录）。
- `mkdir <文件夹名>`：创建一个空文件夹（如 `mkdir learngit`）。
- `cd <文件夹名>`：进入该文件夹（如 `cd learngit`）。
- `cd ~`：直接回到 Linux 用户主目录。
- `touch <文件名>`：快速创建一个空文件（如 `touch test.txt`）。
- `cat <文件名>`：在终端里直接查看该文件的全部内容。

### 2. 文本写入与编辑
- `echo "内容" > 文件名`：创建文件并写入一行内容（会覆盖原有内容）。
- `echo "内容" >> 文件名`：在文件末尾追加内容（不会覆盖原内容）。
- `nano <文件名>`：在终端内打开简单的文本编辑器。
  - 保存：按 `Ctrl + O`，然后按 `Enter` 确认。
  - 退出：按 `Ctrl + X`。

### 3. WSL 与 Windows 的交互
- `code .` 或 `code <文件名>`：唤起 Windows 下的 VS Code 打开当前目录或指定文件。
- `explorer.exe .`：用 Windows 资源管理器打开当前 Linux 目录。

### 4. 终端急救与快捷键
- `Ctrl + C`：**强行中断当前命令**。当终端出现 `>` 提示符卡住，或程序死循环时使用。
- `↑` (上方向键)：调出上一条输入过的命令。
- `Tab` 键：**自动补全**。输入文件名的前几个字母按 Tab，系统会自动补全全名。
- `Ctrl + Shift + V`：在 Windows 终端中粘贴内容。

---

## 第二部分：Git 核心工作流（本地操作）

Git 是一个版本控制系统，用于记录文件的每一次修改。

### 1. 环境检查与配置
- `git --version`：检查是否已安装 Git 及版本号。
- `git config --global user.name "你的名字"`：设置全局用户名。
- `git config --global user.email "你的邮箱"`：设置全局邮箱（**必须与 GitHub 邮箱一致**）。
- `git config --global --list`：检查全局配置是否设置成功。

### 2. 初始化仓库
- `git init`：**核心操作**。将当前目录变成 Git 可以管理的仓库，会生成隐藏的 `.git` 文件夹。**千万不要手动修改 `.git` 里的内容**。

### 3. Git 核心“三步曲”（最常用）
- `git status`：**查看状态**。红色代表未追踪/已修改，绿色代表已放入暂存区准备提交。
- `git add <文件名>`：**把文件放入暂存区**（如 `git add readme.txt`）。
- `git add .`：把当前目录下**所有**修改过的文件都放入暂存区。
- `git commit -m "提交说明"`：**提交到本地仓库**。将暂存区的内容正式存档，`-m` 后面是备注。

### 4. 查看与撤销（时光机）
- `git log`：查看历史提交记录（按 `q` 键退出，注意不要敲回车）。
- `git restore <文件名>`：撤销工作区中尚未 `add` 的修改。
- `git reset --hard <哈希值>`：强制回退到某一次的历史提交状态（**慎用**，会清空当前未提交的修改）。

---

## 第三部分：连接 GitHub（云端同步）

### 1. 建立连接
- 在 GitHub 创建**空仓库**（绝对不要勾选 Add a README）。
- `git remote add origin <你的仓库地址>`：将本地仓库与 GitHub 远程仓库绑定。
- `git branch -M main`：将默认分支重命名为 main（符合现代标准）。

### 2. 推送代码
- `git push -u origin main`：第一次推送并绑定上游分支。
- `git push`：以后再次推送，只需敲这两个词即可。

### 3. GitHub 认证机制（Personal Access Token）
- GitHub 不支持网页登录密码。
- 必须在 GitHub 的 `Settings -> Developer settings -> Personal access tokens (classic)` 生成一个包含 `repo` 权限的 Token。
- 当终端提示输入 `Password` 时，粘贴这串 Token（**终端屏幕上不会显示任何字符**）。

### 4. VS Code 图形化操作
- 打开左侧“源代码管理”面板。
- 写提交信息，点击“提交”（✔️）。
- 点击“同步更改”即可自动推送，无需在终端敲命令。

---

##  第四部分：实战踩坑与避坑指南

1. **VS Code 被 Cursor 劫持（`code` 命令无效）**：
   - 原因：Cursor 抢占了系统 PATH 变量或 WSL 别名。
   - 解决：找到 Cursor 的 `codeBin` 目录，删除或重命名里面的 `code` 文件。或者在 Windows 中重新打开 VS Code，按 `Ctrl+Shift+P` 执行 `Shell Command: Install 'code' command in PATH`。
   - 如果路径冲突，可以在 `~/.bashrc` 末尾手动添加 `export PATH="$PATH:/mnt/d/.../Microsoft VS Code/bin"`。

2. **WSL 安全警告弹窗（`wsl.localhost was not found`）**：
   - 这是 VS Code 的正常安全机制，直接点击 **`Allow` (允许)**，并勾选“永久允许”即可。

3. **编码与记事本问题**：
   - 千万不要用 Windows 自带的“记事本”编写代码或文本。
   - 记事本会在文件开头偷偷加上不可见的 BOM（`0xefbbbf`），导致 Linux 系统或编译器报错（如“第一行出现一个 ?”）。
   - **推荐使用 VS Code 或 Linux 终端创建文件**。

4. **引号与空格问题**：
   - 配置 Git 时：`git config --global user.name"名字"` 是错的，`user.name` 和双引号之间**必须有空格**。
   - 卡在 `>` 提示符：通常是少敲了结尾的引号，按 `Ctrl + C` 强制退出重来。

5. **路径格式问题**：
   - Linux 使用正斜杠 `/`，Windows 使用反斜杠 `\`。
   - 在 WSL 中访问 Windows 的 C 盘，路径是 `/mnt/c/Users/...`。