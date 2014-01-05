# Git

## Log

```Bash
$ git log --oneline --decorate --graph --branches --tags --remotes
```

* --oneline: 1commit 1行のみログ表示
* --decorate: branch名、tag名などの別名を表示
* --graph: revision graphを表示
* --branches: 他のbranchのlogも表示
* --tags: tagを表示
* --remotes: remote branchなどを表示

## Tag


```Bash
# TAG_NAMEへcheckout
$ git checkout TAG_NAME
```

## Error

### error: RPC failed; result=22, HTTP code = 405

`git clone`を行ったとき、上記のエラーが出る  
原因はcloneを行うURLにHTTPを利用しているから。HTTPSを利用するとうまくいく。  

```Bash
# Error
$ git clone http://*
# Success
$ git clone https://*
```

