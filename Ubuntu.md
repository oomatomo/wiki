## 日本語化

[日本語化について](http://www.ubuntulinux.jp/japanese)

```  
wget -q https://www.ubuntulinux.jp/ubuntu-ja-archive-keyring.gpg -O- | sudo apt-key add -  
wget -q https://www.ubuntulinux.jp/ubuntu-jp-ppa-keyring.gpg -O- | sudo apt-key add -  
sudo wget https://www.ubuntulinux.jp/sources.list.d/precise.list -O /etc/apt/sources.list.d/ubuntu-ja.list  
sudo apt-get update   
sudo apt-get upgrade  
```

#vimの文字化け  

```
sudo aptitude install language-pack-ja  
export LANG=ja_JP.UTF-8  
export LC_ALL=ja_JP.UTF-8  
export LC_CTYPE=ja_JP.UTF-8  
``` 
