# Mac

# homebrew

http://brew.sh/

install

```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
# ライセンス認証

Agreeing to the Xcode/iOS license requires admin privileges, please re-run as root via sudo.
Xcodeのライセンス認証

```
sudo xcrun cc
```

# brew file

```
brew tap rcmdnk/file
brew install brew-file
brew file set_repo -r oomatomo/Brewfile
brew file init
brew file edit
brew file update
brew file cask_upgrade
```

# R言語

```
brew tap homebrew/science
brew install gcc
brew install Caskroom/cask/xquartz
brew install r
```
