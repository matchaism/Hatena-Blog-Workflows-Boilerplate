---
Title: VPSのセットアップスクリプトを作った
Category:
- Tech
Date: 2023-12-15T00:00:00+09:00
URL: https://macchanism.hateblo.jp/entry/2023/12/uec-advent-calendar-2023
EditURL: https://blog.hatena.ne.jp/macchanism/macchanism.hateblo.jp/atom/entry/6801883189055329419
---

Advent Calendar 2023の15日目の記事です．師走も残り半分じゃい．

[https://adventar.org/calendars/8698:embed:cite]

昨日の担当はgottiさんでした．最近，ブログを自分で立ち上げる人が散見される気がします．

[https://gotti.dev/post/golang-blog/index.html:embed:cite]

# > whoami
<span style="font-size: 150%">お抹茶おいしいね！一寸先はyummy~~~~~!!!</span>

[f:id:macchanism:20231211005312p:plain]

M1の抹茶と申します．Advent Calendar 2020から毎年参加しています．2021のアドカレは私が建てました．(たくさんのご参加ありがとうございました)

学域時代はM〇〇((現役組のテックスキルが強い))で競プロをやっていました．万年茶色コーダーとして，毎週AtCoderのコンテストに参加しておりました．また，新歓や書記を担当したり部誌を書いたり，賑やかしをしていました．

それ以外だとV〇〇((なんか凄いことになっていますね))でちょっとなんかしてたり((Hello, Worker ありがとおおおおお))しなかったりですね．古の記憶過ぎます．今年のMIKUECも楽しみです．<s>全通したいと思います．</s>全通しました!!!!!!感動をありがとう!!!!!!

どちらもほとんどオンラインで活動しておりました．あと数年経てば，コロナ禍の学生生活のことを誰も話題にしなくなるでしょう．どちらのサークルにも栄光あれ．

<!-- more -->

# VPSのセットアップスクリプトを作った
本題です．皆さんお持ちの(はずの)マイサーバに関する話題です．私はConoHaのVPSを利用しており，主にVPNと簡単なAPIサーバが稼働しております．

定期的にサーバをリセットしてOSを入れ直す運用をしているので，その後のセットアップを自動化したいと考えました．前述のサービスはDocker上で走っているため復帰は簡単なのですが，サーバに関する基本的な設定はこれまで手動で進めていました．しかし設定ミスや設定漏れがあると，リセットのハードルが高くなりますし，セキュリティ的にも危ないです．

## 実装
そういう経緯で今回，セットアップスクリプトを作ることにしました．<s>ゼロから作るのは面倒だった</s>効率よく開発を進めたいので，[先人のリポジトリ](https://github.com/jasonheecs/ubuntu-server-setup)をForkして改変しました．元のスクリプトでは下記の設定をしてくれます．

- アカウント作成
- パスワード認証の禁止
- ルートログインの禁止
- Firewallの設定
- タイムゾーンの設定

Fork後の改変では，下記の機能を追加しました．

 - SSHポートの変更
 - 任意のスクリプトの実行

私はサーバへのログインされた時の通知やFail2Banがブロックした時の通知がスマホに届くように設定するスクリプトを走らせています．

[https://github.com/matchaism/ubuntu-server-setup:embed:cite]

実際に使うときは`git clone`してセットアップスクリプトを実行すると，勝手に設定を進めてくれます．`private/`に別途走らせたいスクリプトを置くと，続けて実行してくれます．今回のスクリプトはUbuntuでbashを利用しているという前提で記述されているので，他の環境の方は修正するか，各々が作ってください．

##  余談
SSHポートをデフォルト(22)から変えておくと，かなり不正アクセスが減るのでおすすめです．Fail2Banと併せて設定しておきましょう．Firewallの設定変更もお忘れなく．

# おわりに
Twitterは鍵をかけていますが，電通大生であればフォロリクを通しますので，どしどしフォローしてください→ [@macchaakamaccha](https://twitter.com/macchaakamaccha)

抹茶のなんちゃってポートフォリオはこちら:

[https://portal.matchaism.net/:embed:cite]


明日は淵野アタリさんの担当です．お楽しみに．

[https://adventar.org/calendars/8698:embed:cite]
