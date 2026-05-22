# 日英版をAntora moduleで分ける理由

## 決定
- 造智機巧のアーキテクチャ文書の日英版は、同一component内のAntora moduleで分ける。
- 英語正本は`ROOT` module、日本語補助版は`ja` moduleに置く。

## 理由
- Antoraの`version`はcomponent versionであり、文書本文の版番号や言語差分として使うと既存のversion廃止方針と衝突する。
- 言語ごとにcomponentを分けると、同じ対象システムの文書が別componentとして見え、component単位の集約や参照が分かれすぎる。
- 同一ページ内に日英を併記すると、英語正本・日本語補助版の差分管理と章単位の編集が重くなる。
- moduleで分けると、同じcomponent内で日英のページ構造を揃えられ、公開時の対応確認もしやすい。
