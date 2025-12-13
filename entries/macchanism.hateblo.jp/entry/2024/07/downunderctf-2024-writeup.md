---
Title: DownUnderCTF 2024 Writeup
Category:
- CTF
- Tech
Date: 2024-07-10T02:37:53+09:00
URL: https://macchanism.hateblo.jp/entry/2024/07/downunderctf-2024-writeup
EditURL: https://blog.hatena.ne.jp/macchanism/macchanism.hateblo.jp/atom/entry/6801883189119936767
---

DownUnderCTFに，3人チームで参加した．

[f:id:macchanism:20240708030222j:plain]

<!-- more -->

私が解いたのは

* [crypto] Sun Zi's Perfect Math Class
* [crypto] shufflebox
* [misc] tldr please summarise
* [misc] DNAdecay
* [forensics] Baby's First Forensics
* [forensics] SAM I AM
* [osint] offtheramp
* [osint] Bridget Lives
* [osint] cityviews
  * 9割は仲間のおかげ
* [web] parrot the emu
* [web] zoo feedback form

である．

<span style="font-size: 150%">目次</span>
[:contents]

公式のwriteupと異なる解き方をした問題を執筆する．

## [crypto] Sun Zi's Perfect Math Class
公式のソルバーとやっていることは近いと思う．

[https://github.com/DownUnderCTF/Challenges_2024_Public/blob/main/beginner/sunzi-perfect-math-class/solve/solve.py:embed:cite]

私はこのサイトのコードを利用した．

[https://www.ochappa.net/posts/rsa-hba:embed:cite]


## [crypto] shufflebox
文字列を暗号化(シャッフル)するプログラムは以下の通りである．

<script src="https://gist.github.com/matchaism/54a5fd8c38308665747520f85d99910d.js?file=hatena-downunderctf2024-writeup-code.py"></script>

### writeup
ソルバーは以下の通りである．

<script src="https://gist.github.com/matchaism/54a5fd8c38308665747520f85d99910d.js?file=hatena-downunderctf2024-writeup-code2.py"></script>

シャッフルするテーブル `PERM` が秘密情報であるので，その特定から始める．以下を繰り返すことで， `PERM` が手に入る．

1. 平文と暗号文の組 `(aaaabbbbccccdddd, ccaccdabdbdbbada)` から，あり得る `PERM` を見つける
2. `PERM` が平文 `abcdabcdabcdabcd` から 暗号文 `bcaadbdcdbcdacab` を生成するかチェック

最後に暗号文 `owuwspdgrtejiiud` から平文を復号することで，フラグを獲得する．

## [forensics] SAM I AM

公式のwriteupと同じ解き方で取り組んだ．

[https://github.com/DownUnderCTF/Challenges_2024_Public/blob/main/forensics/samiam/solve/WRITEUP.md:embed:cite]

`hashcat` でクラックに取り組み，ワードリストに `rockyou.txt` を採用した．解き方はChatGPTに聞いたら教えてくれた．

<script src="https://gist.github.com/matchaism/54a5fd8c38308665747520f85d99910d.js?file=hatena-downunderctf2024-writeup-code3.sh"></script>

## [web] parrot the emu

[https://github.com/DownUnderCTF/Challenges_2024_Public/tree/main/beginner/parrot-the-emu:embed:cite]

Flaskの `render_template_string` が怪しいと感じたので，`flask 2.0.3 render_template_string` でGoogle検索し，脆弱性やCTFにおける攻撃シナリオを調べた．

[https://blog.hamayanhamayan.com/entry/2021/12/15/225142:embed:cite]

このサイトを参考に

 `{{request.application.__globals__.__builtins__.__import__('os').popen('ls -lah').read()}}` 

を入力した．

[f:id:macchanism:20240706212259p:plain]

どうやらシェルコマンドを実行できるらしいので，素直にフラグを `cat` した．

[f:id:macchanism:20240706212329p:plain]

## [web] zoo feedback form

[https://github.com/DownUnderCTF/Challenges_2024_Public/tree/main/beginner/zoo-feedback-form:embed:cite]

`etree.XMLParser(resolve_entities=True)` が怪しいと感じ，脆弱性や攻撃シナリオを調べた．

[https://nanimokangaeteinai.hateblo.jp/entry/2024/02/14/120000:embed:cite]

上記サイトで，XMLといえば "XML External Entity" と言及があった．特に `resolve_entities=True` は危険な引数の設定らしい．

[https://book.hacktricks.xyz/pentesting-web/xxe-xee-xml-external-entity:embed:cite]

このサイトを参考に具体的な攻撃方法を調べ，実行した．

<script src="https://gist.github.com/matchaism/54a5fd8c38308665747520f85d99910d.js?file=hatena-downunderctf2024-writeup-code4.sh"></script>

