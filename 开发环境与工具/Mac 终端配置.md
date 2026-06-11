## 1 Homebrew

Homebrew 是 macOS 的包管理工具，用于安装命令行工具和应用。

```Shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

初始化配置

```Shell
cat >> ~/.zprofile <<'EOF'
# Homebrew 初始化
eval "$(/opt/homebrew/bin/brew shellenv)"

# Homebrew USTC 国内镜像
export HOMEBREW_BOTTLE_DOMAIN="https://mirrors.ustc.edu.cn/homebrew-bottles"
export HOMEBREW_API_DOMAIN="https://mirrors.ustc.edu.cn/homebrew-bottles/api"
EOF

source ~/.zprofile
```

常用命令

```Shell
# 查看配置
brew config

# 更新 Homebrew 元数据
brew update

# 搜索软件
brew search <keyword>

# 查看信息
brew info <formula>

# 已安装列表
brew list

# 升级所有软件
brew upgrade

# 升级指定软件
brew upgrade <formula>

# 清理旧版本
brew cleanup

# 强力清理（推荐）
brew cleanup --prune=all

# 卸载软件
brew uninstall <formula>

# 安装 CLI 工具
brew install <formula>

# 安装 GUI 应用
brew install --cask <app>
```

---



## **2 iTerm2 或 Ghostty**

### 2.1 **iTerm2**

官网：[iTerm2 - macOS Terminal Replacement](https://iterm2.com/)

特点

- 功能完整

- 配置丰富

- 生态成熟

安装命令

```Shell
brew install --cask iterm2
```

常用快捷键：

- `CMD` + `D`：横向分屏

- `CMD` + `Shift` + `D`：纵向分屏

- `CMD` + `W`：关闭当前分屏

### 2.2 **Ghostty**

官网：[Ghostty](https://ghostty.org/)

特点

- 性能更轻

- 启动速度快

- 界面更现代

安装命令

```Shell
brew install --cask ghostty
```

查看配置命令

```Shell
# 显示 Ghostty 所有配置项 + 默认值
ghostty +show-config --default

# 查看当前生效的配置包含你的修改
ghostty +show-config

# 查看默认快捷键
ghostty +list-keybinds --default
```

配置

```Shell
# ===== 基础显示 =====
font-family = "JetBrainsMono Nerd Font"
font-size = 14
# 连字优化：在光标经过时暂时断开连字（比如把 != 变回 ! 和 =），方便精准定位修改
font-shaping-break = cursor

# 行高
# adjust-cell-height = 15%
# 强制 emoji 用 Apple 原生
font-codepoint-map = "U+1F300-U+1F5FF=Apple Color Emoji"

# 透明度与毛玻璃效果
background-opacity = 0.85
background-blur = 15

# 光标样式：bar（条状），也可以设为 block（块状）
cursor-style = bar
cursor-opacity = 0.8
# 光标颜色
cursor-color = green

# 温润色调主题
theme = Catppuccin Mocha
# 透明标题栏。移除窗口顶部的灰色横条，让红绿灯按钮直接“悬浮”在背景上
macos-titlebar-style = transparent
# 自动主题。让终端边框颜色能随系统深浅模式自动切换
window-theme = auto

# ===== 窗口布局 =====
# 内边距，让文字离边框有点距离，视觉更舒适
window-padding-x = 10
window-padding-y = 10

# ===== 性能与限制 =====
scrollback-limit = 800000
# 滚动行为
scroll-to-bottom = keystroke,no-output

# ===== macOS 特有优化 =====
# 将 Option 键映射为 Alt，解决 Zsh/Emacs/Vim 的快捷键冲突
macos-option-as-alt = true

# ===== 窗口管理优化 =====
# 关闭最后一个窗口后彻底退出程序，不留后台进程
quit-after-last-window-closed = true
# 记住上次关闭时的窗口尺寸和位置
window-save-state = always

