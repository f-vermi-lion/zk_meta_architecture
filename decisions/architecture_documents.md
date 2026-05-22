# アーキテクチャ文書の形式
- テンプレートはarc42。
- 単一のarc42で詳細まで全て表すのでなく階層的に複数のarc42を書く。

## 配置規則
- 各ソースリポジトリのアーキテクチャ文書は、Antora標準構成で`docs/`配下に置く。
- Antora component descriptorは`docs/antora.yml`に置く。
- Antoraのナビゲーションファイルは各moduleの`nav.adoc`に置く。
- 文書に独立した版番号を持たせないcomponentでは、`docs/antora.yml`の`version`に`~`を指定する。
- 英語正本のarc42本文は`docs/modules/ROOT/pages/`配下にAsciiDocファイルとして置く。
- 日本語補助版のarc42本文は`docs/modules/ja/pages/`配下にAsciiDocファイルとして置く。
- 再利用する断片は、対応するmoduleの`partials/`配下に置く。
- 画像や補助ファイルは、対応するmoduleの`assets/`配下に置く。
- GitHub上の`README.md`は入口として残すが、arc42本文の正本としては扱わない。
- 子文書はAntoraのページ階層またはモジュール構成で表し、親文書から辿れるようにする。
- 章ファイルはarc42の公式の章番号に対応させ、必要な章だけを置いてよい。
- 1つのページ群に複数の独立したarc42本文を混在させない。

## 文書・ページ命名規則
- arc42文書や子文書を表すページディレクトリ名は`slug`形式にする。
- `slug`は英小文字のkebab-caseにする。
- 親子関係はディレクトリのネストで表し、子の`slug`に親文書名を重ねて含めない。
- ディレクトリ名にarc42の章番号や表示順序を持たせない。
- 文書リンクや図の参照パスは、Antoraのresource IDとページファイル名を正規の識別子として扱う。

## 章ファイル命名規則
- 各章ファイル名は`NN-chapter-slug.adoc`形式にする。
- `NN`はarc42の公式の章番号を表す2桁ゼロ埋め番号にする。
- `chapter-slug`は英小文字のkebab-caseにする。
- `README.md`自体は章ファイルとして扱わない。
- `index.adoc`は文書トップページとして扱い、必要に応じて章ファイルへの導線を置く。

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

## 相互参照
- Antora上のページ間参照は`xref`を基本にする。
- 再利用断片の取り込みはAntoraのpartialとAsciiDoc includeを基本にする。
- リポジトリ内外の参照は、相対パスではなくAntoraのresource IDで表すことを優先する。
- `pages/`配下のサブディレクトリに置いたページを参照する場合は、`xref:architecture-documentation-publishing-platform/index.adoc[]`のように、moduleの`pages/` rootからのresource IDで表す。

## arc42
[arc42](https://docs.arc42.org/home/)

## 階層的なドキュメント
[ドキュメントのモジュール化](https://faq.arc42.org/questions/J-1/)
