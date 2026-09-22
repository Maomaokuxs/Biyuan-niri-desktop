# Biyuan-niri-desktop

Biyuan 的 niri 桌面环境配置发行仓库：开箱即用的 niri + quicksilver栏桌面。
通过 Fedora COPR (`biyuan/software`) 分发，替代旧的 install.sh / by-mgr 手工部署流程。

## 安装（新机器）

```bash
# 1. 启用第三方仓库（部分组件不在 Fedora 官方源）
sudo dnf copr enable -y eddievs/hyprland      # hyprlock / hypridle
sudo dnf copr enable -y biyuan/software       # 本配置包 + hellwal/waypaper

# 2. 安装桌面元包（会自动拉上 niri、quickshell、waybar、kitty……）
sudo dnf install -y biyuan-niri-desktop

# 3. 部署配置到家目录（已有文件先备份到 ~/.config-backup-<时间> 再覆盖）
biyuan-niri-deploy

# 4. 注销，在登录管理器选择 niri 会话进入
#    首次进入会自动执行壁纸初始化与配色同步
```

默认顶栏为 quickshell 定制栏；`Ctrl+Alt+B` 可一键切换 waybar/quickshell。

## 仓库结构

```text
dotfiles/            # stow 风格配置模块（niri/quickshell/waybar/mako/rofi/kitty/…）
wallpapers/          # 默认壁纸库（部署时软链接到 ~/Pictures/wallpapers）
packaging/
  biyuan-niri-deploy # 部署脚本（→ /usr/bin/）
```

## 可移植补丁说明（相对原 Biyuan-Fedora-Learning 的改动）

原仓库只读引用，不做任何修改；以下仅作用于本仓库的副本：

1. `dotfiles/niri/.../scripts/init-wallpaper.sh`：壁纸源优先用
   `/usr/share/biyuan-niri-desktop/wallpapers`；theme-sync 优先找脚本同目录。
2. `dotfiles/quickshell/.../scripts/check-colors.py`：检查根目录改为脚本
   所在位置，不再硬编码 `/home/biyuan/Documents/quickshell`。
3. `dotfiles/hypr/.../hyprlock.conf`：**未收录**。该文件由 theme-sync.sh
   按本机显示器动态生成，首次配色同步时自动产生。

## 许可

MIT（与上游 Biyuan-Fedora-Learning 一致）。
各应用本体的许可归各自上游所有。
