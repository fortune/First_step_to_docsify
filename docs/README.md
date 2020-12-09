# docsify について

**docsify** とは、[本家サイト](https://docsify.js.org/#/)によると、

> 魔法のドキュメントサイトジェネレータ

となる。

## 特徴

- Markdown 記法でドキュメントを書ける
- 静的な HTML ファイルの集積としてのドキュメントサイトを構築するのではないのでビルド不要
- 簡単かつ軽量
- 表示のデザインを複数のテーマから選べる
- プラグインで機能を拡張できる（検索や言語シンタックスハイライト等）

ビルド不要なので、[Sphinx](https://www.sphinx-doc.org/ja/master/) のようにツールやライブラリをインストールしてビルド環境を構築する必要がなく、`index.html` ファイル１つと、Markdown で記述されたドキュメントファイルだけ用意すればよい。

### 仕組み

`index.html` ファイルが SPA として機能する。これが Markdown 形式のドキュメントをパスに応じて取得し、パースしてブラウザ上に表示するだけ。

## 始め方
### 手動でやる方法

docsify は `index.html` ファイルにおいてはたらく SPA にすぎないので、手動で次のような内容の `index.html` を
作って始めることができる。

```html
<!-- index.html -->

<!DOCTYPE html>
<html>
<head>
  <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <meta charset="UTF-8">
  <link rel="stylesheet" href="//cdn.jsdelivr.net/npm/docsify@4/themes/vue.css" />
</head>
<body>
  <div id="app"></div>
  <script>
    window.$docsify = {
      //...
    }
  </script>
  <script src="//cdn.jsdelivr.net/npm/docsify@4"></script>
</body>
</html>
```

あとは、ドキュメントのルートとなる `README.md` ファイルを用意する。このファイルもそうだ。

### Node.js を使う方法

docsify を使うのにツールやライブラリをインストールする必要はないが、便利なコマンドラインツールが Node.js の
パッケージとして提供されている。これを使うなら `npm` で `docsify-cli` パッケージというのをインストールすればいいが、
`npx` コマンドを使えば、インストールせずに使える（厳密には実行のたびにリポジトリからパッケージをダウンロード、展開して、
終了後に削除する）ので環境を汚したくないなら、こちらがお勧め。いずれにしろ、Node.js のインストールは必要。

`npx` で次のように実行すれば、`index.html`, `README.md` の雛形と、`.nojekyll` ファイルが `./docs` フォルダに
作成される。`.nojekyll` ファイルは、GitHub Pages で公開する場合に必要になる。

```bash
npx docsify-cli init ./docs
```

### ローカルでのプリビュー方法

`index.html` を作成したフォルダがドキュメントルートとなるようにローカルで Web サーバを起動すればいい。Python を
使うなら、`index.html` ファイルがあるフォルダに移動して、シェル上で次のように実行する。

```bash
python -m http.server port 3000
```

`npx` を使うなら、ドキュメントのフォルダを指定して次のように実行する。

```bash
npx docsify-cli serve ./docs/
```

## カスタマイズのやり方

`index.html` ファイルにカスタマイズや使用したいプラグインを指定する。このサンプルで使ったもののいくつかは次のとおり。

### テーマの設定

```html
 <link rel="stylesheet" href="//cdn.jsdelivr.net/npm/docsify@4/lib/themes/vue.css">
  <!--
  <link rel="stylesheet" href="//cdn.jsdelivr.net/npm/docsify@4/lib/themes/vue.css">
  <link rel="stylesheet" href="//cdn.jsdelivr.net/npm/docsify/lib/themes/buble.css">
  <link rel="stylesheet" href="//cdn.jsdelivr.net/npm/docsify/lib/themes/dark.css">
  <link rel="stylesheet" href="//cdn.jsdelivr.net/npm/docsify/lib/themes/pure.css">
  -->
```

### 言語のシンタックスハイライトの設定

```html
<script src="//cdn.jsdelivr.net/npm/prismjs@1/components/prism-bash.min.js"></script>
<script src="//cdn.jsdelivr.net/npm/prismjs@1/components/prism-python.min.js"></script>
```

### ナビゲーションバーの名前とリポジトリへのリンクの設定

```html
<script>
    window.$docsify = {
      name: 'About docsify',
      repo: 'https://github.com/aisingltd'
    }
</script>
```

## ドキュメントの公開方法

ドキュメントのフォルダを Web で公開すればそれで終わりなのだが、GitHub Pages を使うと、
ソースコードやその他のリソースと一元管理しているドキュメントをそのまま公開できる。

### GitHub Pages での公開

リポジトリのルートに `docs/` という名前でドキュメント用のフォルダをつくり、そこに
docsify の `index.html` を置いておく。GitHub Pages の設定は https://www.tam-tam.co.jp/tipsnote/html_css/post11245.html でも見てください。

このサンプルの [GitHub でのリポジトリ](https://github.com/fortune/First_step_to_docsify)

GitHub Pages 以外でも GitLab Pages というのもあるようです。


## 感想と今後

docsify はビルドが必要なく、特別なライブラリや環境がいらないので、始めるにあたってのハードルが非常に低いし、
セットアップやデプロイも簡単である。
簡単な Markdown 記法でドキュメントが記述でき、設定がほぼ `index.html` ファイルで完結するのもよい。
小規模なプロジェクトやプロトタイプの作成で徐々に慣れていくのがいいのではないか。

不利な点は、SPA で動的にドキュメントをパースして表示するので、検索エンジンに登録されないことだが、
少なくとも仕事用のドキュメントとして使うには問題にはならないと思う。

近い内に試したいのは以下の２点。

- `README.md` 以外の複数のドキュメントの作成
- [Plant UML](https://ja.wikipedia.org/wiki/PlantUML) の利用
