Perl

# perlbrew install

```install
curl -L http://xrl.us/perlbrewinstall | bash
```

```.bashrc
source ~/perl5/perlbrew/etc/bashrc
```

* install

```perl_install
perlbrew install perl-5.14.1
perlbrew available list
```

* switch

```perl_switch
perlbrew switch perl-5.14.1
```

# cpanm

```install
perlbrew install-cpanm
curl -L http://cpanmin.us/ | perl - App::cpanminus
```

# Carton

```
cpanm Carton
```

cpanfileにインストールするモジュールを書く

```cpanfile
#perl version
requires 'perl', '5.16.3';

````



