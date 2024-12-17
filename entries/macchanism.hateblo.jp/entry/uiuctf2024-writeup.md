---
Title: UIUCTF 2024 Writeup
Category:
- CTF
- Tech
Date: 2024-07-08T03:00:43+09:00
URL: https://macchanism.hateblo.jp/entry/uiuctf2024-writeup
EditURL: https://blog.hatena.ne.jp/macchanism/macchanism.hateblo.jp/atom/entry/6801883189117943973
---

<script>
MathJax = {
  tex: {inlineMath: [['$', '$'], ['\\(', '\\)']]}
};
</script>
<script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js"></script>

[https://2024.uiuc.tf/:embed:cite]

今回は1人で参加し，959位中267位であった．

[f:id:macchanism:20240704235619p:plain]

<!-- more -->

今回解いた問題は

* [Crypto] X Marked the Spot
* [Crypto] Without a Trace
* [Crypto] Determined
* [Crypto] Naptime
* [OSINT] Hip With the Youth
* [OSINT] An Unlikely Partnership
* [OSINT] Night
* [Misc] Sanity Check

であった．

<span style="font-size: 150%">目次</span>
[:contents]

## [Crypto] X Marked the Spot
下記プログラムで暗号化されたフラグを復号する．ほかにフラグの暗号文が書き込まれたファイル `ct` も事前配布されている．

```python
from itertools import cycle

flag = b"uiuctf{????????????????????????????????????????}"
# len(flag) = 48
key  = b"????????"
# len(key) = 8
ct = bytes(x ^ y for x, y in zip(flag, cycle(key)))

with open("ct", "wb") as ct_file:
    ct_file.write(ct)

# ct = 1d0d1c121600111f5810361b17531e2e1c0c5a2e11125e031c3b0b0416395e1d5d5436050a5535420600485043474b0c
```

### writeup
長さ8の暗号鍵 `key` とフラグ `flag` の各文字を，先頭から順にXORすることで暗号化する． `flag` は末尾の文字まで暗号化に使用されると，再び先頭の文字から使われる．( `cipher[i] <- flag[i] XOR key[i%8]` )

ここでフラグの形式は `uiuctf{????????????????????????????????????????}` のため，先頭7文字と末尾の48文字目は既知である．したがって，この既知の平文 `uiuctf{}` と，暗号文の先頭7文字と末尾の48文字目( `48%8=0` )を各文字ごとにXORすることで，暗号鍵 `key` を全て獲得することができる．

あとは暗号文の全文に対し，求めた暗号鍵 `key` で復号してやればよい．

ソルバーを以下に掲載する．

```python
l = []
with open('ct', 'rb') as f:
    l = f.readlines()
crypt = [a[b] for a in l for b in range(len(a))]
print(crypt)

key = ''
for i in range(7):
    key += chr(crypt[i] ^ ord('uiuctf{'[i]))
key += chr(crypt[-1] ^ ord('}'))
print(key)

msg = ''
for i in range(48):
    msg += chr(crypt[i] ^ ord(key[i % 8]))
print(msg)
```

## [Crypto] Without a Trace
`f_i <- bytes_to_long(bytes(FLAG[5*i:5*(i+1)]))` ( `i=0...4` )として，

$$
F = 
\begin{bmatrix}
   f_0 & 0 & 0 & 0 & 0 \\\
   0 & f_1 & 0 & 0 & 0 \\\
   0 & 0 & f_2 & 0 & 0 \\\
   0 & 0 & 0 & f_3 & 0 \\\
   0 & 0 & 0 & 0 & f_4
\end{bmatrix}
$$

$m_i$ はユーザが指定できる任意の整数として，

$$
M = 
\begin{bmatrix}
   m_0 & 0 & 0 & 0 & 0 \\\
   0 & m_1 & 0 & 0 & 0 \\\
   0 & 0 & m_2 & 0 & 0 \\\
   0 & 0 & 0 & m_3 & 0 \\\
   0 & 0 & 0 & 0 & m_4
\end{bmatrix}
$$

この問題ではサーバにアクセスし， $\left( m_0, m_1, m_2, m_3, m_4 \right)$ を渡すと

$$
\rm{trace}\left( FM \right) = f_0 m_0 + f_1 m_1 + f_2 m_2 + f_3 m_3 + f_4 m_4
$$

が得られる．ただし $\det{M} \ne 0$ でないといけない．

様々な $\left( m_0, m_1, m_2, m_3, m_4 \right)$ を用いて， $f_0, f_1, f_2, f_3, f_4$ を求める．

### writeup
まず $f_0$ を知るには $|m_0 - m_0'|=1$ となるような $m_0, m_0'$ と， $m_1, m_2, m_3, m_4$ (共通)を考える．その後， $\left( m_0, m_1, m_2, m_3, m_4 \right)$ と $\left( m_0', m_1, m_2, m_3, m_4 \right)$ をそれぞれサーバに渡して， $\rm{trace}\left( FM \right)$ と $\rm{trace}\left( FM' \right)$ を獲得する．最後に両者の差を計算することで $f_0$ が手に入る．

$$
\begin{array}{rrr}
   & f_0 m_0 &+ f_1 m_1 + f_2 m_2 + f_3 m_3 + f_4 m_4 \\\
-) & f_0 m_0' &+ f_1 m_1 + f_2 m_2 + f_3 m_3 + f_4 m_4 \\\
\hline
 & f_0 \left( m_0 - m_0' \right) &
\end{array}
$$

$f_1, f_2, f_3, f_4$ についても同様にして求めればよい．( $\det{M} \ne 0$ に注意)

$f_0, f_1, f_2, f_3, f_4$ が手に入った後は， `f_i <- bytes_to_long(bytes(FLAG[5*i:5*(i+1)]))` と逆の手順で `FLAG` を求める．

## [Crypto] Determined
本問題ではフラグである平文をRSA暗号の仕組みを利用し，暗号化している．既知の情報として暗号文 $c$ ，公開鍵 $n(=p q)$ と $e=65535$ が与えられている．

$m_i$ はユーザが指定できる任意の整数， $p, q, r$ は素数(定数)として，

$$
M = 
\begin{bmatrix}
   p & 0 & m_0 & 0 & m_1 \\\
   0 & m_2 & 0 & m_3 & 0 \\\
   m_4 & 0 & m_5 & 0 & m_6 \\\
   0 & q & 0 & r & 0 \\\
   m_7 & 0 & m_8 & 0 & 0
\end{bmatrix}
$$

$\left( m_0, m_1, m_2, m_3, m_4, m_5, m_6, m_7, m_8 \right)$ をサーバに渡すと，行列式 $\det{M}$ を返される．

\begin{split}
\det{M} = & -p r m_2 m_6 m_8 + p q m_3 m_6 m_8 + r m_1 m_2 m_4 m_8 - q m_1 m_3 m_4 m_8 \\\
                 & - r m_1 m_2 m_5 m_7 + r m_0 m_2 m_6 m_7 - q m_0 m_3 m_6 m_7 + q m_1 m_3 m_5 m_7
\end{split}

本問題では $\left( m_0, m_1, m_2, m_3, m_4, m_5, m_6, m_7, m_8 \right)$ の様々な組合せを試しながら，秘密鍵 $p, q$ を手に入れる．

### writeup
$\left( m_0, m_1, m_2, m_3, m_4, m_5, m_6, m_7, m_8 \right) = \left( any, 1, 0, 1, 1, any, 0, 0, 1 \right)$ のとき， $\det{M} = -q$ から片方の秘密鍵 $q$ が得られる．さらに $n=p q$ ゆえ， $p$ も得られる．

秘密鍵 $p, q$ が手に入ったので，あとはRSA暗号と同様に，暗号文 $c$ を復号することで平文(フラグ)が手に入る．

## [Crypto] Naptime
以下が暗号化・復号のプログラムである．

```python
from random import randint
from Crypto.Util.number import getPrime, bytes_to_long, long_to_bytes
import numpy as np

def get_b(n):
    b = []
    b.append(randint(2**(n-1), 2**n))
    for i in range(n - 1):
        lb = sum(b)
        found = False
        while not found:
            num = randint(max(2**(n + i), lb + 1), 2**(n + i + 1))
            if num > lb:
                found = True
                b.append(num)
    return b

def get_MW(b):
    lb = sum(b)
    M = randint(lb + 1, 2*lb)
    W = getPrime(int(1.5*len(b)))
    return M, W

def get_a(b, M, W):
    a_ = []
    for num in b:
        a_.append(num * W % M)
    pi = np.random.permutation(list(i for i in range(len(b)))).tolist()
    a = [a_[pi[i]] for i in range(len(b))]
    return a, pi

def enc(flag, a, n):
    bitstrings = []
    for c in flag:
        # c -> int -> 8-bit binary string
        bitstrings.append(bin(ord(c))[2:].zfill(8))
    ct = []
    for bits in bitstrings:
        curr = 0
        for i, b in enumerate(bits):
            if b == "1":
                curr += a[i]
        ct.append(curr)
    return ct

def dec(ct, a, b, pi, M, W, n):
    # construct inverse permuation to pi
    pii = np.argsort(pi).tolist()
    m = ""
    U = pow(W, -1, M)
    ct = [c * U % M for c in ct]
    for c in ct:
        # find b_pi(j)
        diff = 0
        bits = ["0" for _ in range(n)]
        for i in reversed(range(n)):
            if c - diff > sum(b[:i]):
                diff += b[i]
                bits[pii[i]] = "1"
        # convert bits to character
        m += chr(int("".join(bits), base=2))
    return m

def main():
    flag = 'uiuctf{I_DID_NOT_LEAVE_THE_FLAG_THIS_TIME}'

    # generate cryptosystem
    n = 8
    b = get_b(n)
    M, W = get_MW(b)
    a, pi = get_a(b, M, W)

    # encrypt
    ct = enc(flag, a, n)

    # public information
    print(f"{a =  }")
    print(f"{ct = }")

    # decrypt
    res = dec(ct, a, b, pi, M, W, n)

if __name__ == "__main__":
    main()

# a =  [66128, 61158, 36912, 65196, 15611, 45292, 84119, 65338]
"""
ct = [
    273896, 179019, 273896, 247527, 208558, 227481, 328334, 179019,
    336714, 292819, 102108, 208558, 336714, 312723, 158973, 208700,
    208700, 163266, 244215, 336714, 312723, 102108, 336714, 142107,
    336714, 167446, 251565, 227481, 296857, 336714, 208558, 113681,
    251565, 336714, 227481, 158973, 147400, 292819, 289507
]
"""
```

非常に複雑に見えるが，実は関数 `enc` に注目するだけで解答の道筋ができてしまう．

平文の各文字に対し，

 1. 各文字の文字コードを2進数8桁のビット列に変換
 2. ビット列の各ビットについて，値が1となる位置と同じ位置にある配列aの値を合計 (例: 01000011のとき，a[1]+a[6]+a[7])
 3. 合計値をその平文の文字の暗号として記録

配列aは上手くできているので，一意に暗号化・復号できる．

### writeup
ソルバーを以下に掲載する．

```python
n = 8
a = [66128, 61158, 36912, 65196, 15611, 45292, 84119, 65338]
ct = [
    273896, 179019, 273896, 247527, 208558, 227481, 328334, 179019,
    336714, 292819, 102108, 208558, 336714, 312723, 158973, 208700,
    208700, 163266, 244215, 336714, 312723, 102108, 336714, 142107,
    336714, 167446, 251565, 227481, 296857, 336714, 208558, 113681,
    251565, 336714, 227481, 158973, 147400, 292819, 289507
]

ct_to_a = dict()
for bits in range(2**8):
    s = 0
    for idx, bit in enumerate(bin(bits)[2:].zfill(8)):
        if bit == "1":
            s += a[idx]
    ct_to_a[s] = bin(bits)[2:]

msg = ""
for c in ct:
    msg += chr(int(ct_to_a[c], 2))
print(msg)
```

 1. `00000000 ~ 11111111`の各ビット列について，各ビットの値が1となる位置と同じ位置にある配列aの値を合計 (合計値からビット列を得る辞書 `ct_to_a` を作成)
 2. `ct_to_a`を用いて，既知の暗号文の各値から対応するビット列を取得 (全39個)
 3. ビット列を10進数に変換することで文字コードを獲得
 4. 文字コードから元の平文(フラグ)の文字を獲得

## [OSINT] Hip With the Youth
The Long Island Subway Authority (LISA)のInstagram → Threads → 投稿の内容にフラグ

## [OSINT] An Unlikely Partnership
The Long Island Subway Authority (LISA)のThreads → LinkedIn → スキル(1件のスキル推薦) → UIUC Chan → プロフィールにフラグ

## [OSINT] Night
Googleの画像検索でビル特定してから，ストリートビューを駆使して地道に撮影場所を特定した．撮影場所はボストン(Boston)のアーリントン・ストリート(Arlington Street)であった．

[f:id:macchanism:20240629113458p:plain]

[f:id:macchanism:20240629113742p:plain]