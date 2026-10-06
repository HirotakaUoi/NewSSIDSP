NewSSIDSP
=========

**Super Simple Introduction to the Denotational Semantics of Programs**

プログラムの表示的意味論（denotational semantics）を入門的に体験するための、小さな手続き型言語とその処理系です（Erlang 製、2013 年）。
[SSIDSP](https://github.com/HirotakaUoi/SSIDSP) の `ssidsp5_2` までの版を収めた別のリポジトリです。言語と処理系の仕組みの詳しい説明は SSIDSP の README を参照してください。

## SSIDSP との違い

- 意味の計算のモジュールは `ssidsp1.erl` 〜 `ssidsp5_2.erl` まで（スレッドなどを足した `ssidsp6` 以降は含まない）
- 文法（`grammerTest.yrl`）で、`program` で始まるプログラムに名前を付ける形にした: `program 名前 { ... } .`
- `ssidsp1.erl` の変数の削除（`del`）を修正

## 実行方法

Erlang/OTP が必要です（OTP 24 で動作を確認）。

`compTest.erl` の `semFilename()` は `ssidsp8_1` を指していますが、このリポジトリには無いので、`ssidsp5_2` に書き換えてから使ってください。

```bash
erlc grammerTest.erl compTest.erl ssidsp5_2.erl
erl
```

```erlang
1> compTest:semInputByFileNamed().
FileName> nQueen2.ssidsp
Input List> [6].
```

入力リストは Erlang の項として `[6].` のように末尾に `.` を付けて入力します。
`grammerTest.yrl` を変更したときは `compTest:inputByFileNamedReparse()` でパーサ（`grammerTest.erl`）を作り直してください（このリポジトリの `grammerTest.erl` は SSIDSP のものと同じで、変更後の文法からはまだ生成されていません）。

## ファイル

| ファイル | 内容 |
|---|---|
| `grammerTest.yrl` | 文法の定義（yecc） |
| `grammerTest.erl` | yecc で生成したパーサ |
| `compTest.erl` | 字句解析・構文解析・実行をまとめて呼び出すドライバ |
| `ssidsp1.erl` 〜 `ssidsp5_2.erl` | 意味の計算の各版 |
| `*.ssidsp` | サンプルプログラム（ソート、N-Queen など） |
| `double.erl`、`ctemplate.erl` | 実験用のコード |
| `*.sublime-project`、`*.sublime-workspace` | Sublime Text の設定 |

## 背景のメモ

いわゆる「原始的」ソフトウエア
- キーボードから入力して、画面に「文字で」出力する
- ビットマップでも「描画命令を出力」と考えればよい

手続き型プログラム
- 実行が逐次的に行われる
- 条件判断、繰り返しがある
- GOTO（無条件分岐）…も？

表示的意味論
- プログラム（の断片）を「意味を表す」関数に変換する
- どんな関数か？
  - 環境（Environment）・文脈（Context）と呼ばれるもの
  - 変数（名）から その値（未定義も含む）への関数
- ボトムアップに変換する
  - 式、文といった断片から全体を合成する
