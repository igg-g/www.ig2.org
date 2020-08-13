---
title: "Hugo と GitHub Pages でかわいいブログつくったよ☆"
date: 2020-07-28T19:23:43+09:00
draft: true
tags: ["Hugo", "作業ログ", "書きかけ"]
---

## はじめに

こんにちは、igg です。  
この度 [Hugo](https://gohugo.io/) という静的サイトジェネレーターをつかってこちらのブログををつくってみました。  
ホスティング先は [GitHub Pages](https://pages.github.com/) です。  

<figure>
    <img src="https://www.ig2.org/images/logo_img_only.svg" alt="logo" class="logo_img_only" style="display: block; margin: 2rem auto;">
    <figcaption>回転する人</figcaption>
</figure>

また、せっかくなので [Amazon Route 53](https://aws.amazon.com/jp/route53/) で `ig2.org` という独自ドメインを取得してサブドメインの `www.ig2.org` を割り当ててみました。  

それぞれのサービスを採用した理由は次のとおりです。

| サービス | 採用した理由 |
|---|---|
| Hugo | なんかめっちゃ速いらしいので。 |
| GitHub Pages | 利用したことがあったため。 |
| Amazon Route 53 | AWS のアカウントをすでに持っていたため。クレカも（強制的に）登録済みだし。 |

ブログの作成についてとくにハマったところはなく、特段の知見も得られなかったのですが、一応作業ログを残しておこうと思います。

### 作業環境
- macOS Catalina (10.15.6)
- zsh 5.7.1
- Homebrew 2.4.8
- Git 2.27.0

### 前提
- GitHub アカウントを持っている
- AWS アカウントも持っている

### 参考にしたサイト
- [Quick Start | Hugo](https://gohugo.io/getting-started/quick-start/)
- [Hugoを導入してWebサイトをGitHub Pagesで公開してみた話 - Qiita](https://qiita.com/akivajp/items/1fd52a610e3eed5b7758)

## Amazon Route 53 で独自ドメインを取得する

とりあえず独自ドメインをとっちゃいましょう。  

まず AWS マネジメントコンソールにサインインして Route 53 にアクセスします。  
次に左側のメニューにある［登録済みドメイン］をクリックし、［ドメインの登録］を開始してください（**Figure 2**）。

{{< figureCupper
img="images/img1.png" 
caption="ドメインの登録を開始する" 
command="Resize" 
options="700x" >}}

すると **Figure 3** のような画面に遷移します。あとは指示にしたがってドメインの登録をすすめてください。

{{< figureCupper
img="images/img2.png" 
caption="ドメイン登録フロー" 
command="Resize" 
options="700x" >}}

ここでは `ig2.org` というドメインを取得したことにします。

これで AWS での作業は一旦おわりですが、
あとで DNS の設定をする際に戻ってきますので画面は開いたままにしておくとよいでしょう。

## Hugo のインストール

それでは Hugo をインストールしましょう。Hugo は Homebrew でインストールすることができます。  
ターミナルを開いて次のコマンドを実行してください。

```
% brew install hugo
```

正常にインストールできたかどうかを確認するため、Hugo のバージョンを確認してみます。

```
% hugo version
Hugo Static Site Generator v0.74.2/extended darwin/amd64 BuildDate: unknown
```

このように Hugo のバージョンが表示されれば OK です。  
私の環境では `BuildDate` がなぜか `unknown` になっていました。が、気にしないことにしました。

## Hugo プロジェクトを作成する

さて `hugo` コマンドが使えるようになりました。
これを利用して Hugo プロジェクトを作成しましょう。

まず、Hugo プロジェクトを作成したいディレクトリに移動します。
私の場合、Git でバージョン管理を行うプロジェクトはホームディレクトリ直下の `git` ディレクトリの中にまとめて置くことにしていますので、そちらへ移動しました。

```
% cd ~/git
```

移動したら、 `hugo new site` というサブコマンドを利用して Hugo プロジェクトを作成しましょう。  
ここで引数に指定したものが Hugo プロジェクトの名前となります。名前はなんでもよいのですが、今回は先ほど取得した独自ドメイン `ig2.org` のサブドメイン `www.ig2.org` をブログに割り当てる予定ですので、分かりやすいように Hugo プロジェクトの名前も `www.ig2.org` としました。

```
% hugo new site www.ig2.org
```

このコマンドを実行すると次のようなファイル群が自動的に生成されます。

```
www.ig2.org
├── archetypes
│   └── default.md
├── config.toml
├── content
├── data
├── layouts
├── static
└── themes
```

確認できたとろこで、Git 管理の初期化を行っておきます。

```
% cd www.ig2.org
% git init
% git add .
% git commit -m "Initial commit"
```

そしたら、サイトのテーマを追加していきましょう。
次のサイトにアクセスしてお好みのテーマを探してください。

[Complete List | Hugo Themes](https://themes.gohugo.io/)

{{< figureCupper
img="images/img3.png" 
caption="Hugo テーマ一覧サイト" 
command="Resize" 
options="700x" >}}

上記のサイトではアップデート日が新しいものから順に並んでいるようなので、テーマ選びの際に参考にするとよいでしょう。  
私は [Cupper](https://themes.gohugo.io/cupper-hugo-theme/) というテーマを選びました。

{{< figureCupper
img="images/img4.png" 
caption="Hugo テーマ Cupper" 
command="Resize" 
options="700x" >}}

アクセシビリティに配慮したテーマのようです。
私はたんにデザインが気に入ったのこちらのテーマを選びました。

テーマを選んだら［Download］ボタンをクリックします。すると GitHub にジャンプしますのでリポジトリの URL をコピーしておきます（**Figure 6**）。

{{< figureCupper
img="images/img5.png" 
caption="選んだテーマのリポジトリ URL をコピーする" 
command="Resize" 
options="700x" >}}

では Hugo プロジェクトにテーマを取り込みましょう。
テーマは `themes` ディレクトリの直下に取り込みます。
テーマの更新が行われたときに変更をとりこみやすくするため、サブモジュールとして追加するとよいでしょう。

```
% git submodule add https://github.com/zwbetz-gh/cupper-hugo-theme.git themes/cupper-hugo-theme
```

Git のサブモジュールについてはこちらを参考にしてください。

[Gitのサブモジュール機能を使ってプロジェクトを管理してみよう | vdeep](http://vdeep.net/git-submodule)

