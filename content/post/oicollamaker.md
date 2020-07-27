---
title: "html2canvasをつかってコラ画像生成アプリをつくってみた"
date: 2020-07-27T22:50:45+09:00
draft: false
tags: ["html2canvas"]
---
## つくったもの

<img src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/445469/16a31f90-b5e6-5f5a-b7ec-82003a94cd16.gif" width="600">

**[Oicollamaker](https://igg-g.github.io/Oicollamaker/)**（GitHubは[こちら](https://github.com/igg-g/Oicollamaker)）

できました。*[明治おいしい牛乳](https://www.meijioishiigyunyu.com/)* のパッケージのコラージュ画像を生成できるアプリです。

**Q.**　こんなアプリだれがつかうんですか？  
**A.**　**知りません**。

**Q.**　スマホにも対応していますか？  
**A.**　**完全非対応**です。

## なぜつくったか

JavaScriptでDOMを操作する方法を学び、何かをつくりたくなったからです。

## html2canvas

*明治おいしい牛乳*（以下「*おい牛*」という）のパッケージ部分はHTMLでできています。
そのため右クリックで保存することができません。

もちろんスクショを撮ってトリミングすれば保存できますが、ちょっとめんどうです。

そこで、*おい牛*のパッケージ部分を画像化する機能をつけてみました。
その部分では**html2canvas**というJavaScriptのライブラリを利用しています。

公式サイトはこちら: 
[html2canvas - Screenshots with JavaScript](https://html2canvas.hertzen.com/)

つかいかたは至ってかんたんです。
たとえばHTML内の画像化したい要素に`target`という`id`をつけておいて、

```javascript:JavaScript
html2canvas(document.getElementById("target")).then(canvas => {
      document.body.appendChild(canvas);
});
```

という魔法を唱えると、`target`の内容が描画された`canvas`を`body`の子要素として追加することができます。

## 問題点

肝心の*おい牛*パッケージを画像化する機能ですが、いろいろと問題があります。

たとえば、  
<img width="150" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/445469/a69b3246-d6a5-04f8-7016-320d50b66202.png">  
これをhtml2canvasで画像化すると・・  

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/445469/428c8aaf-db56-58e5-4f51-18a68d2690a8.png)  
こうなります。

ヨとグのあいだの長音符（のばし棒）の向きがおかしいですね。
おそらくhtml2canvasが縦書きのテキストに対応していないのでしょう。

また、画面最上部からいくらか下にスクロールした状態でボタンを押して画像化を実行すると・・

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/445469/183e7ed8-341c-060c-c447-cef2326f0de1.png)

このように、スクロールした分だけ下にずれた画像が生成されてしまいます。
このバグはなんとかしたら直せそうなので、気が向いたらやってみようとおもいます。

## 工夫した点
はじめは、`input`要素をHTMLに直接記述することで入力用のテキストボックスを設置していました。

```html:HTML
<form>
  <input type="text" placeholder="明治">
  <input type="text" placeholder="おいしい牛乳">
  <input type="text" placeholder="ナチュラルテイスト製法">
</form>

<section>
  <div class="output">明治</div>
  <div class="output">おいしい牛乳</div>
  <div class="output">ナチュラルテイスト製法</div>
</section>
```

こんなかんじです（コードは説明のために簡略化しています）。

このうえでJavaScriptを次のように書いていました。

```javascript:JavaScript
const inputs = document.querySelectorAll("input");
const outputs = document.querySelectorAll(".output");

for (i = 0; i < inputs.length; i++) {

  // 入力した文字がリアルタイムで反映されるように設定
  inputs[i].addEventListener("input", e => {
    outputs[i].textContent = e.target.value; 
}
```

このままでもいいのですが、HMTLのほうで重複している箇所が多いのが気になります。

たとえば「おいしい牛乳」を「世界一おいしい牛乳」にさしかえたい場合、
`input`要素の`placeholder`属性の値と、`div`内のテキストの２箇所を修正しなければなりません。
これはちょっとめんどうです。

そこで、ページが読み込まれたときにDOMを操作することによって、
入力用のテキストボックスが自動的に設置されるようにしてみました。

まず、HTMLを次のように修正します。

```html:HTML
<form></form>

<section>
  <div class="output">明治</div>
  <div class="output">おいしい牛乳</div>
  <div class="output">ナチュラルテイスト製法</div>
</section>
```

`input`要素をすべて取りのぞきました。

そしてJavaScriptを次のように修正します。

```javascript:JavaScript
const outputs = document.querySelectorAll(".output");

for (i = 0; i < outputs.length; i++) {
  const form = document.querySelector("form");

  // input要素を作成
  const input = document.createElement("input");

  // 作成したinput要素に属性を付与
  input.setAttribute("type", "text");
  input.setAttribute("placeholder", outputs[i].textContent);
  
  // 入力した文字がリアルタイムで反映されるように設定
  input.addEventListener("input", e => {
      outputs[i].textContent = e.target.value;
  });

  // formの子要素としてinputを追加
  form.appendChild(input);
}
```

このようにすれば、ページが読み込まれたときに、
`output`クラスをつけておいたHTML要素それぞれについて、
対応する入力用のテキストボックスが設置されるようになります。

もしもコラージュの対象を *[明治エッセルスーパーカップ](https://www.meiji.co.jp/sweets/icecream/essel/)* に変更したくなったら、
HTML内の該当する部分を*スーパーカップ*仕様にかきかえて、
編集させたい要素に`output`クラスをつけるだけでOKです。ラクチンですね。


## おわりに

なんか*明治*のまわしものみたいになってしまいましたが、
いっさい関係ありません。。。

最近はPHPもさわりはじめたので、今度はPHPもつかって何かつくってみようとおもいます。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/445469/d5f04790-e8bf-fd1e-3a8f-f01d4dc42568.png)
