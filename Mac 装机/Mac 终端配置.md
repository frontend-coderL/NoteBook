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

* * *

## **2 iTerm2 或 Ghostty**

### 2.1 **iTerm2**

官网：[iTerm2 - macOS Terminal Replacement](https://iterm2.com/)

特点

-   功能完整
    
-   配置丰富
    
-   生态成熟
    

安装命令

```Shell
brew install --cask iterm2
```

常用快捷键：

-   `CMD` + `D`：横向分屏
    
-   `CMD` + `Shift` + `D`：纵向分屏
    
-   `CMD` + `W`：关闭当前分屏
    

### 2.2 **Ghostty**

官网：[Ghostty](https://ghostty.org/)

特点

-   性能更轻
    
-   启动速度快
    
-   界面更现代
    

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
# 允许字体缺失时自动合成粗体/斜体，防止出现方块或显示异常
font-synthetic-style = bold,italic,bold-italic
# 连字优化：在光标经过时暂时断开连字（比如把 != 变回 ! 和 =），方便精准定位修改
font-shaping-break = cursor

adjust-cell-height = 15%
# 让 Nerd Font 图标变大一点，视觉上更协调
adjust-icon-height = 15%

# 透明度与毛玻璃效果
background-opacity = 0.85
background-blur-radius = 30

# 光标样式：bar（条状），也可以设为 block（块状）
cursor-style = bar
cursor-opacity = 0.8

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

# ===== 颜色主题 (Tokyo Night) =====
foreground = "#c0caf5"
background = "#1a1b26"

# 调色板映射（0-15 号色）
palette = 0=#15161e
palette = 1=#f7768e
palette = 2=#9ece6a
palette = 3=#e0af68
palette = 4=#7aa2f7
palette = 5=#bb9af7
palette = 6=#7dcfff
palette = 7=#a9b1d6
palette = 8=#414868
palette = 9=#f7768e
palette = 10=#9ece6a
palette = 11=#e0af68
palette = 12=#7aa2f7
palette = 13=#bb9af7
palette = 14=#7dcfff
palette = 15=#c0caf5

# ===== 性能与限制 =====
scrollback-limit = 100000

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
shell-integration-features = cursor,sudo,title
# 通知等级。任何任务结束都会提醒
notify-on-command-finish = always

# ===== 自定义快捷键 =====
# 注意：Ghostty 默认已经映射了 cmd+c/v，如果想手动强制覆盖可保留
keybind = cmd+k=clear_screen
keybind = cmd+t=new_window
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

* * *

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

-   git：官方插件，默认开启，提供Git命令别名
    
-   sudo：官方插件，使用快捷键 ESC ESC，在当前命令前快速插入 sudo
    
-   z：官方插件，跟踪最常访问的目录，能够通过输入所需目录路径中的几个字符来快速访问
    

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

-   zsh-autosuggestions：三方插件，自动补全插件，根据历史命令和当前输入内容，自动推荐可能的命令
    
-   zsh-syntax-highlighting：三方插件，语法高亮插件
    

## 4 其他工具

### 4.1 Starship

作用：精简、快速、可定制的 shell 提示符

安装插件

```Shell
brew install starship
```

以下预设可选或选一

```Shell
starship preset catppuccin-powerline -o ~/.config/starship.toml
starship preset tokyo-night -o ~/.config/starship.toml
starship preset gruvbox-rainbow -o ~/.config/starship.toml
```

部分终端可能不支持该预设的图标，需要安装Nerd Font字体并手动设置，推荐字体：`JetBrainsMono Nerd Font`

```Shell
brew install --cask font-jetbrains-mono-nerd-font
```

### 4.2 fastfetch

作用：获取系统信息并以美观的方式呈现，更快更现代的 `Neofetch`

安装插件

```Shell
brew install fastfetch
```

运行：`fastfetch`

### 4.3 fzf 模糊搜索

作用：通用命令行模糊搜索工具

安装插件

```Shell
brew install fzf
/opt/homebrew/opt/fzf/install
```

使用：

-   Ctrl + R：历史搜索增强
    
-   Ctrl + T：文件搜索
    
-   Alt + C：目录跳转
    
-   Ctrl + C：退出
    

# **6 后续**

-   zsh 优化（性能 + 启动速度）
    
-   cmux
    

![image.png](https://cdn2.flowus.cn/oss/aaa28365-c965-43cd-bb75-89d6c3062fad/image.png?time=1776701700&token=f9a643cb9b93b516f3f18db32b45f577709d5de084cc72656d1420e027662987&role=sharePaid)