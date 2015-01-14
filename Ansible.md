# Ansible

## Install

```Bash
# for Centos 6.6
yum -y install epel-release
rpm -ivh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
yum install ansible
```

### ローカルに対して実行

```
$ echo "127.0.0.1" > ~/ansible_hosts
$ export ANSIBLE_HOSTS=~/ansible_hosts
```

ansibleでは実行対象のホストを明確に指定する必要があります
自分の場合は playbookのホストをlocalhostにしてます
ansible_hostsは以下の通りにする

```
[localhost]
127.0.0.1
```

## ansible-palybook

文法チェック  
$ ansible-playbook simple-playbook.yml --syntax-check  
task一覧  
$ ansible-playbook simple-playbook.yml --list-tasks  
dry-run　変更されるかどうか確認できる  
$ ansible-playbook simple-playbook.yml --check  
Done  
$ ansible-playbook simple-playbook.yml  
hostファイル指定  
$ ansible-playbook -i hosts simple-playbook.yml   

## sudo 権限

LANG=C ansible-playbook -i hosts centos.yml --ask-su-pass --check

-ask-su-pass オプションでsu のパスワードを聞かれる  
LAN=Cを設定しないと固まるので注意  

 yaml側の設定  
su: True  
su_user: root  
remote_user: omagari.tomohisa  

su: su コマンドを実行する  
su_user: suコマンド切り替わるユーザ  
remote_user: サーバに接続するときのユーザ名  
