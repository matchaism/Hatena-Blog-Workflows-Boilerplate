---
Title: JavaScriptからTypeScriptへ / maccha Advent Calendar 2024
Category:
- Tech
- Advent Calendar
Date: 2024-12-15T20:13:35+09:00
URL: https://macchanism.hateblo.jp/entry/2024/12/maccha-advent-calendar-2024/day15
EditURL: https://blog.hatena.ne.jp/macchanism/macchanism.hateblo.jp/atom/entry/6802418398311827176
---

[maccha Advent Calendar 2024](https://adventar.org/calendars/10199)の15日目の記事です．

<img src="https://cdn-ak.f.st-hatena.com/images/fotolife/m/macchanism/20241215/20241215191712.png" width="50%" />

最近，少しずつTypeScriptの勉強を始めました．同時に，これまでJavaScriptで開発していたプロジェクトをTypeScriptに書き直しています．

<!-- more -->

例えばこちらのプロジェクトです．

[https://github.com/matchaism/jpbank-coin-handling-fee-simulator/tree/main:embed:cite]

以前からTypeScriptに興味はありましたが，学び始めるきっかけとなったのは，「型を意識することの重要性」を改めて実感したからです．

### TypeScriptの第一印象

TypeScriptは，JavaScriptに型やクラスの概念を加えた言語という印象を持っています．特に便利だと思うのは，コンパイル時にコードの問題に気付けることです．JavaScriptでは，実行して初めてエラーに気付くことが多々ありますが，TypeScriptだとそれが事前に防げるので非常に助かります．VSCode上でエラーが可視化されるだけでも，開発効率が向上するのを実感しています．

### 動的型付け言語の課題

私は研究でPythonを常用しており，日常的に動的型付け言語にまみれています．しかしコードが長くなったり，久しぶりにコードを読み返す際に，変数や関数，クラスの内容を把握することに苦労することが増えてきました．例えば，自作のクラスやライブラリを使うとき，引数や返り値の型がわからず，結果としてコードリーディングに時間を取られることが多いのです．「誰よその関数！」という状態ですね．

コメントを充実させることで解決できる場合もありますが，実際にはコード量が増えるほどコメントの記述は後回しにされがちです．それに対して，型宣言はコーディング中に自然と行うものなので，記述漏れが少ないのが利点です．型さえわかれば，関数やクラスの詳細な中身を覚えていなくても再利用がしやすくなります．

### 型の恩恵

普段，私たちは様々なライブラリやクラスを利用してコーディングをしていますが，全ての挙動を詳細に把握しているわけではありません．それでも，引数や返り値の型，役割がわかっていれば問題なく使えます．これと同じ原理ですね(多分)．

そのため，私はPythonでも積極的に型ヒントを活用しています．この恩恵は自分だけでなく，将来コードを引き継ぐ後輩たちにも及ぶはずです．いわば早めの引継ぎ作業ですね．

### TypeScriptを選んだ理由

話をTypeScriptに戻します．最近，過去にJavaScriptで開発したコードを見直したり，他の人のコードを読む機会がありました．時間が経つと，自分が書いたコードですら信頼性や可読性に疑問を持つことがあります．また，他人からコードを引き継いだり，逆に提供する際にも，コードの意図や仕様を明確に伝える必要性を感じました．

こうした経験やPythonでの課題意識も相まって，今回TypeScriptを本格的に学び始めることにしました．
