# Vim


## Delete

de   - 単語の最後まで削除     
db   - （カーソルが単語の先頭にあるとき）前の単語を削除  
d0   - カーソルの前から行の先頭までを削除   
dd   - 一行削除  
dj   - カーソルがある行とその下の行を削除   
dk   - カーソルがある行とその上の行を削除   
daw  - 単語を削除  
diw  - 単語を削除（単語だけ。スペースは削除しない）  
dib  - ()内を削除  
dip  - パラグラフ（かたまり）削除  
dap  - パラグラフとその後の空行を削除  
dvip - パラグラフの最後の行を残し削除  
D    - カーソルから行の最後まで削除  
3dd  - ３行削除  
4dd  - ４行削除  

## 折りたたみ

zo - カーソル下にある折りたたみをひとつ開く  
zO - カーソル下にある折りたたみを全て開く  
zc - カーソル下にある折りたたみをひとつ閉じる  
zC - カーソル下にある折りたたみを全て閉じる  

## テキストオブジェクト

ある特定の文字に囲まれた文字の集合体  

簡単に言うと　() で囲まれた文字　みたいな感じです  

## 改行削除

```
:%s/\n//g
```
