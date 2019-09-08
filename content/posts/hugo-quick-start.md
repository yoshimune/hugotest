---
title: "Hugo Quick Start"
date: 2019-09-08T15:41:27+09:00
draft: false
---

# Summary

Hugoの使い方をまとめます。  
本チャプターでは以下の事項について説明します

1. Hugoダウンロード
2. 初期設定
3. テストサイト生成
4. テーマ設定
6. 記事投稿
7. ローカルサーバー起動

# Reference

[HUGO](https://gohugo.io/)

# Hugoとは

Hugoはウェブサイトを構築するためのフレームワークです。Hugoを使って簡単に静的サイトを生成することができます。

## Hugoの特徴

Hugoの特徴は以下の２つです

- [Go言語](https://golang.org/)を使って開発されている
- 高速でサイト生成が可能

> HugoはGoで作成されていますが、Hugoを使用する上でGoの開発環境をセットアップする必要はありません。Hugoはコンパイル済みバイナリファイルとして配布されています。

# Hugo セットアップ

ここからHugoのセットアップ方法について解説します。ここではWindows10を使用していることを前提に解説を進めます。Windows以外のプラットフォームでのセットアップ方法は、[公式のリファレンス](https://gohugo.io/getting-started/installing)を参照してください。

## 前提条件

gitが使えるようにしておいてください

## Hugoダウンロード

[Hugoのリリースページ](https://github.com/gohugoio/hugo/releases)からHugoのzipファイルを適当な場所にダウンロードします。今回は`hugo_0.58.1_Windows-64bit.zip
`をダウンロードしました。

ダウンロードが完了したら、zipファイルを解凍します。

## 初期設定

Hugoの初期設定を行います

### プロジェクトフォルダ

まず、プロジェクトを管理するための場所を用意します。今回は`D:\Hugo`にプロジェクトを配置することにしました。もちろん、各自の環境に合わせて適切な場所を用意して大丈夫です。

`D:\Hugo`配下に、Hugo本体を格納する`bin`フォルダと生成したサイトを格納する`Sites`フォルダを作成します。

`bin`フォルダ配下に、ダウンロードして解凍したHugoフォルダの中身をすべて入れます。

### パス設定

`D:\Hugo\bin`に`Path`を通します。

システムの詳細設定 → 環境変数ウィンドウを開き、`Path`変数に`D:\Hugo\bin`を追加します。

### 動作確認

コマンドプロンプトを開き、以下のコマンドを入力してください。

```
PS D:\Hugo\Sites> hugo new site example.com
```

次のような文言が表示されれば設定完了です

```
PS D:\Hugo> hugo help
hugo is the main command, used to build your Hugo site.

Hugo is a Fast and Flexible Static Site Generator
built with love by spf13 and friends in Go.

Complete documentation is available at http://gohugo.io/.
```

## テストサイト生成

テストサイトを生成してみます。コマンドプロンプトで、`D:\Hugo\Sites`に移動し、以下のコマンドを入力してください。

```
PS D:\Hugo\Sites> hugo new site hugotest.com
```

`D:\Hugo\Sites\hugotest.com`というフォルダが生成されれば成功です。

> Hugoは`UTF-8`でないと正しく動作しません。コマンドプロンプトの文字コードを`UTF-8`に変更するには`CHCP 65001`と入力します

## テーマ設定

生成したサイトにテーマを適用します。テーマのリストは[themes.gohugo.io](https://themes.gohugo.io/)にあります。このquick startは[Ananke theme](https://themes.gohugo.io/gohugo-theme-ananke/)を使います

ここからgitを使います。テーマのプロジェクトをサブモジュールとして追加します。

まずは`example.com`フォルダに移動してください

```
PS D:\Hugo\Sites> cd hugotest.com
```

gitを初期化します。

```
PS D:\Hugo\Sites\hugotest.com> git init
```

テーマをサブモジュールとして追加します。

```
PS D:\Hugo\Sites\hugotest.com> git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/ananke
```

これでテーマが追加されました。最後にプロジェクト設定ファイルを更新してテーマを有効にします。`hugotest.com\config.toml`を以下のように修正してください

```
title = "Hugo Test"
baseURL = "https://hugotest.com"
languageCode = "en-us"
theme = "ananke"

MetaDataFormat = "yaml"
DefaultContentLanguage = "en"
SectionPagesMenu = "main"
Paginate = 3 # this is set low for demonstrating with dummy content. Set to a higher number
googleAnalytics = ""
enableRobotsTXT = true

[sitemap]
  changefreq = "monthly"
  priority = 0.5
  filename = "sitemap.xml"

[params]
  favicon = ""
  description = "The last theme you'll ever need. Maybe."
  facebook = ""
  twitter = ""
  instagram = ""
  youtube = ""
  github = ""
  gitlab = ""
  linkedin = ""
  mastodon = ""
  # choose a background color from any on this page: http://tachyons.io/docs/themes/skins/ and preface it with "bg-"
  background_color_class = "bg-black"
  featured_image = "/images/gohugo-default-sample-hero-image.jpg"
  recent_posts_number = 2
```


## 記事投稿

記事を投稿するには次のコマンドを使用します

```
PS D:\Hugo\Sites\hugotest.com> hugo new posts/my-first-post.md
```

`hugotest.com\content\posts\my-first-post.md`というファイルが生成されます。

新規投稿した記事は、デフォルトで[ドラフト](https://gohugo.io/getting-started/usage/#draft-future-and-expired-content)状態となっています。記事のファイルを開いて、ファイルの先頭に以下の設定が書き込まれていることを確認してください

```
draft: true
```

ドラフトを無効にする場合は`draft: false`にします

## ローカルサーバー起動

ローカルサーバーを起動し、表示を確認します。以下のコマンドを実行し、サーバーを起動してください

```
PS D:\Hugo\Sites\hugotest.com> hugo server
```

ドラフト状態の記事も表示したい場合は、`-D`をつけてサーバーを起動します

```
PS D:\Hugo\Sites\hugotest.com> hugo server -D
```

ローカルサイトは http://localhost:1313 で確認できます

### 補足

Hugoのサイトを何らかのホスティングサービス(たとえばGitHub Pages)で公開する場合、`config.toml`の`baseURL`にドメインを設定します。例えば次のような設定をしたとします。

```
baseURL = "https://yoshimune.github.io/hugotest/"
```

この場合、ローカルサーバーのアドレスは http://localhost:1313/hugotest/ となります
