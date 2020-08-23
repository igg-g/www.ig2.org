---
title: GitHub メモ
date: 2020-08-20T23:32:17+09:00
tags: ["GitHub", "Git"]
draft: false
---

## 二段階認証設定後に HTTP でプッシュする方法

　自分の GitHub ページでパーソナルアクセストークンを取得する必要がある。
コマンドラインで入力するパスワードは、GitHub アカウントのパスワードではなく取得したパーソナルアクセストークン。

- [リモートの URL の変更 - GitHub Docs](https://docs.github.com/ja/github/using-git/changing-a-remotes-url#switching-remote-urls-from-ssh-to-https)
    > 2要素認証 を有効にしている場合は、パーソナルアクセストークンを作成して、GitHub パスワードのかわりに使用することができます。

    「使用することができます」ではなく「使用しなくてはなりません」が正しいのでは。
    
- [個人アクセストークンを使用する - GitHub Docs](https://docs.github.com/ja/github/authenticating-to-github/creating-a-personal-access-token)

- [GitHub Error: Authentication Failed from the Command Line | by Ginny Fahs | Medium](https://medium.com/@ginnyfahs/github-error-authentication-failed-from-command-line-3a545bfd0ca8)

- [GitHubに二段階認証を設定した後にGit操作できない時の解決策 - Qiita](https://qiita.com/kitoko552/items/3f45de6c876c638b690d)

    > repoからgistまでを全てチェックしてGenerate token

    プッシュするだけなら repo だけでよい。
