---
title: "Git"
date: 2020-08-21T00:02:36+09:00
draft: false
tags: ["Git"]
---

## 所感

　Git を使いこなすためには Git の内部構造を理解することが必要不可欠。

## 有用なリンク

- [[To Be Decided ~]$ find /tags/git/](https://www.kaitoy.xyz/tags/git/)  
    Git の内部構造を図を用いてわかりやすく解説している。各種コマンドの機能についても内部構造にもとづいた説明がなされており大変わかりやすい。古い記事から順にひと通り目を通すとよい。

- [Git - Reference](https://git-scm.com/docs)
    - [Git - gitglossary Documentation](https://git-scm.com/docs/gitglossary)
- [Git - Book](https://git-scm.com/book/ja/v2)
- [逆引きGit](https://backlog.com/ja/git-tutorial/reference/)
- [GitHub Git チートシート - GitHub Cheatsheets](https://github.github.com/training-kit/downloads/ja/github-git-cheat-sheet/)

## ブランチ

　**ブランチ**とはあるコミットを指し示すポインタのこと。コミットの集合をブランチとよぶのはあやまり。

## HEAD

　**HEAD** とはブランチまたはコミットを指し示すポインタのこと。 メタ的には「現在作業している場所」を指し示すものと解釈される。

### detached HEAD 状態

　HEAD が **detached HEAD 状態**であるとは、HEAD が指し示すものがブランチでなくコミットであるときをいう。  
　この状態のときに Git の何らかの処理を行うと、通常とは違う結果をもたらすことがある。たとえば、HEAD が detached HEAD 状態のときに何度かコミットしてから別のブランチまたはコミットにチェックアウトすると、将来自動的に行われる（または手動で行う）garbage collection という処理によってそれらのコミットはすべて破棄される。

- [Git の 'detached HEAD' 状態とその注意点 - yu8mada](https://yu8mada.com/2018/05/31/detached-head-state-and-its-caution-in-git/)
