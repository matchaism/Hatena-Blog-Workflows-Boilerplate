---
Title: ImaginaryCTF 2024 Writeup
Category:
- CTF
- Tech
Date: 2024-07-24T02:44:26+09:00
URL: https://macchanism.hateblo.jp/entry/2024/07/imaginaryctf-2024-writeup
EditURL: https://blog.hatena.ne.jp/macchanism/macchanism.hateblo.jp/atom/entry/6801883189123372264
---

ImaginaryCTF 2024に参加した．

[f:id:macchanism:20240724022257p:plain]

<!-- more -->

私が解いたのは

* [web] readme
* [web] journal
* [forensics] bom
* [forensics] packed
* [forensics] cartesian-1
* [forensics] dog-mom
* [forensics] crash
* [crypto] base64
* [crypto] integrity
* [rev] BF
* [rev] unoriginal

である．

<span style="font-size: 150%">目次</span>
[:contents]

今回特に印象に残った問題について，解説する．

## [forensics] crash
メモリダンプを解くにあたり，以下のサイトを利用した．

[https://blog.hamayanhamayan.com/entry/2022/12/14/231806:embed:cite]

メモリ解析のために利用したツールはvolatility3である．

[https://github.com/volatilityfoundation/volatility3:embed:cite]

<script src="https://gist.github.com/matchaism/6117a21e95aecb4521f99f3a4d580ccd.js"></script>

base64のデコード結果: `ictf{aa0eb707a41b2ca6}`