# ===== 行为设置 =====
copy-on-select = clipboard
# 粘贴保护。防止网页上带恶意指令的代码在粘贴时被直接触发执行
clipboard-paste-protection = true
clipboard-paste-bracketed-safe = true
# 关闭窗口时不弹出确认对话框
confirm-close-surface = false

# 自动注入 Shell 集成脚本，增强光标和标题的感知
shell-integration = detect
shell-integration-features = cursor,sudo,title,path

# 通知，只有终端不在前台时才通知
notify-on-command-finish = unfocused
notify-on-command-finish-after = 10s

# ===== 自定义快捷键 =====
# 注意：Ghostty 默认已经映射了 cmd+c/v，如果想手动强制覆盖可保留
keybind = cmd+k=clear_screen
keybind = cmd+d=new_split:right
keybind = cmd+shift+d=new_split:down

# 类似 iTerm2 的热键窗口，按 Cmd + ` 瞬间从顶部滑出一个终端，随用随走
keybind = global:cmd+grave_accent=toggle_quick_terminal
quick-terminal-position = top
quick-terminal-size = 40%
quick-terminal-autohide = true
# 快速终端在“鼠标所在”的屏幕弹出
quick-terminal-screen = mouse
```

---



## **3 Oh My Zsh**

### **3.1 安装 Oh My Zsh**

```Shell
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

### **3.2 配置 zsh 插件**

编辑配置文件

```Shell
nano ~/.zshrc
```

修改 plugins 配置

```Shell
plugins=(git sudo z)
```

- git：官方插件，默认开启，提供Git命令别名

- sudo：官方插件，使用快捷键 ESC ESC，在当前命令前快速插入 sudo

- z：官方插件，跟踪最常访问的目录，能够通过输入所需目录路径中的几个字符来快速访问

**生效配置**

```Shell
source ~/.zshrc
```

### 3.3 三方插件

```Shell
git clone https://github.com/zsh-users/zsh-autosuggestions \
${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

git clone https://github.com/zsh-users/zsh-syntax-highlighting \
${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

plugins=(git sudo z zsh-autosuggestions zsh-syntax-highlighting)
```

- zsh-autosuggestions：三方插件，自动补全插件，根据历史命令和当前输入内容，自动推荐可能的命令

- zsh-syntax-highlighting：三方插件，语法高亮插件

## 4 其他工具

### 4.1 Starship

- 作用：精简、快速、可定制的 shell 提示符

- 安装：`brew install starship`

- 可选预设

    ```Shell
    starship preset catppuccin-powerline -o ~/.config/starship.toml
    starship preset tokyo-night -o ~/.config/starship.toml
    starship preset gruvbox-rainbow -o ~/.config/starship.toml
    ```

    部分终端可能不支持该预设的图标，需要安装Nerd Font字体并手动设置，推荐字体：`JetBrainsMono Nerd Font`

### 4.2 fastfetch

- 作用：获取系统信息并以美观的方式呈现，更快更现代的 `Neofetch`

- 安装：`brew install fastfetch`

- 运行：`fastfetch`

### 4.3 fzf 模糊搜索

- 作用：通用命令行模糊搜索工具

- 安装：`brew install fzf`

- 使用：

    - `Ctrl` + `R`：历史搜索增强

    - `Ctrl` + `T`：文件搜索

    - `Alt` + `C`：目录跳转

    - `Ctrl`+ `C`：退出

### 4.4 [eza](https://github.com/eza-community/eza)

- 作用：现代的 ls 替代方案

- 安装：`brew install eza`

- 配置：

    ```Shell
    # eza aliases
    alias ls="eza"
    alias ll="eza -l --group-directories-first --icons"
    
    source ~/.zshrc
    ```

### 4.5 [Yazi](https://github.com/sxyazi/yazi)

