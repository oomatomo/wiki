# Ansible

## Install

yum install ansible

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

