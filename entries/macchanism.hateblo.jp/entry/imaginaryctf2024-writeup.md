---
Title: ImaginaryCTF 2024 Writeup
Category:
- CTF
- Tech
Date: 2024-07-24T02:44:26+09:00
URL: https://macchanism.hateblo.jp/entry/imaginaryctf2024-writeup
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

```shell
$ python3 volatility3/vol.py -f dump.vmem windows.info.Info
Volatility 3 Framework 2.7.2
(中略)
Progress:  100.00               PDB scanning finished                        
Variable        Value

Kernel Base     0xf80344205000
DTB     0x1ad000
Symbols file:///(中略)
Is64Bit True
IsPAE   False
layer_name      0 WindowsIntel32e
memory_layer    1 FileLayer
KdVersionBlock  0xf80344e14400
Major/Minor     15.19041
MachineType     34404
KeNumberProcessors      1
SystemTime      2024-07-19 02:02:18
NtSystemRoot    C:\Windows
NtProductType   NtProductWinNt
NtMajorVersion  10
NtMinorVersion  0
PE MajorOperatingSystemVersion  10
PE MinorOperatingSystemVersion  0
PE Machine      34404
PE TimeDateStamp        Mon Dec  9 11:07:51 2019

$ python3 volatility3/vol.py -f dump.vmem windows.filescan | grep flag
(中略)
0xc60c81c70ce0.0\Users\imaginarypc\Documents\flag.txt   216
0xc60c81c7c540  \Users\imaginarypc\AppData\Roaming\Microsoft\Windows\Recent\flag.lnk    216

$ python3 volatility3/vol.py -f dump.vmem windows.dumpfiles --virtaddr 0xc60c81c70ce0
Volatility 3 Framework 2.7.2
(中略)
Progress:  100.00               PDB scanning finished                        
Cache   FileObject      FileName        Result

DataSectionObject       0xc60c81c70ce0  flag.txt        file.0xc60c81c70ce0.0xc60c83b5e650.DataSectionObject.flag.txt.dat

$ strings file.0xc60c81c70ce0.0xc60c83b5e650.DataSectionObject.flag.txt.dat 
aWN0ZnthYTBlYjcwN2E0MWIyY2E2fQ==
```

base64のデコード結果: `ictf{aa0eb707a41b2ca6}`

