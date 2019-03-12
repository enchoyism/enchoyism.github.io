---
title: macOS + iTerm2 setting guide (feat zsh)
date: 2019-03-12 22:16:23
tags: [macOS, iTerm2]
---
macOS 기본 터미널을 대체하는 **[iTerm2](https://www.iterm2.com/)** 와 zsh를 이용하여 터미널을 세팅 하고, ls 명령어 사용시 파일 확장자별로 색을 입혀서 출력 시켜보도록 한다. macOS에서 사용되는 ls는 FreeBSD에서 사용되는 다른 버젼의 ls이다. 따라서 GNU ls를 사용하려면 별도로 설치를 해줘야 한다.

# iTerm2 + zsh

## Homebrew 설치
``` bash
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

## zsh 설치
``` bash
$ brew install zsh
```

## zsh 설치경로 확인
``` bash
$ which zsh
```

## 기본 shell 변경
``` bash
$ sudo chsh -s $(which zsh)
```

## oh-my-zsh 설치
``` bash
$ sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

## zsh theme 적용 (dracula)
> https://github.com/robbyrussell/oh-my-zsh/wiki/themes
> https://draculatheme.com/zsh/

``` plain
~/.oh-my-zsh/themes/dracula.zsh-theme 의 경로로 테마 파일 이동.
~/.zshrc => ZSH_THEME="{테마}"
```

# GNU ls

## coreutils 설치
```
$ brew install coreutils
```

## GNU theme clone
```
$ cd ~ && git clone https://github.com/seebi/dircolors-solarized.git ~/.dircolors-solarized
```

## profile 추가 (~/.bash_profile or ~/.zsh_profile)
``` plain
alias ls='gls --color=auto'
alias dir='gdir --color=auto'
alias grep='grep --color=auto'
eval $(gdircolors ~/.dircolors-solarized/dircolors.ansi-dark)
```
``` bash
source {~/.bash_profile} OR {~/.zsh_profile}
```

# Result
![macos-iterm2-setting-1](/images/macos-iterm2-setting-1.png)
