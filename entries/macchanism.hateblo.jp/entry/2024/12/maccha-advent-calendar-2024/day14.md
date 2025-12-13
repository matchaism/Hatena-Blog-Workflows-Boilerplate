---
Title: 生体認証の用語を早く統一化してほしい / maccha Advent Calendar 2024
Category:
- Advent Calendar
- Misc
- Tech
Date: 2024-12-31T15:23:27+09:00
URL: https://macchanism.hateblo.jp/entry/2024/12/maccha-advent-calendar-2024/day14
EditURL: https://blog.hatena.ne.jp/macchanism/macchanism.hateblo.jp/atom/entry/6802418398308851853
---

[maccha Advent Calendar 2024](https://adventar.org/calendars/10199)の14日目の記事です．遅めの投稿になります．

## はじめに

本記事は著者独自の解釈を含む内容になっております．また，取り扱う内容は執筆時点での情報になります．専門家の解釈や最新の情報と異なる可能性にご注意ください．

<!-- more -->

## 生体認証の用語を取り巻く現状

生体認証における"authentication"(認証)，"identification"(識別)，"verification"(検証)，"recognition"(認識)という用語は，文献間でその意味に一貫性がなく用いられることがありました [1]．ここ数年，国際的な取り組み[2]によりこの状況は変わりつつありますが，その流れに日本はまだキャッチアップできていないようです．

### 進む国際標準

近年，国際的な標準化組織(ISO/IEC JTC 1/SC 37)により，生体認証の用語が規定されています．"authentication"，"identification"，"verification"，"recognition"の標準化された定義[2]を表にまとめます (独自解釈による翻訳を含む)．

|用語|定義|
|---|---|
|authentication|act of proving or showing to be of undisputed origin or veracity<br/>→ 真に本人であること，または主張が真実であることを証明すること (biometric identification や biometric verification と同義ではない)|
|biometric identification|process of searching against a biometric enrolment database to find and return the biometric reference identifier(s) attributable to a single individual<br/>→ 被認証者と生体情報が最も同一とされる人物を登録データ内で検索すること|
|biometric verification|process of confirming a biometric claim through comparison<br/>→ 被認証者が名乗っている人物と，被認証者との生体情報が同一か確かめること|
|biometric recognition (=biometrics)|automated recognition of individuals based on their biological and behavioural characteristics<br/>→ 生体情報に基づいて被認証者を機械的に認識すること (biometric identification と biometric verification の意味を包含する)|

上表のように，ISO/IEC 2382-37:2022では生体認証の用語に対する標準化が進んでいます．

### 日本の現状

しかし2024年10月現在，対応するJISの制定が追い付いていません [3]．

[4]によれば，日本語における「認証」，「識別」，「検証」は下表で定義されます．広義の「認証」は「識別」と「検証」の意味を含むが，狭義の場合は「検証」を指すことが多いです．

|用語|定義|
|---|---|
|(広義の)認証|「識別」と「検証」の 2 つの意味を包含|
|識別<br/>(identification)|提示された被認証者の特徴を示す情報を全ての登録情報と比較し，最も類似している人物を探し出す処理 (1:N 照合)|
|検証<br/>(verification)|提示された被認証者の特徴を示す情報と，被認証者が名乗る人物の登録情報を比較し，一致するか判定する処理 (1:1 照合) (狭義の「認証」に該当)|

各用語を英語・日本語で翻訳するにあたり，辞書的には"authentication"は「認証」と，"recognition"は「認識」と訳されます．しかし"face recognition"を「顔認証」と訳すように，生体認証においては必ずしも辞書上の対応関係に当てはまらない場合があります．

### 注意すべきこと

ISO/IEC 2382-37:2022で定義された用語を，辞書的に訳してそのまま利用するべきではないです．JISによる規格が制定されるまでは，日本語における生体認証の用語の意味は一意に定まりません．

そのため生体認証に関する議論をする際，各用語の定義を明らかにする必要があると考えられます．いっそ英単語そのままで議論・記述した方が安全かもしれません．

また，既存の生体認証に関する文献を読む際は，用語の意味を確認しておいた方が良いです．

## おわりに

繰り返しになりますが，本記事は専門家の解釈や最新の情報と異なる可能性があります．

生体認証の用語は国際標準化が進んでいます．日本語の対応もそのうち始まるでしょう．それまでは注意して用語を利用しましょう．

## 参考文献・リンク

<small>

[1] James L. Wayman., "Biometric Verification/Identification/Authentication/Recognition: The Terminology," Springer US, 2015.

[2] ISO/IEC JTC 1/SC 37, "Information technology ― vocabulary ― part 37: Biomet-
rics," https://standards.iso.org/ittf/PubliclyAvailableStandards/index.html, 2022/3. 最終閲覧日 2024/10/31.

[3] 一般財団法人日本規格協会 (JSA), "JIS 作成予定 (一覧表)(制定案)," https://webdesk.jsa.or.jp/pdf/jisnintei/seiteian_202410.pdf, 最終閲覧日 2024/10/31.

[4] 一般社団法人日本自動認識システム協会, "よくわかる生体認証," オーム社, 2019.

</small>
