# アーキテクチャ図の作成と埋め込み
- アーキテクチャ図の正本はStructurizr DSLとする。
- arc42文書には、Structurizr DSLから生成した図を画像として埋め込む。
- arc42本文では、DSL断片を主たる図表現として直接埋め込まない。

## 配置
- 図の生成物は、対応するAntora module配下の`assets/diagrams/`に置く。
- Structurizr DSLのソースは、対応するAntora module配下の`assets/diagrams/source/`に置く。
- 1つの図を更新するときは、まずDSLソースを更新し、その結果として生成画像を更新する。

## 埋め込み形式
- arc42文書へ埋め込む図の標準形式は`svg`とする。
- 標準生成経路は、`Structurizr DSL/JSON -> Structurizr export -format svg -> Antora assets -> image::...[]`とする。
- PlantUML / C4-PlantUML経由は、生成される図の見た目が目的に合わないため標準経路にしない。
- SVGを標準生成物とし、PNGは必要時の補助出力に留める。
- CI上の標準実行環境は、Playwright依存を含むStructurizr Docker imageを第一候補にする。
- ローカル検証では、Playwright同梱のStructurizr `.war`を使ってもよい。
- 図は該当するAntoraページ（`docs/modules/<module>/pages/*.adoc`）で生成画像を埋め込むことを基本にする。
- AntoraページではAsciiDocの`image::...[]`で生成画像を埋め込む。
- `README.md`では必要な代表図だけを扱ってよい。
- 必要なら対応するDSLソースへのリンクを併記してよい。

## `!include` の扱い
- `!include`を使う場合は、対応するAntora moduleの`assets/diagrams/source/`配下のDSLソースツリー内で相対参照が完結するように構成する。
- 生成画像の置き場やarc42本文ファイルを`!include`の対象にしない。
