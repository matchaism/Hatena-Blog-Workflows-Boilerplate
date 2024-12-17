---
Title: WaniCTF 2024 Writeup
Category:
- CTF
- Tech
Date: 2024-06-25T23:51:52+09:00
URL: https://macchanism.hateblo.jp/entry/wanictf2024-writeup
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

```python
p = getPrime(64)
q = getPrime(64)
r = getPrime(64)
s = getPrime(64)
a = getPrime(64)
n = p*q*r*s*a
e = 0x10001

FLAG = b'FLAG{This_is_a_fake_flag}'
m = bytes_to_long(FLAG)
enc = pow(m, e, n)
print(f'n = {n}')
# n = 317903423385943473062528814030345176720578295695512495346444822768171649361480819163749494400347
print(f'e = {e}')
# e = 65537
print(f'enc = {enc}')
# enc = 127075137729897107295787718796341877071536678034322988535029776806418266591167534816788125330265
```

RSA暗号のような暗号化をしている．わかっている情報は公開鍵$(n,e)$と暗号文$enc$である．この暗号文を復号することで得られる平文がフラグである．

### 解法

方針として，まずRSA暗号と似た暗号化をしているので，復号もRSA暗号と同様であると予想した．また素数$p, q, r, s, a$が64bitであることから，算出できると予想した．

まず先に素数$p, q, r, s, a$は，下記サイトで取得した．このサイトには素因数分解の結果がまとめられている．

[http://www.factordb.com/:embed:cite]

あとはRSA暗号の復号アルゴリズムをそのまま転用することで，フラグを獲得できる．

```python
n = 317903423385943473062528814030345176720578295695512495346444822768171649361480819163749494400347
e = 65537
enc = 127075137729897107295787718796341877071536678034322988535029776806418266591167534816788125330265

p0 = 9953162929836910171
p1 = 11771834931016130837
p2 = 12109985960354612149
p3 = 13079524394617385153
p4 = 17129880600534041513

phi = (p0-1)*(p1-1)*(p2-1)*(p3-1)*(p4-1) # phi = (p-1)*(q-1)
d = extend_gcd(e, phi) # eとphiが互いに素のとき，e*d=1 (mod phi)となるd (mod phi)が拡張ユークリッドの互除法によって計算できる

m_hat = pow(enc, d, n) # オイラーの定理より，C^d = m (mod n)

FLAG_hat = long_to_bytes(m_hat)
print(FLAG_hat)
```

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

```python
key = b'the_enc_key_is_'
iv = b'my_great_iv_is_'
key += urandom(1)
iv += urandom(1)

cipher = AES.new(key, AES.MODE_CBC, iv)
FLAG = b'FLAG{This_is_a_dummy_flag}'
flag_hash = hashlib.sha256(FLAG).hexdigest()

msg = pad(FLAG, 16)
enc = cipher.encrypt(msg)

print(f'enc = {enc}') # bytes object
# enc = b'\x16\x97,\xa7\xfb_\xf3\x15.\x87jKRaF&"\xb6\xc4x\xf4.K\xd77j\xe5MLI_y\xd96\xf1$\xc5\xa3\x03\x990Q^\xc0\x17M2\x18'
print(f'flag_hash = {flag_hash}') # str object
# flag_hash = 6a96111d69e015a07e96dcd141d31e7fc81c4420dbbef75aef5201809093210e
```

上記のAES暗号(モドキ？)から得られた暗号文`enc`とフラグのハッシュ値`flag_hash`から平文を求め，フラグを取得する．

### 解法

鍵`key`と初期化ベクトル`iv`は末尾の1バイトしかランダムに変化しない．それぞれ256通り(`\x00 ~ \xff`)しかないので，短時間で全てのパターンについて復号を試みることができる．つまり，鍵`key`と初期化ベクトル`iv`を決めて暗号文を復号する処理を，全ての鍵`key`と初期化ベクトル`iv`の組で実行することでフラグを入手できる．

取得したフラグが正解かどうかは，フラグのハッシュ値`flag_hash`と取得したフラグのハッシュが一致するか確認すればよい．

```python
enc = b'\x16\x97,\xa7\xfb_\xf3\x15.\x87jKRaF&"\xb6\xc4x\xf4.K\xd77j\xe5MLI_y\xd96\xf1$\xc5\xa3\x03\x990Q^\xc0\x17M2\x18'
flag_hash = '6a96111d69e015a07e96dcd141d31e7fc81c4420dbbef75aef5201809093210e'

all_bytes = bytes(range(256))

for byte0 in all_bytes:
    key = b'the_enc_key_is_'
    key += byte0.to_bytes()

    for byte1 in all_bytes:
        iv = b'my_great_iv_is_'
        iv += byte1.to_bytes()

        cipher = AES.new(key, AES.MODE_CBC, iv)
        msg = cipher.decrypt(enc)

        if bytes(msg[:5]) == b'FLAG{':
            tail_idx = msg.index(b'}')
            FLAG = msg[:tail_idx+1]

            if hashlib.sha256(FLAG).hexdigest() == flag_hash:
                print('correct:', FLAG)
                print(' - key', key)
                print(' - iv', iv)
```

## [Crypto] replacement

```python
from secret import cal

enc = []
for char in cal:
    x = ord(char)
    x = hashlib.md5(str(x).encode()).hexdigest()
    enc.append(int(x, 16))
        
with open('my_diary_11_8_Wednesday.txt', 'w') as f:
    f.write(str(enc))

enc = [265685380796387128074260337556987156845, 75371056103973480373443517203033791314, ..., 301648155472379285594517050531127483548]
```

テキストファイルを1文字ずつmd5でハッシュ化している．元のテキストファイルを復元することでフラグを取り出す．

### 解法

まずキーボードで入力できる全ての文字について，同様のハッシュ化を行い，平文字とハッシュ化後の文字の対応を調べる．そして，この対応テーブルから復号を繰り返すだけである．

```python
pattern = '1234567890-^\qwertyuiop@[asdfghjkl;:]zxcvbnm,./\\!"#$%&()=~|QWERTYUIOP`{ASDFGHJKL+*}ZXCVBNM<>?_ ' + "'"

enc_pattern = dict()
for char in pattern:
    x = ord(char)
    x = hashlib.md5(str(x).encode()).hexdigest()
    enc_pattern[int(x, 16)] = char

enc = [265685380796387128074260337556987156845, 75371056103973480373443517203033791314, ..., 301648155472379285594517050531127483548]

msg = ''
for i in enc:
    m = enc_pattern.get(i, '<?>')
    msg += m

print(msg)
```

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

