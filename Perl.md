# Perl

## perlbrew install

```Bash
curl -L http://xrl.us/perlbrewinstall | bash
```

~/.bashrcに記入する

```Bash:.bashrc
source ~/perl5/perlbrew/etc/bashrc
```

* install

```Bash
perlbrew install perl-5.14.1
perlbrew available list
```

* switch

```Bash
perlbrew switch perl-5.14.1
```

## cpanm

```Bash
perlbrew install-cpanm
curl -L http://cpanmin.us/ | perl - App::cpanminus
```

## Carton

```Bash
cpanm Carton
```

cpanfileにインストールするモジュールを書く

```Bash:cpanfile
#perl version
requires 'perl', '5.16.3';

```

## Nytprof

NYTPROF=file=/tmp/nytprof.out:out:addpid=1:endatexit=1:stmts=0
NYTPROF=sigexit=int

```
nytprofmerge nytprof.out.*
nytprofhtml -m --file=nytprof-merged.out
```

## 豆知識
### Array to Hash

```Bash  
$hash{$_}++ for (@array);
 or
%hash = map { $_ => 1 } @array;
```

