---
Title: WaniCTF 2024 Writeup
Category:
- CTF
- Tech
Date: 2024-06-25T23:51:52+09:00
URL: https://macchanism.hateblo.jp/entry/2024/06/wanictf-2024-writeup
EditURL: https://blog.hatena.ne.jp/macchanism/macchanism.hateblo.jp/atom/entry/6801883189117120720
---

<script>
MathJax = {
  tex: {inlineMath: [['$', '$'], ['\\(', '\\)']]}
};
</script>
<script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js"></script>

大阪大学CTFサークル[Wani Hackase](https://wanictf.org/about/)が開催する，WaniCTF2024に参加した．

5人チームで参加し，順位は97位(2168pt)であった．

<figure class="figure-image figure-image-fotolife" title="成績">[f:id:macchanism:20240625203210p:plain]<figcaption>成績</figcaption></figure>

私が解いたのは，Cryptoのbeginners_rsa，beginners_aes，replacementとForensicsのSurveillance_of_susである．

私が解いた4問のwriteupを書いていこうと思う．急ぎ足で執筆したので，誤りや説明が荒い箇所があると思う．

<!-- more -->

<span style="font-size: 150%">目次</span>

[:contents]

## [Crypto] beginners_rsa

<script src="https://gist.github.com/matchaism/bc4d96676d5ee1743d9991a5fc90e34f.js?file=hatena-wanictf2024-writeup-code.py"></script>

RSA暗号のような暗号化をしている．わかっている情報は公開鍵$(n,e)$と暗号文$enc$である．この暗号文を復号することで得られる平文がフラグである．

### 解法

方針として，まずRSA暗号と似た暗号化をしているので，復号もRSA暗号と同様であると予想した．また素数$p, q, r, s, a$が64bitであることから，算出できると予想した．

まず先に素数$p, q, r, s, a$は，下記サイトで取得した．このサイトには素因数分解の結果がまとめられている．

[http://www.factordb.com/:embed:cite]

あとはRSA暗号の復号アルゴリズムをそのまま転用することで，フラグを獲得できる．

<script src="https://gist.github.com/matchaism/bc4d96676d5ee1743d9991a5fc90e34f.js?file=hatena-wanictf2024-writeup-code2.py"></script>

### 原理

補助資料として，RSA暗号について確認する．下記サイトを参考にしている．

[https://falconctf.hatenablog.com/entry/2019/09/30/212910:embed:cite]

<span style="font-size: 120%">暗号化</span>

1. 素数$p, q$の決定 ($N=pq$)
2. $ \phi (N)=(p-1)(q-1)$と互いに素である自然数$e$を設定
3. $ed \equiv 1 \pmod{\phi(N)}$となる$d$を計算
4. $enc \equiv m^{e} \pmod{N}$で暗号化

<span style="font-size: 120%">復号</span>

1. $\hat{m} \equiv enc^{d} \pmod{N}$で復号

復号ができる理由はオイラーの定理にある．

<span style="font-size: 120%">オイラーの定理</span>

自然数$N$と互いに素となる$N$以下の自然数の個数を$\phi(N)$とする．
$a$と$N$が互いに素のとき

$$
a^{\phi(N)} \equiv 1 \pmod{N}
$$

が成り立つ．

ここで，素数$p$のときは$\phi(p)=p-1$であり，さらに素数$p,q$については$\phi(pq)=(p-1)(q-1)$となることは，多分自明である．

復号のときの仕組みは下記の通りである．

\begin{split} 
enc^{d} & \equiv (m^{e})^{d} \pmod{N} \\\
             & \equiv m^{ed} \\\
             & \equiv (m^{k \phi(N) +1}) \quad (\because ed \equiv 1 \pmod{\phi(N)} \quad \therefore ed=k \phi(N) +1 (k \in \mathbb{R})) \\\
             & \equiv m \times (m^{k \phi(N)}) \\\
             & \equiv m \quad (\because (m^{k})^{\phi(N)} \equiv 1 \pmod{N})
\end{split}

以上がRSA暗号の復号の仕組みである．

今回の問題では素数が5個($p, q, r, s, a$)に増えた．このとき$\phi(pqrsa)=(p-1)(q-1)(r-1)(s-1)(a-1)$となるが，フェルマーの定理は成立するので，RSA暗号と同じ復号が適用できる．

## [Crypto] beginners_aes

<script src="https://gist.github.com/matchaism/bc4d96676d5ee1743d9991a5fc90e34f.js?file=hatena-wanictf2024-writeup-code3.py"></script>

上記のAES暗号(モドキ？)から得られた暗号文`enc`とフラグのハッシュ値`flag_hash`から平文を求め，フラグを取得する．

### 解法

鍵`key`と初期化ベクトル`iv`は末尾の1バイトしかランダムに変化しない．それぞれ256通り(`\x00 ~ \xff`)しかないので，短時間で全てのパターンについて復号を試みることができる．つまり，鍵`key`と初期化ベクトル`iv`を決めて暗号文を復号する処理を，全ての鍵`key`と初期化ベクトル`iv`の組で実行することでフラグを入手できる．

取得したフラグが正解かどうかは，フラグのハッシュ値`flag_hash`と取得したフラグのハッシュが一致するか確認すればよい．

<script src="https://gist.github.com/matchaism/bc4d96676d5ee1743d9991a5fc90e34f.js?file=hatena-wanictf2024-writeup-code4.py"></script>

## [Crypto] replacement

<script src="https://gist.github.com/matchaism/bc4d96676d5ee1743d9991a5fc90e34f.js?file=hatena-wanictf2024-writeup-code5.py"></script>

テキストファイルを1文字ずつmd5でハッシュ化している．元のテキストファイルを復元することでフラグを取り出す．

### 解法

まずキーボードで入力できる全ての文字について，同様のハッシュ化を行い，平文字とハッシュ化後の文字の対応を調べる．そして，この対応テーブルから復号を繰り返すだけである．

<script src="https://gist.github.com/matchaism/bc4d96676d5ee1743d9991a5fc90e34f.js?file=hatena-wanictf2024-writeup-code6.py"></script>

## [Forensics] Surveillance_of_sus

与えられたキャッシュファイル(バイナリファイル)からフラグを見つけ出す．

### 解法

まずバイナリを見ると，マジックナンバー`52 44 50 38 62 6d 70`が目に入った．これはこのファイルが`RDP Bitmap Cache`であることを指す．

[f:id:macchanism:20240625230802p:plain]

[f:id:macchanism:20240625230726p:plain]

[https://jpn.nec.com/cybersecurity/blog/231006/index.html:title]によれば，RDP Bitmap Cacheの解析には[https://github.com/ANSSI-FR/bmc-tools:title]を利用するらしい．このBMC-Toolsはキャッシュデータから画像を抽出するものである．

BMC-Toolsによって，画面の画像が分割して出力された．

[f:id:macchanism:20240625231443p:plain]

あとはパズルの要領で，分割された画像軍の中からフラグの部分を並べて復元するだけである．

[f:id:macchanism:20240625231653p:plain]

一文字ずつ変換しているので，元の文字が簡単に推定できることがポイントであった．

## 感想
今回，miscが1問も解けなかったのが悔しくてしょうがない．

JQ Playgroundはインジェクション攻撃のように，フラグの内容を出力させるコマンドを注入するのかと考えた．しかし入力できるクエリの長さが短いため，不可能だった．

他の参加者のwriteupでは，`jq`コマンドのオプションを活用する解放が見られた．クエリが短いという制約下では，`jq`コマンドそのものを悪用することを考察すべきだったと反省した．

## 参考
 - [https://zenn.dev/anko/articles/ctf-crypto-rsa:title]
 - [http://www.factordb.com/:title]
 - [https://falconctf.hatenablog.com/entry/2019/09/30/212910:title]
 - [https://www.slideshare.net/slideshow/rsa-n-ssmjp/72368516:title]

- [https://d415k.github.io/ctf/cheatsheets/forensics/magic_number:title]
- [https://jpn.nec.com/cybersecurity/blog/231006/index.html:title]
- [https://github.com/ANSSI-FR/bmc-tools:title]

