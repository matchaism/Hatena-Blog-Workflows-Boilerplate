---
Title: 「オンライン自習室」の紹介
Category:
- Misc
Date: 2021-12-24T20:30:00+09:00
URL: https://macchanism.hateblo.jp/entry/2021/12/24/uec-advent-calendar-2021-2
EditURL: https://blog.hatena.ne.jp/macchanism/macchanism.hateblo.jp/atom/entry/13574176438045711258
---

この記事はUEC Advent Calendar 2021 その2の24日目の記事になります。

[https://adventar.org/calendars/6598:embed:cite]

前回はかなたさん(みなとさん)の記事でした。

[https://zenn.dev/minatoneko/articles/b4038eb6524199:embed:cite]

## 自己紹介

私は電通大3年の抹茶と名乗る者です。詳細はこちらからどうぞ。本日は本家の方も私が担当です。

[https://macchanism.hateblo.jp/entry/uecadventcalendar2021-12-24-1:embed:cite]

Twitterで気軽に絡んでくれると嬉しいです。

<!-- more -->

## オンライン自習室

先日、面白いものを発見しました。

まずはこれをみてください。

[https://youtu.be/Py62O_ifjuo:embed:cite]

オンライン自習室と聞くと、何を思い浮かべますか。利用しているサービスはなんでしょう。LINEやDiscord、Twitter(スペース機能)でのボイスチャットとかでしょうか。ここで紹介するのは、YouTube配信を使ったオンライン自習室です。実物はありませんが、座席も架空で用意されます。

私が開発した技術ではないので偉そうに語れませんが、この自習室の使い方を紹介したいと思います。

### どう使うの？

まずは配信を開いてください。([🚪📖✍ オンライン自習室 一緒に勉強 ライブ 24/7 study with me lofi hip hop](https://youtu.be/Py62O_ifjuo))

参加者(視聴者)はチャット欄にコマンドを投稿することで、操作ができます。

* `!in`: 入室する (参加する)
* `!out`: 退出する
* `!in work=hoge`: 作業名をhogeとして、入室
* `!change work=fuga`: 作業名をfugaに変更
* `!xxx`: 番号xxxの座席に着席(空いていたら)
* `!seat`: 座席番号、経過時間、自動退出までの残り時間を確認(Botがチャット欄に投稿する)

入室すると、座席に着くことになり、座席番号が割り当てられます。この座席の総数は状況に応じて、増えたり減ったりするみたいです。

配信画面には、座席状況が表示されます。各座席に誰が何をしているのかがわかります。作業名を登録しない場合は空白です。

開発リポジトリ(GitHub)はこちらから。詳しい操作方法も載っております。

[https://github.com/sorarideblog/youtube-study-space:embed:cite]

### おわりに

私はYouTubeで動画を見て、しばしば時間を無駄にしてしまうことがあるので、こうやってYouTube画面をこの配信に占有させています。

同日に2つの記事を担当した自分を呪います。

---

明日は、最終日です。UEC Advent Calendar その2では、WiZさんが担当してくれます。どんな記事が出るのか、今からわっくわくしております。
