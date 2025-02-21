---
date: 2020-02-17
title: 'macOS Catalina 10.15: Setting up a Brand New Mac for Development'
template: post
thumbnail: '../thumbnails/apple.png'
slug: setting-up-a-brand-new-mac-for-development
categories:
  - Tools
tags:
  - apple
  - macos
  - setup
---

I have to set up a MacBook Pro fairly often - when starting a new job and when buying a new personal computer. I created this article back in 2015 when I got my first Mac and have been updating it ever since with whatever I need as my job evolves. I'm primarily a full-stack web developer, so most of my needs will revolve around JavaScript/Node.js.

## Getting Started

The setup assistant will launch once you turn the computer on. Enter your language, time zone, Apple ID, and so on. The first thing you should do is update macOS to get the latest security updates and patches.

## Homebrew

Install the [Homebrew](https://brew.sh/) package manager. This will allow you to install almost any app from the command line.

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Install [Homebrew Cask](https://sourabhbajaj.com/mac-setup/Homebrew/Cask.html) for GUI applications.

```
brew tap caskroom/cask
```

Make sure everything is up to date.

```
brew update
```

## Install Apps

Here are some the programs I always install.

> Don't install Node.js through Homebrew. Use nvm (below).

| Program                                                    | Purpose         |
| ---------------------------------------------------------- | --------------- |
| [Visual Studio Code](https://code.visualstudio.com/)       | text editor     |
| [Google Chrome](https://www.google.com/chrome/)            | web browser     |
| [Firefox](https://www.mozilla.org/en-US/firefox/products/) | web browser     |
| [Opera](http://www.opera.com/)                             | web browser     |
| [Spectacle](https://www.spectacleapp.com/)                 | window resizing |
| [iTerm2](https://iterm2.com/)                              | terminal        |
| [Docker](https://www.docker.com/)                          | development     |
| [VLC Media Player](http://www.videolan.org/vlc/index.html) | media player    |

```bash
# Core
brew install \
  git \
  yarn \
  make \
  travis

# Cask
brew cask install \
  visual-studio-code \
  google-chrome \
  firefox \
  opera \
  spectacle \
  iterm2 \
  docker \
  vlc
```

## Shell

Catalina comes with [zsh](http://zsh.sourceforge.net/) as the default shell. Install [Oh My Zsh](https://ohmyz.sh/) for sensible defaults.

```bash
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

## Node.js

Use [Node Version Manager (nvm)](https://github.com/nvm-sh/nvm/blob/master/README.md) to install Node.js.

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.2/install.sh | bash
```

### Install

Install the latest version.

```bash
nvm install node
```

Restart terminal and run the final command.

```bash
nvm use node
```

Confirm that you are using the latest version of Node and npm.

```bash
node -v
npm -v
```

### Update

For later, here's how to update nvm.

```bash
nvm install node --reinstall-packages-from=node
```

### Change version

Here's how to switch to another version and use it.

```
nvm install xx.xx
nvm use xx.xx
```

And to set the default:

```
nvm alias default xx.xx
```

## Git

The first thing you should do with Git is [set your global configuration](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup).

```bash
touch ~/.gitconfig
```

Input your name, email, GitHub username, some aliases, and connect Git to the OS X Keychain.

```bash
[user]
  name   = Firstname Lastname
  email  = you@example.com
[github]
  user   = username
[alias]
  a      = add
  ca     = commit -a
  cam    = commit -am
  cm     = commit -m
  s      = status
  pom    = push origin master
  pog    = push origin gh-pages
  puom   = pull origin master
  puog   = pull origin gh-pages
  cob    = checkout -b
  co     = checkout
  fp     = fetch --prune --all
  l      = log --oneline --decorate --graph
  lall   = log --oneline --decorate --graph --all
  ls     = log --oneline --decorate --graph --stat
  lt     = log --graph --decorate --pretty=format:'%C(yellow)%h%Creset%C(auto)%d%Creset %s %Cgreen(%cr) %C(bold blue)%an%Creset'
```

With the above aliases, I can run `git s` instead of `git status`, for example. The less I have to type repeatedly, the happier I am.

## SSH

Simplify the process of SSHing into other boxes. Create an SSH config file.

```bash
mkdir ~/.ssh && touch ~/.ssh/config
```

Add the following contents, changing the variables for any hosts that you connect to. Using the below will be the same as running `ssh -i ~/.ssh/key.pem user@example.com`.

```bash
Host *
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_rsa

Host myssh
  HostName example.com
  User user
  IdentityFile ~/.ssh/key.pem
```

Now just run the alias to connect.

```bash
ssh myssh
```

#### Generate SSH key

You can [generate an SSH key](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/) to distribute.

```bash
ssh-keygen -t rsa -b 4096 -C "email@example.com"
```

```
ssh-add -K ~/.ssh/id_rsa
```

## Settings

I don't like a lot of the Apple defaults so here are the things I always change.

To get the Home folder in the finder, press `CMD + SHIFT + H` and drag the home folder to the sidebar.

### General

- Dark mode
- Google Chrome default browser

### Dock

- Automatically hide and show Dock
- Show indicators for open applications

### Keyboard

- Key Repeat -> Fast
- Delay Until Repeat -> Short
- Disable "Correct spelling automatically"
- Disable "Capitalize words automatically"
- Disable "Add period with double-space"
- Disable "Use smart quotes and dashes"

### Security and Privacy

- Allow apps downloaded from App Store and identified developers
- Turn FileVault On (makes sure SSD is securely encrypted)
- Turn Firewall On (extra security measure)

### Sharing

- Change computer name
- Make sure all file sharing is disabled

### Users & Groups

- Add "Spectacle" to Login Items

## Defaults

A few more commands to change some defaults.

```bash
# Show Library folder
chflags nohidden ~/Library

# Show hidden files
defaults write com.apple.finder AppleShowAllFiles YES

# Show path bar
defaults write com.apple.finder ShowPathbar -bool true

# Show status bar
defaults write com.apple.finder ShowStatusBar -bool true

# Prevent Two Finger Scroll on Chrome
defaults write com.google.Chrome AppleEnableSwipeNavigateWithScrolls -bool FALSE
```

## Application Settings

### Chrome

- Turn off "warn before quitting"
- Install ublock origin
- Install [DevTools Theme - New Moon](https://chrome.google.com/webstore/detail/devtools-theme-new-moon/lndddploiofhfpdcoclegenegblkhlfk?hl=en)
- Settings
  - Set theme to Dark
  - Go to `chrome://flags` and set Developer Tools Experiments to Enabled
  - Go to Experiments and select Allow custom UI themes

### Visual Studio Code

- Press `CMD + SHIFT + P` and click Install code command in PATH.
- Install Prettier
- Install [New Moon Theme](https://marketplace.visualstudio.com/items?itemName=taniarascia.new-moon-vscode)
- Install GitLens
- Keyboard Shortcuts
  - Copy Line Down - `CMD + SHIFT + E`
  - Delete Line - `CMD + SHIFT + D`
  - Reload Windw - Remove Development Mode from When
  - Format Document - `CMD + SHIFT + L`

### Spectacle

- Full Screen: `CMD + SHIFT + '` (prevents messing with other commands)
- Left Half: `CMD + OPTION + LEFT`
- Right Half: `CMD + OPTION + RIGHT`

### iTerm2

- [Use ⌥ ← and ⌥→ to jump forwards / backwards](https://coderwall.com/p/h6yfda/use-and-to-jump-forwards-backwards-words-in-iterm-2-on-os-x)
- Set tab to open in same location

## Conclusion

That sums it up for my current preferences on setting up a MacBook Pro. I hope it helped speed up your process or gave you ideas for the next time you're setting one up.