- 作用：Rust 编写的终端文件管理器，全异步 I/O，启动约 1ms，二进制 5MB，38.6k Star。
- 安装：`brew install yazi`
- 核心能力：
    - 全异步非阻塞 I/O，读文件 / 渲染 / 输入并行不互斥
    - 文件预加载：光标移到时即预读图片解码、语法高亮、文件类型识别
    - 内联预览：图片、PDF、视频缩略图（需终端支持：kitty / iTerm2 / WezTerm / Ghostty）
    - Vim 风格按键：j/k 移动、l 进入、h 返回、空格选中、y 复制、p 粘贴
    - 内置集成：ripgrep 搜索、fd 查找、fzf 模糊搜索、zoxide 快速跳转
    - 支持拖放文件、批量重命名、一键解压、Lua 插件系统
    - 主题：`ya pack -a yazi-rs/flavors#catppuccin-mocha`
- 同类对比：比 ranger（Python）快很多，比 lf（Go）功能更全，比 nnn（C）学习曲线更友好。

## 5 Fish Shell

官网：[fishshell.com](https://fishshell.com/)

Fish（Friendly Interactive Shell）主打开箱即用，把 zsh 需要装三四个插件才能实现的能力全部内置了。和 Oh My Zsh 是二选一的关系，不是叠加。

### 5.1 安装与切换

```Shell
# 安装
brew install fish

# 设置为默认 shell
chsh -s /usr/local/bin/fish

# 切回 zsh
chsh -s /bin/zsh
```

### 5.2 核心优势

- 智能自动补全：打几个字母就猜到完整命令，按右方向键直接补上。fastfetch 长参数、rg + fzf 管道命令都能补回来。
- 实时语法高亮：打错命令实时变红，还没按回车就知道拼错了。
- Tab 补全：自带 1000+ 工具的补全规则，git 子命令后面都带说明。
- 以上能力在 zsh 上要装 zsh-autosuggestions + zsh-syntax-highlighting 等三四个插件，Fish 全是内置的。

### 5.3 缩写系统 abbr

Fish 独有的缩写机制，打 `gst` 按空格自动展开成 `git status`，历史记录里存的是完整命令，翻历史不会一脸懵。

```Fish
# 定义缩写
abbr -a gst git status
abbr -a gco git checkout
abbr -a gc git commit
abbr -a gp git push
abbr -a gl git log --oneline --graph
```

### 5.4 bash 兼容性

- bash 脚本在 Fish 里直接跑，系统看的是第一行的 shebang，跟用什么 shell 没关系。
- 网上复制的命令大部分能直接用，碰上不行的 `bash -c '...'` 一包就行。
- zshrc 配置不用手动翻译，丢给 Claude Code 让它转成 config.fish，几秒钟搬完。

### 5.5 配置文件

Fish 配置文件位于 `~/.config/fish/config.fish`，语法和 zsh 不同，但更简洁。

```Fish
# 环境变量
set -gx EDITOR nvim
set -gx PATH $PATH /usr/local/bin

# alias
alias ll='eza -l --group-directories-first --icons'
alias cat='bat'

# 缩写
abbr -a gst git status
abbr -a gco git checkout
```

### 5.6 与 Oh My Zsh 的取舍

| 维度 | Oh My Zsh + zsh | Fish |
|------|----------------|------|
| 开箱即用 | 需装多个插件 | 内置全部能力 |
| 脚本兼容性 | 原生 bash 语法 | 语法不同，脚本需转换 |
| 生态插件 | 极其丰富 | 相对较少 |
| 配置复杂度 | 高（zshrc + 插件配置） | 低（单个 config.fish） |

适合人群：不想折腾插件配置、追求开箱即用的人。如果已经在 zsh 生态里配好了一套顺手的插件，没必要换。

# **6 后续**

- zsh 优化（性能 + 启动速度）

- cmux

![image.png](Mac+终端配置+3fe0fdae-f61c-4d74-b233-3405600badfd/image.png)

