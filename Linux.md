# linux

## user

### not login 

```Bash
$ passwd -l tom
$ usermod -s /sbin/nologin tom
```

### allow login  

```Bash
$ passwd -u tom
$ usermod -s /bin/bash tom
```

### netstat


### curl

-m 時間制限
-s 不要なデータを省く
-r 読み込むデータ量を指定

## Security
### get checksum

```Bash
$ sudo yum install openssl
$ openssl dgst -sha256 tmux-1.8.tar.gz
SHA256(tmux-1.8.tar.gz)=f265401ca890f8223e09149fcea5abcd6dfe75d597ab106e172b01e9d0c9cd44
```

## ssh

```Bash
$ chmod 700 ~/.ssh
$ chmod 600 ~/.ssh/authorized_keys
```
