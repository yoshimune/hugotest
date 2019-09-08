---
title: "Hugo on Github Pages"
date: 2019-09-08T16:59:03+09:00
draft: false
---

# Summary

Hugoで生成したサイトをGitHub Pagesでホスティングする方法を解説します

# Reference

[Host on GitHub - HUGO](https://gohugo.io/hosting-and-deployment/hosting-on-github/)  
[User, Organization, and Project Pages - GitHub Help](https://help.github.com/en/articles/user-organization-and-project-pages#user--organization-pages)

# GitHub Pages

## GitHub Pages Type

GitHub Pagesには２つのタイプがあります

* ユーザー/組織ページ
	- ユーザー、または組織のアカウント名に紐づくページを作成します
* プロジェクトページ
	- プロジェクトリポジトリに紐づくページを作成します。

今回はプロジェクトページを利用します

## 公開方法タイプ

公開方法にも２つタイプがあります。

1. `master`ブランチのルートディレクトリに公開用ディレクトリ`docs`を作成する
2. 公開用ブランチ`gh-pages`を作成する

今回は2.の方法を採用します。2.の方法は公開するサイトとソースコードを分離してバージョン管理することができ、バージョン管理がやりやすいという利点があります。


# 公開方法手順

公開用ブランチ`gh-pages`にはサイトに関するファイル以外コミットしないようにします。そのため、`gh-pages`ブランチは[orphan branch](https://git-scm.com/docs/git-checkout/#git-checkout---orphanltnewbranchgt)として作成します。

サイトに関連するファイルはすべて`public`フォルダに生成されます。`gh-pages`ブランチはこのフォルダのみコミットしていきます。逆に`master`ブランチは`public`フォルダを無視するようにします。

## コンフィグ修正

`config.toml`を修正してGitHub Pagesで公開するための設定を行います。`config.toml`を開いて、以下の箇所を修正します。

```
baseURL = "https://yoshimune.github.io/hugotest/"
```

`baseURL`はプロジェクト毎に適切なものを設定します。基本的には`https://<USER_NAME>.github.io/<PROJECT_NAME>/`という形式になっています。

正確に調べたい場合はGitHubのプロジェクトのページへ行き、 **Settings** 画面の下部 **GitHub Pages** のところにURLが書いてあります

[![Image from Gyazo](https://i.gyazo.com/626b36b1383b2c3ffe70e32c650cb1b4.png)](https://gyazo.com/626b36b1383b2c3ffe70e32c650cb1b4)

[![Image from Gyazo](https://i.gyazo.com/4f9c95650bf14039695ef71a276ebcc1.png)](https://gyazo.com/4f9c95650bf14039695ef71a276ebcc1)

もしこの場所にURLが書いてなければ一旦この手順を飛ばしても構いません。`gh-pages`ブランチをpushした後には表示されているはずです。


## .gitignore

`.gitignore`ファイルに`public`フォルダを追加してください

## gh-pages ブランチ作成

`gh-pages`ブランチは[orphan branch](https://git-scm.com/docs/git-checkout/#git-checkout---orphanltnewbranchgt)として作成します

```
$ git checkout --orphan gh-pages
$ git reset --hard
$ git commit --allow-empty -m "Initializing gh-pages branch"
$ git push origin gh-pages
$ git checkout master
```

> ファイルが大量に消えたり再生成したりするので、プロジェクト配下のファイルはすべて閉じておくことを推奨します


## Build and Deployment

`gh-pages`ブランチをgitの[worktree feature](https://git-scm.com/docs/git-worktree)を使って`public`フォルダにチェックアウトします。  
worktreeは複数の同じローカルリポジトリのブランチを持って別のディレクトリにチェックアウトさせることを可能にします。

```
$ rm -rf public
$ git worktree add -B gh-pages public origin/gh-pages
```

`hugo`コマンドを使ってサイトを再生成し、生成したファイルを`gh-pages`ブランチにコミットしてください

```
$ hugo
$ cd public && git add --all && git commit -m "Publishing to gh-pages" && cd ..
```

もしローカルの`gh-pages`ブランチの変更が問題なければ、それらをリモートリポジトリにpushします

```
$ git push origin gh-pages
```

### GitHub Pages 設定

`gh-pages`ブランチをパブリッシュブランチとして使用するため、GitHub のUIでリポジトリの構成を行う必要があります。GitHubがこのブランチを作成したことを認識すると、これはおそらく自動で行われます。GitHubプロジェクト内から自動でブランチをセットすることも可能です。


1. **Settings** → **GitHub Pages**
2. **Source** から、"gh-pages ブランチ"を選択して **Save** します。もしオプションが有効でない場合、まだブランチが作成されていないかローカルマシンからホストリポジトリへのpushが完了していないかもしれません

[![Image from Gyazo](https://i.gyazo.com/626b36b1383b2c3ffe70e32c650cb1b4.png)](https://gyazo.com/626b36b1383b2c3ffe70e32c650cb1b4)

[![Image from Gyazo](https://i.gyazo.com/5b86accad25dded869e505375606d383.png)](https://gyazo.com/5b86accad25dded869e505375606d383)

## スクリプトで自動化

上記の一連の手順（GitHubでの設定を除く）を行うスクリプトファイルを用意すると便利です。[こちら](https://github.com/yoshimune/hugotest/blob/master/publish_to_ghpages.sh)を参考にスクリプトファイルを用意してみてください。

このスクリプトはpublishまでは完了させますが、リモートへのpushは行っていません。リモートへのpushは手動で行ってください。
