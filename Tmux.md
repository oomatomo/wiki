# Tmux

## Install

必要な物  

```Bash
yum install wget gcc make ncurses ncurses-devel
apt-get install build-essential checkinstall wget libncurses5-dev
```

```Bash
# libevent install
wget https://github.com/downloads/libevent/libevent/libevent-2.0.21-stable.tar.gz
tar xvf libevent-2.0.21-stable.tar.gz
cd libevent-2.0.21-stable
./configure
make
sudo make install

# tmux install
wget http://downloads.sourceforge.net/tmux/tmux-1.8.tar.gz
tar xvzf tmux-1.8.tar.gz
cd tmux-1.8
./configure
make
sudo make install
```

## Errorの種類

```Bash
control.c:63: warning: implicit declaration of function ‘evbuffer_readln’
control.c:63: error: ‘EVBUFFER_EOL_LF’ undeclared (first use in this function)
control.c:63: error: (Each undeclared identifier is reported only once
control.c:63: error: for each function it appears in.)
```

libeventのバージョンが古い状態