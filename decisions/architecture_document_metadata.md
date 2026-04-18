# アーキテクチャ文書のメタデータ
- arc42文書のメタデータの正本は、各文書ディレクトリの`index.adoc`に置くAsciiDocヘッダ属性とする。
- このメタデータ規則は、造智機巧のアーキテクチャ文書に適用する。
- 英語正本・日本語補助の運用に合わせて、各文書は`:lang:`属性で文書言語を明示する。

## 必須項目
- 文書タイトルを1行目の`= Title`で書く。
- `:docstatus:`
- `:created:`
- `:revdate:`
- `:authors:`
- `:lang:`

## 記法
- 文書タイトルは1行目の`= Title`形式にする。
- `:docstatus:`は`draft`、`reviewed`、`published`のいずれかにする。
- `:created:`は初回作成日にする。
- `:revdate:`は最新版の識別にも使う最新更新日にする。Markdown前提の`updated`に相当する項目は、AsciiDocでは`:revdate:`に置き換える。
- `:created:`と`:revdate:`は`YYYY-MM-DD`形式にする。
- `:authors:`は著者名を`;`区切りで並べる。
- `:lang:`は`en`または`ja`にする。
- 明示的なversion番号は持たないため、`:revnumber:`は使わない。

## 例
```adoc
= System Context
:docstatus: draft
:created: 2026-04-18
:revdate: 2026-04-18
:authors: yafoo
:lang: en
```
