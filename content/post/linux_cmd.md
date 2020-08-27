---
title: "Linuxコマンド"
date: 2020-08-25T23:02:29+09:00
draft: false
tags: ["Linux"]
---

## 有用なリンク

- [JM Project (Japanese)](https://linuxjm.osdn.jp/)

## ジョブ

　シェルから見た実行中のコマンドを**ジョブ**という。

### コマンドをバックグラウンドジョブとして実行する

```
$ コマンド &
```

#### 例

```
$ find . -size +100M > out.txt &
[1] 10322
```

　`[1]`：ジョブ番号、`10322`：プロセスID


### 実行中または一時停止中のジョブの一覧を表示する

```
$ jobs
```

#### 例

```
$ sleep 100 &
[1] 10708
$ sleep 200 &
[2] 10710
$ sleep 300 &
[3] 10713
$ sleep 400 &
[4] 10715
$ jobs
[1]    running    sleep 100
[2]    running    sleep 200
[3]  - running    sleep 300
[4]  + running    sleep 400
```

　`-`：プリビアスジョブ、`+`：カレントジョブ  

### フォアグラウンドジョブを一時停止する

　次のキーを入力する。
```
[control]+[z]
```

#### 例

```
$ sleep 100
^Z
zsh: suspended  sleep 100
$ jobs
[1]  + suspended  sleep 100
```

### 一時停止中のジョブをバックグラウンドジョブにする

```
$ bg [%ジョブ番号]
```

#### 例

```
$ jobs
[1]    suspended  sleep 100
[2]    suspended  sleep 200
[3]  - suspended  sleep 300
[4]  + suspended  sleep 400
$ bg %2
[2]    continued  sleep 200
$ jobs
[1]    suspended  sleep 100
[2]    running    sleep 200
[3]  - suspended  sleep 300
[4]  + suspended  sleep 400
```

　引数なしで実行するとカレントジョブが対象となる。

```
$ jobs
[1]    suspended  sleep 100
[2]    suspended  sleep 200
[3]  - suspended  sleep 300
[4]  + suspended  sleep 400
$ bg
[4]    continued  sleep 400
$ jobs
[1]    suspended  sleep 100
[2]  - suspended  sleep 200
[3]  + suspended  sleep 300
[4]    running    sleep 400
```

### フォアグラウンドジョブを終了させる

　次のキーを入力する。

```
[control]+[c]
```

#### 例

```
$ jobs
$ sleep 100
^C
$ jobs
```

### 実行中または一時停止中のジョブを終了させる

```
$ kill %ジョブ番号
```

#### 例

```
$ jobs
[1]    running    sleep 100
[2]    running    sleep 200
[3]  - running    sleep 300
[4]  + running    sleep 400
$ kill %2
[2]    terminated  sleep 200
$ jobs
[1]    running    sleep 100
[3]  - running    sleep 300
[4]  + running    sleep 400
```

## プロセス

　システムから見た実行中のプログラムを**プロセス**という。

## 基本コマンド

### キーワードでmanページを検索する

```
$ apropos キーワード
```

#### 例

```
$ apropos editor
ed(1), red(1)            - text editor
mg(1)                    - emacs-like text editor
nano(1)                  - Nano's ANOther editor, an enhanced free Pico clone
pdisk(8)                 - Apple partition table editor
psed(1)                  - a stream editor
sed(1)                   - stream editor
vim(1)                   - Vi IMproved, a programmer's text editor
zshzle(1)                - zsh command line editor
Mach-O(5)                - Mach-O assembler and link editor output
```

### 行をソートする

```
$ sort [オプション] ファイルパス
```

　与えられたファイルの各行をソートし、結果を標準出力に書き出す。

[sortコマンド、基本と応用とワナ - Qiita](https://qiita.com/richmikan@github/items/cc4494359b1ac2f72311)

#### オプションなし

　文字コード順にソートする（と思っておけばよい）。

```
$ cat trump.txt
again
Make
great
America
$ sort trump.txt
America
Make
again
great
```

　正確には `LC_COLLATE` という環境変数によって挙動が変わる。

> LC_COLLATE  
>         (特に他の指定がない限り) 全ての比較で用いられる 文字の照合順序を指定する。  
> 出典：[Man page of SORT](http://linuxjm.osdn.jp/html/gnumaniak/man1/sort.1.html)

#### 大文字と小文字を区別しない

```
$ cat trump.txt
again
Make
great
America
$ sort -f trump.txt
again
America
great
Make
```

#### 逆順

```
$ cat trump.txt
again
Make
great
America
$ sort -r trump.txt
great
again
Make
America
$ !! | tail -2 && !! | head -2
sort -r trump.txt | tail -2 && sort -r trump.txt | head -2
Make
America
great
again
```

<div style="width: 300px">
<div class="tenor-gif-embed" data-postid="11955445" data-share-method="host" data-width="100%" data-aspect-ratio="1.1237623762376239"><a href="https://tenor.com/view/donald-trump-trippy-deal-with-it-gif-11955445">Donald Trump Trippy GIF</a> from <a href="https://tenor.com/search/donaldtrump-gifs">Donaldtrump GIFs</a></div><script type="text/javascript" async src="https://tenor.com/embed.js"></script>
</div>

#### 数値順

```
$ cat number.txt
-3
5
-7
11
2
$ sort -n number.txt
-7
-3
2
5
11
$ sort number.txt
-3
-7
11
2
5
```

#### ソート対象の列を指定

```
$ cat suchmos.txt
Vo YONCE 28
Ba HSU 31
Dr OK 29
Gt TAIKING 30
Dj KCEE 27
Key TAIHEI 28
$ sort -k 2,2 suchmos.txt
Ba HSU 31
Dj KCEE 27
Dr OK 29
Key TAIHEI 28
Gt TAIKING 30
Vo YONCE 28
$ sort -k 2r,2 suchmos.txt
Vo YONCE 28
Gt TAIKING 30
Key TAIHEI 28
Dr OK 29
Dj KCEE 27
Ba HSU 31
```

　列番号は１から始まることに注意。

```
$ cat suchmos.txt
Vo,YONCE,28
Ba,HSU,31
Dr,OK,29
Gt,TAIKING,30
Dj,KCEE,27
Key,TAIHEI,28
$ sort -t ',' -k 2,2 suchmos.txt
Ba,HSU,31
Dj,KCEE,27
Dr,OK,29
Key,TAIHEI,28
Gt,TAIKING,30
Vo,YONCE,28
```

　このように区切り文字が半角スペース１文字でないときは `-t` オプションで区切り文字を指定する。
