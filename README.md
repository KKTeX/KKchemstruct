# KKchemstruct

高校化学（有機・無機）の構造式を組むための LaTeX パッケージです。
描画そのものは [chemfig](https://ctan.org/pkg/chemfig) が行い、本パッケージは
参考書の図版の見えに合うよう寸法・線幅・向きを設定したうえで、
置換ベンゼン・示性式・反応式・注釈を短い記法で書けるようにします。

寸法はすべて `em` 指定なので、本文の文字サイズを変えれば図もそれに追随します。
判型ごとに数値を書き直す必要はありません。

*A LaTeX package for the structural formulas of high-school chemistry.
See [the English manual](./doc/KKchemstruct-manual-en.pdf) for full documentation.*

## 取扱説明書 / Manual

- [日本語版 (PDF)](./doc/KKchemstruct-manual-ja.pdf)
- [English (PDF)](./doc/KKchemstruct-manual-en.pdf)

原稿は `doc/` の `.tex`（`jlreq` クラス）です。

## 導入 / Installation

`KKchemstruct.sty` を TeX のサーチパス上か、文書と同じディレクトリに置いて読み込みます。

```latex
\usepackage{KKchemstruct}
```

パッケージオプションはありません。

- TeX エンジン: LuaLaTeX、または pLaTeX + dvipdfmx
- 依存パッケージ: `chemfig`, `etoolbox`, `xkeyval`（いずれも TeX Live に収録）

## 使用例 / Examples

```latex
\KKbenzene[2=OH,3=COOH]                 % サリチル酸（位置は真上を 1 として時計回り）
\KKchemchain{H-O-S(\KKdative{2}O)(\KKdative{6}O)-O-H}   % 硫酸（配位結合つき）
\KKscheme{\KKsalicylicacid\arrow{->[\ck{CH_3OH}][エステル化]}\KKmethylsalicylate}
```

## 主なマクロ / Main macros

| マクロ | 用途 |
| --- | --- |
| `\KKbenzene[2=OH,3=COOH]` | 置換ベンゼン。位置は真上を 1 として時計回りに 1〜6。`angle=`（回転）、`sep=`（一辺）、`kekule=b`（二重結合の反転）も指定可。 |
| `\KKphenyl{COOH}` / `\KKphenylene{SO_3H}` / `\KKphenylring` | 本文中に流し込む横向き（flat-top）の環。 |
| `\KKchemring[<chemfigキー>]{...}` | 縮合環など、chemfig の生の記法で書く環（原子中心間距離が一定）。 |
| `\KKchemchain[<chemfigキー>]{...}` | 示性式。可視線の長さが一定になるので `CH_2` のような広いラベルでも潰れません。 |
| `\KKcarbonyl` / `\KKcarbonyldown` / `\KKdative{<角度>}` | 鎖の中で使う断片（上下向きの C=O、配位結合の矢印）。 |
| `\KKscheme{...}` / `\KKbranchscheme{...}` | 反応式と、1 つの基質から 2 つの生成物へ分かれる反応式。 |
| `\KKname{<構造>}{<名称>}` | 構造式の下に化合物名を添える。 |
| `\KKchemmark{<節点>}{<節点>}` / `\KKhbond` / `\KKchemcross` | 脱離部分の点線囲み・水素結合の点線・「反応しない」を示す×印。 |
| `\ck{<式>}`（`\KKchemformula`）、`\ckm` / `\ckp` | 数式モードに入らない場所（矢印ラベルなど）で使う化学式とイオンの右肩。 |
| `\KKchemsetup{ring sep=1.35em,...}` | 寸法の一括変更。グループ内で呼べば変更はそのグループに閉じます。 |

`\KKsalicylicacid`、`\KKphthalicanhydride`、`\KKtriglyceride{R_1}{R_2}{R_3}` など、
頻出化合物のマクロも同梱しています（`.sty` の末尾の節）。

## 注意 / Notes

- `\KKchemmark` と `\KKhbond` は TikZ のノード参照を使うため、位置が定まるまでに
  2 回コンパイルが要ります。
- chemfig では丸括弧が枝分かれの記法です。`CH_3(CH_2)_{11}` のような
  「構造を描かない化学式」は `\ck{...}` で書いてください。
- 反応矢印のラベルは chemfig の仕様で数式モードに入りません。化学式を置くときは
  `\ck{CH_3OH}` のように包みます。和文はそのまま書けます。

## 動作確認 / Tests

`tests/kkchemstruct-regression.tex` は、置換ベンゼン、横向きの環、示性式、縮合環、
注釈、反応式、寸法の一括変更を一通り確認します。注釈の位置決めのため 2 回実行してください。

```sh
TEXINPUTS=.: lualatex -output-directory=/tmp tests/kkchemstruct-regression.tex
TEXINPUTS=.: platex -kanji=utf8 -output-directory=/tmp tests/kkchemstruct-regression.tex
dvipdfmx -o /tmp/kkchemstruct-regression.pdf /tmp/kkchemstruct-regression.dvi
```

出力例は `tests/output/` に置いてあります。

説明書を組み直すときも同様です。

```sh
TEXINPUTS=.: lualatex -output-directory=/tmp doc/KKchemstruct-manual-ja.tex
```

## ライセンス / License

[MIT License](./LICENSE.md).

このパッケージはもともと [KKTeX/Publicated-Files](https://github.com/KKTeX/Publicated-Files)
の一部として公開していたものを、履歴ごと分離したものです。
