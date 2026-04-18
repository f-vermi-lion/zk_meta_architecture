# アーキテクチャ文書のメタデータ
- arc42文書の各`README.md`の冒頭には、YAML front matterでメタデータを置く。
- このメタデータ規則は、造智機巧のアーキテクチャ文書に適用する。
- 英語正本・日本語補助の運用に合わせて、各文書は`language`で文書言語を明示する。

## 必須項目
- `title`
- `status`
- `created`
- `updated`
- `authors`
- `language`

## 記法
- `title`は文字列にする。
- `status`は`draft`、`reviewed`、`published`のいずれかにする。
- `created`は初回作成日、`updated`は最新版の識別にも使う最新更新日にする。
- `created`と`updated`は`YYYY-MM-DD`形式にする。
- `authors`は著者名のYAML配列にする。
- `language`は`en`または`ja`にする。

## 例
```yaml
---
title: System Context
status: draft
created: 2026-04-18
updated: 2026-04-18
authors:
  - yafoo
language: en
---
```
