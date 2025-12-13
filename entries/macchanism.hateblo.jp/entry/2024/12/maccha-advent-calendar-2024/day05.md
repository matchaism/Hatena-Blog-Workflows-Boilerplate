---
Title: Google Apps ScriptのソースコードをGitHubで管理し，claspによるデプロイ作業をGitHub Actionsで自動化する /
  maccha Advent Calendar 2024
Category:
- Tech
- Advent Calendar
Date: 2024-12-06T01:00:00+09:00
URL: https://macchanism.hateblo.jp/entry/2024/12/maccha-advent-calendar-2024/day05
EditURL: https://blog.hatena.ne.jp/macchanism/macchanism.hateblo.jp/atom/entry/6802418398309310586
---

この記事は[maccha Advent Calendar 2024](https://adventar.org/calendars/10199)の5日目の記事です．

タイトルや内容的にQiitaに投稿かなと思ったけど，既に同じテーマの記事が散見され2番煎じになるので，ここに投下します．

GASで動かすソースコードをGitHubで管理し，GASへのデプロイをGitHub Actionsで自動化しました．

<!-- more -->

下記記事を参考にしましたが，私の開発環境に合った改変&2024年12月最新の方法としての価値は多少あると思います．

* [https://qiita.com/yuzinet/items/17be9967b2c660ee8432:title]
* [https://qiita.com/shunexe/items/fdf0def390a160d044c3:title]
* [https://dev.classmethod.jp/articles/vscode-clasp-setting/:title]

## さくっと本題

やり方を手短に説明します．

[f:id:macchanism:20241205232837p:plain]

### 1. claspの認証情報の取得

1. ローカルにnode.jsをインストール
2. claspをインストール : `npm install -g @goole/clasp`
3. Googleアカウントにログイン : `clasp login`
4. ログインが完了すると，認証情報が手に入る (`~/.clasprc.json`)

### 2. Google Apps Script APIを有効化

1. [https://script.google.com/home/usersettings:title]にアクセス
2. GASのAPIを有効化

### 3. プロジェクトの認証情報を取得

1. GASのプロジェクトを作成，開く
2. スクリプトIDを取得 : `https://script.google.com/home/projects/<SCRIPT_ID>]`
3. 「新しいデプロイ」を作成 :  GASのプロジェクト > デプロイ > 新しいデプロイ > 「ウェブアプリ」のデプロイを作成
3. デプロイIDを取得 : GASのプロジェクト > デプロイ > デプロイを管理 > デプロイID

### 4. (必要に応じて)今のプロジェクトのソースコードをダウンロード

1. スクリプトIDを取得 : `https://script.google.com/home/projects/<SCRIPT_ID>]`
2. GASのソースコード(JavaScript)や`appsscript.json`をダウンロード : `clasp clone <SCRIPT_ID>`

### 5. gitの準備

1. `git init` or `git clone`など，ローカルリポジトリの作成，git環境の準備 (詳細は省く)
2. `src/`にGASのソースコード(JavaScript)や`appsscript.json`を配置

### 6. 認証情報をActions secretsに保存

1. `<repository_url>/settings/secrets/actions`にアクセス
2. Repository secretsに，機密性の高い各種認証情報を保存

|入手方法|認証情報|
|---|---|
|手順1|`ACCESS_TOKEN`, `REFRESH_TOKEN`, `SCOPE`, `TOKEN_TYPE`, `ID_TOKEN`, `EXPIRY_DATE`, `CLIENT_ID`, `CLIENT_SECRET`, `REDIRECT_URI`, `IS_LOCAL_CREDS`|
|手順2|`DEPLOYMENT_ID`, `SCRIPT_ID`|

保存したsecretは次の手順で呼び出します．名前に気を付けて，登録してください．

### 7. `deploy.yml`の作成

下記YAMLファイルを作成し，`.github/workflows/deploy.yml`に保存します．

<script src="https://gist.github.com/matchaism/34341e50e2c7f6be4d238dff7eafc30c.js?file=hatena-maccha_advent_calendar_2024_day5-code.yml"></script>

### 8. `clasprc.json`と`clasp.json`を生成するスクリプトを作成

前述のyamlのワークフロー終盤で必要になります．

<script src="https://gist.github.com/matchaism/34341e50e2c7f6be4d238dff7eafc30c.js?file=hatena-maccha_advent_calendar_2024_day5-code2.sh"></script>

<script src="https://gist.github.com/matchaism/34341e50e2c7f6be4d238dff7eafc30c.js?file=hatena-maccha_advent_calendar_2024_day5-code3.sh"></script>

## 最後に

あとは開発を進め，deployブランチにpushすることで，適宜GASへのデプロイを自動実行してくれるはずです．ざっくりしすぎた説明ですが，概ねこんな感じで上手くいきました．

今回は下記サイトを参考に進め，修正しつつ実装しました．下記サイトに従ったうえで不足した点を，この記事で埋めていただけたらと思います．

* [https://qiita.com/yuzinet/items/17be9967b2c660ee8432:title]
* [https://qiita.com/shunexe/items/fdf0def390a160d044c3:title]
* [https://dev.classmethod.jp/articles/vscode-clasp-setting/:title]

最後にこの自動デプロイを利用したGitHubリポジトリを掲載します．このプロジェクトについては，後日記事にします．

[https://github.com/matchaism/adventar_bell:embed:cite]
