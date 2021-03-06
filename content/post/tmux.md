---
title: "tmux"
date: 2020-08-22T04:57:28+09:00
draft: false
tags: ["tmux"]
---

　tmux は「ティマックス」と読む。「ティーマックス」ではない。  
<audio src="http://www.howtopronounce.cc/file/303a2518c5924b609004e2604a2cd246.mp3" controls="controls" controlslist="nodownload">Your browser does not support the audio element.</audio>  
（[How to pronounce Tmux: howtopronounce.cc](http://www.howtopronounce.cc/tmux) より）

## コマンドラインから

| コマンド | 説明 |
| -- | -- |
| `tmux [new]` | 新しいセッションを作成してアタッチ |
| `tmux new -s <session-name>` | 新しいセッションを指定した名前で作成しアタッチ |
| `tmux ls` | すべてのセッションを一覧表示 |
| `tmux a [-t <session-name>]` | 指定したセッションにアタッチ。`-t` オプションを省略したときは最後にデタッチしたセッションにアタッチする（？） |
| `tmux kill-server` | すべてのセッションを破棄し tmux サーバーを終了 |

## tmux 内部から

　以下のコマンドはプレフィックスキーに続けて入力するもの。

### ペイン

| コマンド | 説明 |
| -- | -- | 
| `%` | 現在のペインを左右に分割 |
| `"` | 現在のペインを上下に分割 |
| `x` | 現在のペインを強制終了。コマンドラインで `$ exit` するほうが安全 |
| `q` | ペイン番号をすこしのあいだ表示 |
| `o` | ペイン番号が 1 大きいペインに移動 |

### ウィンドウ
    
| コマンド | 説明 |
| -- | -- | 
| `c` | 新しいウィンドウを作成 |
| `,` | 現在のウィンドウの名前を変更する |
| `n` | ウィンドウ番号が 1 大きいウィンドウに移動 |
| `p` | ウィンドウ番号が 1 小さいウィンドウに移動 |
| `w` | 対話的にウィンドウを選択 |
| `&` | 現在のウィンドウを強制終了。すべてのペインで `$ exit` するほうが安全 |

### セッション


| コマンド | 説明 |
| -- | -- | 
| `$` | 現在のセッションの名前を変更 |
