---
aliases:
  - Linux Installed Apps
tags:
  - Seed
references:
---
[[Linux]], [[Ricing|System Configuration]]
2026-07-20 19:49

## Installed Apps
- hyprland
- auto-cpufreq
- bibata-cursor-theme
- claude-code
- cloudflare-warp-bin
- cloudflared
- cups
- docker
- git
- git-fls
- github-cli
- hyprshade
- hyprsunset
- neovim
- cliphist
- dragon-drop
- yazi
- fastfetch
- hplip
- kitty
- nmap
- noto-fonts-emoji
- noto-fonts-jp-vf
- nss-mdns
- avahi
- obs-studio
- papirus-icon-theme
- ripgrep
- swayosd
- unityhub
- visual-studio-code-bin
- xdg-desktop-portal-termfilechooser
- xdg-desktop-portal-wlr
- yq
- jq
- zathura
- zen-browser-bin
- zsh
- spicetify-bin
- spicetify-marketplace-bin
- slurp
- imagemagick
- nvm
- obsidian
- pavucontrol
- lua-language-server
- lsp-plugins
- micromamba-bin
- mpris-scrobbler
- spotify

## Shell Config
```zsh
#=========================================================================
# Oh My Zsh & Plugins Setup
#=========================================================================
export ZSH="$HOME/.oh-my-zsh"
ZSH_THEME="robbyrussell"
ZSH_CUSTOM="$ZSH/custom"

# Install Oh-My-Zsh if it doesn't exist
if [ ! -d "$ZSH" ]; then
  echo "Installing Oh-My-Zsh..."
  git clone https://github.com/ohmyzsh/ohmyzsh.git "$ZSH" >/dev/null 2>&1
fi

# Auto-fetch necessary plugins if they don't exist
if [ ! -d "$ZSH_CUSTOM/plugins/zsh-autosuggestions" ]; then
  echo "Installing zsh-autosuggestions..."
  git clone https://github.com/zsh-users/zsh-autosuggestions "$ZSH_CUSTOM/plugins/zsh-autosuggestions" >/dev/null 2>&1
fi

if [ ! -d "$ZSH_CUSTOM/plugins/zsh-syntax-highlighting" ]; then
  echo "Installing zsh-syntax-highlighting..."
  git clone https://github.com/zsh-users/zsh-syntax-highlighting.git "$ZSH_CUSTOM/plugins/zsh-syntax-highlighting" >/dev/null 2>&1
fi

# NVM lazy loading config
export NVM_DIR="$HOME/.nvm"
zstyle ':omz:plugins:nvm' lazy yes
zstyle ':omz:plugins:nvm' lazy-cmd eslint prettier typescript vitest

plugins=(git zsh-autosuggestions zsh-syntax-highlighting nvm)

source $ZSH/oh-my-zsh.sh

#=========================================================================
# Aliases
#=========================================================================
alias caffeine="pkill hypridle && systemd-inhibit --what=handle-lid-switch --why="Temporary work override" sleep infinity"
alias ssh="kitty +kitten ssh"
rviz2() {
  xhost +local:docker
  docker run --rm -it \
    --net=host \
    --ipc=host \
    --env="DISPLAY=$DISPLAY" \
    --env="QT_X11_NO_MITSHM=1" \
    --env="ROS_DOMAIN_ID=42" \
    --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" \
    --device=/dev/dri \
    osrf/ros:humble-desktop \
    rviz2
}

#=========================================================================
# History Configuration
#=========================================================================
HISTFILE="$HOME/.zsh_history"
HISTSIZE=10000
SAVEHIST=10000

setopt HIST_IGNORE_ALL_DUPS
setopt APPEND_HISTORY
setopt INC_APPEND_HISTORY
setopt SHARE_HISTORY

#=========================================================================
# Custom Functions
#=========================================================================

# Automatically list directory contents upon changing directories
cd() {
  builtin cd "$@" && ls
}

# Dynamic Fastfetch with Matugen Colors
function fetch() {
    local color_file="$HOME/.config/hypr/scripts/quickshell/qs_colors.json"
    local config_path="/tmp/qs_fastfetch.jsonc"
    
    # Only rebuild the config if the Matugen colors changed or the config is missing
    if [ "$color_file" -nt "$config_path" ] || [ ! -f "$config_path" ]; then
        
        # Extract analogous cool tones
        local c_blue=$(grep -E '"blue"\s*:\s*"[^"]+"' "$color_file" 2>/dev/null | cut -d '"' -f 4)
        c_blue=${c_blue:-"#89b4fa"}
        
        local c_sapphire=$(grep -E '"sapphire"\s*:\s*"[^"]+"' "$color_file" 2>/dev/null | cut -d '"' -f 4)
        c_sapphire=${c_sapphire:-"#74c7ec"}
        
        local c_teal=$(grep -E '"teal"\s*:\s*"[^"]+"' "$color_file" 2>/dev/null | cut -d '"' -f 4)
        c_teal=${c_teal:-"#94e2d5"}
        
        local c_mauve=$(grep -E '"mauve"\s*:\s*"[^"]+"' "$color_file" 2>/dev/null | cut -d '"' -f 4)
        c_mauve=${c_mauve:-"#cba6f7"}
        
        local c_text=$(grep -E '"text"\s*:\s*"[^"]+"' "$color_file" 2>/dev/null | cut -d '"' -f 4)
        c_text=${c_text:-"#cdd6f4"}

        # Extract a full rainbow palette
        local palette_hexes=()
        for col in red peach yellow green sapphire mauve pink; do
            local val=$(grep -E "\"$col\"\s*:\s*\"[^\"]+\"" "$color_file" 2>/dev/null | cut -d '"' -f 4)
            case $col in
                red) val=${val:-"#f38ba8"} ;;
                peach) val=${val:-"#fab387"} ;;
                yellow) val=${val:-"#f9e2af"} ;;
                green) val=${val:-"#a6e3a1"} ;;
                sapphire) val=${val:-"#74c7ec"} ;;
                mauve) val=${val:-"#cba6f7"} ;;
                pink) val=${val:-"#f5c2e7"} ;;
            esac
            palette_hexes+=("$val")
        done

        # Convert the hex codes into a printable string of ANSI truecolor circles
        local palette_str=""
        for hex in "${palette_hexes[@]}"; do
            hex="${hex//\#/}" # Strip the hash
            local r=$((16#${hex:0:2}))
            local g=$((16#${hex:2:2}))
            local b=$((16#${hex:4:2}))
            palette_str+="\\\\e[38;2;${r};${g};${b}m● \\\\e[0m"
        done

        # Generate the dynamic Fastfetch configuration
        cat > "$config_path" <<EOF
{
  "\$schema": "https://github.com/fastfetch-cli/fastfetch/raw/master/doc/json_schema.json",
  "logo": {
    "source": "arch_small",
    "color": {
      "1": "$c_blue",
      "2": "$c_sapphire"
    },
    "padding": {
      "top": 1,
      "left": 2,
      "right": 3
    }
  },
  "display": {
    "separator": "  ",
    "color": {
      "separator": "$c_text"
    }
  },
  "modules": [
    "break",
    {
      "type": "title",
      "key": " usr",
      "keyColor": "$c_blue",
      "format": "{1}"
    },
    {
      "type": "os",
      "key": "󱄅 os ",
      "keyColor": "$c_blue"
    },
    {
      "type": "cpu",
      "key": " cpu",
      "keyColor": "$c_sapphire"
    },
    {
      "type": "memory",
      "key": "󰘚 ram",
      "keyColor": "$c_teal"
    },
    {
      "type": "shell",
      "key": " sh ",
      "keyColor": "$c_mauve"
    },
    "break",
    {
      "type": "command",
      "key": " ",
      "text": "echo -e '$palette_str'"
    },
    "break"
  ]
}
EOF
    fi

    # Run Fastfetch instantly using the cached config
    fastfetch -c "$config_path"
}

clear_screen_clean() {
    tput civis
    clear
    zle reset-prompt 2>/dev/null 
    tput cnorm
}

# Function to check terminal size and clear if too small
check_terminal_size() {
    tput cols | read WIDTH
    tput lines | read HEIGHT

    if [[ $WIDTH -le $MAX_WIDTH ]] || [[ $HEIGHT -le $MAX_HEIGHT ]]; then
        clear_screen_clean
    fi
}

#=========================================================================
# Execute on Startup
#=========================================================================

MAX_WIDTH=55
MAX_HEIGHT=10

tput cols | read WIDTH
tput lines | read HEIGHT

if [[ $WIDTH -gt $MAX_WIDTH ]] || [[ $HEIGHT -gt $MAX_HEIGHT ]]; then
    fetch
fi

trap check_terminal_size SIGWINCH

#=========================================================================
# Mamba Startup
#=========================================================================
export MAMBA_EXE='/usr/bin/micromamba';
export MAMBA_ROOT_PREFIX='/home/peace/.local/share/mamba';
__mamba_setup="$("$MAMBA_EXE" shell hook --shell zsh --root-prefix "$MAMBA_ROOT_PREFIX" 2> /dev/null)"
if [ $? -eq 0 ]; then
    eval "$__mamba_setup"
else
    alias micromamba="$MAMBA_EXE"  # Fallback on help from micromamba activate
fi
unset __mamba_setup

conda() {
    micromamba "$@"
}
mamba() {
    micromamba "$@"
}

#=========================================================================
# Secrets
#=========================================================================
source $HOME/.claude_openrouter_key
```