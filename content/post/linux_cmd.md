---
title: "Linuxコマンド"
date: 2020-08-25T23:02:29+09:00
draft: false
tags: ["Linux"]
---

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
