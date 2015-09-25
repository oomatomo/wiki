# Go

## 環境変数

```
export GOROOT=/usr/local/opt/go/libexec
export GOPATH=$HOME/go
export PATH=$GOPATH/bin:$GOROOT/bin:$PATH
```

## PATH系

GOROOT : goインストールルートフォルダ
GOPATH : ワーキングディレクトリ

## コマンド

```
go get code.google.com/p/go.tools/cmd/goimports
go get code.google.com/p/go.tools/cmd/godoc
go get code.google.com/p/go.tools/cmd/vet
go get code.google.com/p/go.tools/cmd/cover
go get github.com/nsf/gocode
go get github.com/golang/lint
go get code.google.com/p/rog-go/exp/cmd/godef
go get github.com/jstemmer/gotags
```

## クロスコンパイル

```
brew install go --cross-compile-all
```

https://golang.org/doc/install/source#environment  
各環境の情報

x86_64はamd64  
GOOSはOS  
GOARCHはCPU アーキテクチャ  
`uname -m` で確認できる

```
GOOS=linux GOARCH=amd64 go build 
```



