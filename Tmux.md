
# Tmux

## Install

必要な物 
```
yum install wget gcc make ncurses ncurses-devel
apt-get install build-essential checkinstall wget libevent-dev libncurses5-dev
```

```
wget http://downloads.sourceforge.net/tmux/tmux-1.8.tar.gz
tar xvzf tmux-1.8.tar.gz
cd tmux-1.8
./configure
make
sudo make install
```
