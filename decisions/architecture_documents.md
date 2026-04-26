# アーキテクチャ文書の形式
- テンプレートはarc42。
- 単一のarc42で詳細まで全て表すのでなく階層的に複数のarc42を書く。

## 配置規則
- 階層的なarc42文書は、文書単位ごとに1ディレクトリを割り当てる。
- 子文書は親文書ディレクトリの直下に子ディレクトリとして置き、親`README.md`から辿れるようにする。
- 各文書ディレクトリの`README.md`は、GitHub上での文書概要と公開ページへの入口に使う。
- 各文書ディレクトリの`index.adoc`は、公開用の親文書とする。
- `index.adoc`から`src/`配下の章ファイルをincludeして本文を構成する。
- 各文書ディレクトリでは、arc42本文を`src/`配下の章ごとのAsciiDocファイルに分けて管理する。
- 章ファイルはarc42の公式の章番号に対応させ、必要な章だけを置いてよい。
- ある文書に付随する画像や補助ファイルは、その文書ディレクトリ配下の`assets/`など、文書単位で閉じた場所に置く。
- 1つのディレクトリに複数の独立したarc42本文を混在させない。

## ディレクトリ命名規則
- arc42文書ディレクトリ名は`slug`形式にする。
- `slug`は英小文字のkebab-caseにする。
- 親子関係はディレクトリのネストで表し、子の`slug`に親文書名を重ねて含めない。
- ディレクトリ名にarc42の章番号や表示順序を持たせない。
- 文書リンクや図の参照パスは、このディレクトリ名と章ファイル名を正規の識別子として扱う。

## 章ファイル命名規則
- 各章ファイル名は`NN-chapter-slug.adoc`形式にする。
- `NN`はarc42の公式の章番号を表す2桁ゼロ埋め番号にする。
- `chapter-slug`は英小文字のkebab-caseにする。
- `README.md`自体は章ファイルとして扱わない。
- `index.adoc`自体は章ファイルとして扱わない。

## 章ファイルの標準slug
- `01-introduction-and-goals`
- `02-architecture-constraints`
- `03-context-and-scope`
- `04-solution-strategy`
- `05-building-block-view`
- `06-runtime-view`
- `07-deployment-view`
- `08-concepts`
- `09-architecture-decisions`
- `10-quality-requirements`
- `11-technical-risks`
- `12-glossary`

## arc42
[arc42](https://docs.arc42.org/home/)

## 階層的なドキュメント
[ドキュメントのモジュール化](https://faq.arc42.org/questions/J-1/)
