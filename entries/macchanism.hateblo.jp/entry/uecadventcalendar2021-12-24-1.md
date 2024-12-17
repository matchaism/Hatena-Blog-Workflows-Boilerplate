---
Title: stzrで宇宙へ行った話
Category:
- Tech
Date: 2021-12-24T21:00:00+09:00
URL: https://macchanism.hateblo.jp/entry/uecadventcalendar2021-12-24-1
EditURL: https://blog.hatena.ne.jp/macchanism/macchanism.hateblo.jp/atom/entry/13574176438045581725
---

最終更新 2021/12/26

---

この記事はUEC Advent Calendar 2021の24日目の記事になります。

[https://adventar.org/calendars/6400:embed:cite]

前回はMMAの聡明担当こと、azarasingくんの記事でした。

[https://azarasing.hatenablog.com/entry/2021/12/22/234123:embed:cite]

彼は調布祭実行委員で技術班としても大活躍でした。

---

目次はこちら

[:contents]

## まえがき

UEC Advent Calendar 2021の主催者こと、抹茶と申します。参加している皆さま、および閲覧している皆さま、本当にありがとうございます。昨年と同様、Advent Calendarが2枚目に渡るという大盛況となりました。今年も素晴らしい記事に恵まれ、参加者の皆さまには感謝の思いでいっぱいです。閲覧している電通大生も、来年は是非参加してみては？

さてさて、話は変わって本日は12/24、クリスマス・イヴです。イヴですよ〜、イヴ！諸君はクリスマスとクリスマス・イブの違い、わっかるかなぁ？クリスマスの意味がわからない人のために、Advent Calendarには「クリスマス」のWikipediaページのリンクを貼っておいたけど、念の為イヴの方も貼ったほうがいいかなぁ？？

[https://ja.wikipedia.org/wiki/%E3%82%AF%E3%83%AA%E3%82%B9%E3%83%9E%E3%82%B9%E3%83%BB%E3%82%A4%E3%83%B4:embed:cite]

あと、イヴは「イブ」ではなく、「イヴ」(Eve)ですよ！上手に発音できるかなぁ？？

### Advent Calendarって何？

Advent Calendarってそもそも何という方は、こちらをご覧下さい。(今更ですが)

[https://ja.wikipedia.org/wiki/%E3%82%A2%E3%83%89%E3%83%99%E3%83%B3%E3%83%88%E3%82%AB%E3%83%AC%E3%83%B3%E3%83%80%E3%83%BC:embed:cite]

UEC Advent Calendarでは、12/1から12/25まで一人ずつ自由に記事を投稿していただければOKです。技術系の話題が並ぶQiitaなどと違い、テーマは自由です。

<!-- more -->

## 自己紹介

私は電通大3年の抹茶と名乗る者です。[MMA](https://wiki.mma.club.uec.ac.jp/)では書記を担当しております。[VLL](https://mikuec.com/)にも所属しているという噂もありますが、なんのことやら。

普段からTwitterに生息しており、くだらないことばかりつぶやいております。<s>[#抹茶のガンギマリレコード](https://twitter.com/search?q=%23抹茶のガンギマリレコード)</s>

あと、絵師様の絵を見ることが好きです。大好きです。絵師様の絵を毎朝拝見するまでは、布団から出ません。体調が悪い時にも拝見します。時には画集や同人誌を持ち歩いています。この大学にも絵師様がいるようで、胸がアツいです。絵師様が絵を描いていることこそが、私の喜び。

---

ここからが本題。ブラウザバックするなら、今のうち。

## stzrで宇宙へ行く

この記事の内容は、先日開催されたDentoo.LT#26と重複します(新要素もありますが)。見ていない方はこちら。リアルで私が発表しております。

[https://youtu.be/BNBpVStc4-Q?t=5249:embed:cite]

### はじめに

私はお酒を他の方と一緒に飲んだことがありません。飲酒時は談笑することもなく、かといってお酒を一人美味しく楽しむことをしておりません。私の一人飲みの目的は**キマること**、これだけです。

そんな私でも、流石に昼間からストゼ○を飲むことはしません。講義や課題、業務中に飲酒するわけにはいかないです。人にはやって良いことと悪いことがあります。

それでも、昼間からストゼ○をプシュッ！と開けたい、と思うことはあります。どうやらこれは私だけではないようです。

[https://twitter.com/macchaakamaccha/status/1470793220800258051:embed]

遠隔講義が主体となり、またリモートワークの普及により、常に手が届く場所にお酒があります。こんな状況で、作業に集中できるでしょうか。私以外にもそういう人はいるでしょう。今こそ、改革をせねばなりません。

### stzr

作業中、ストゼ○が飲めなくても、開けることはできる...

[f:id:macchanism:20211224160639p:plain]

そう、ターミナル上で...

[f:id:macchanism:20211223172029p:plain]

ここには、全国プランク定数人のストゼ○erの夢が詰まっている。stzrは、ターミナル上でstzrを開けるだけのコマンドです。ストゼ○erの皆さまのために私が作りました。

[https://github.com/macchanism/stzr:embed:cite]

オプションをつけることで、お好みの味をお楽しみいただけます。

[f:id:macchanism:20211223173123p:plain]

え？プルタブが勝手に開くのが気に入らない？でしたら`./stzr -p`を試してください。キーボード入力をトリガーに、缶が開きます。

もちろんロング缶もご用意しております、`./stzr -l`でお楽しみください。

[f:id:macchanism:20211223173308p:plain]

### 汚ねぇ花火だ

ストゼ○はあなたを宇宙へ連れて行ってくれます。

スペースシャトル(350ml)のふわふわ宇宙旅行には、`./stzr -s`をご利用ください。

[f:id:macchanism:20211224123759p:plain]

ロケット(ロング缶)によるガンギマリ宇宙旅行には、`./stzr -r`を是非ご利用こください。

[f:id:macchanism:20211224123823p:plain]

聖夜の闇空にストゼ○を解き放とうじゃないか。僕らの忘れられないクリスマスの幕開けだ。

### おわりに

実用性のないコマンドのことをジョークコマンドと呼びます。もちろん、このstzrコマンドもジョークコマンドの一つです。

stzrコマンドのリポジトリ(GitHub)はこちらから(再掲)。

[https://github.com/macchanism/stzr:embed:cite]

**お酒は20歳になってから** (stzrコマンドはどなたでも無料でご利用いただけます)

以上。

---

どうやら私は本日、[UEC Advent Calendar 2021 その2](https://adventar.org/calendars/6598)も担当するみたいです、忙しい。

[https://macchanism.hateblo.jp/entry/uecadventcalendar2021-12-24-2:embed:cite]

次回はいよいよUEC Advent Calendar 2021の最終日です。お届けするのは、我がMMAの代表もとい、アイドルのTerryニキです。こちらからどうぞ！！！

[https://terry-uec.hatenablog.com/entry/2021/12/25/235649:embed:cite]

