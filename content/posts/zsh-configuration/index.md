---
title: configure zsh environment
description: install oh-my-zsh and configure new plugins and theme
slug: zsh
date: 2021-03-19 10:34:00+0000
image: daveverwer.jpg
categories:
  - zsh
tags:
  - terminal
---

## Install `oh-my-zsh` with curl
```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

## Enabling Plugins and Themes
### zsh-autosuggestions
 
```bash
 git clone https://github.com/zsh-users/zsh-autosuggestions.git $ZSH_CUSTOM/plugins/zsh-autosuggestions
```
 
### zsh-syntax-highlighting
 
 - download
```bash
 git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting
```

 - `nano ~/.zshrc` find `plugins=(git)`
 
 - Append `zsh-autosuggestions & zsh-syntax-highlighting` to  `plugins()` like this 
 
 `plugins=(git zsh-autosuggestions zsh-syntax-highlighting)`
 
### powerlevel10k 

  - download
```bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/themes/powerlevel10k
```
  - edit ~/.zshrc, add `ZSH_THEME="powerlevel10k/powerlevel10k"`

  - reopen terminal to configure p10k theme (or run `p10k configure`)

## Install `Nerd Fonts` (optional)

  - download
```bash
mkdir -p ~/.local/share/fonts
cd ~/.local/share/fonts && curl -fLo "Droid Sans Mono for Powerline Nerd Font Complete.otf" https://github.com/ryanoasis/nerd-fonts/raw/master/patched-fonts/DroidSansMono/complete/Droid%20Sans%20Mono%20Nerd%20Font%20Complete.otf
```

  - cache fonts
```bash
fc-cache -vf ~/.local/share/fonts/
```
  - check
```bash
fc-list | grep -i droid
```

  - set font for terminal

## Ref
> - [oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh)
> - [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)
> - [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)
> - [https://gist.github.com/kevin-smets/8568070](https://gist.github.com/kevin-smets/8568070)
 
