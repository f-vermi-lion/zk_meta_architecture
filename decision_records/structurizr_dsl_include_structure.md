# Structurizr DSLの分割単位と!include配置規則を決めた理由

## 決定
- 標準では、1つのAntora moduleにつき1つのStructurizr workspaceを置く。
- 標準入口は`docs/modules/<module>/examples/diagrams/workspace.dsl`とする。
- 小規模なうちは、DSLを分割せず`workspace.dsl`に集約する。
- DSL分割が必要になった場合は、`examples/diagrams/`直下に`model.dsl`、`views.dsl`、`styles.dsl`を置き、`workspace.dsl`から相対パスで`!include`する。
- さらに大きい場合だけ、`examples/diagrams/model/`や`examples/diagrams/views/`配下へ細分化する。
- `!include`は`examples/diagrams/`配下のDSLソースツリー内に限定し、`pages/`、`partials/`、`assets/images/`、別module、リポジトリ外ファイルは参照しない。

## 理由
- 1 module 1 workspaceを標準にすると、Antora module、Structurizr workspace、生成SVGの対応が追いやすい。
- 最初から細かく分割すると、図が少ない段階で認知負荷が増える。
- `workspace.dsl`を入口に固定すると、CIやローカル検証の入力が安定する。
- `model.dsl`、`views.dsl`、`styles.dsl`の分割は、Structurizr DSLの責務に沿っており、差分確認もしやすい。
- `!include`の参照範囲をDSLソースツリー内に閉じることで、Antoraページ、生成画像、外部ファイルとの依存が混ざることを避けられる。

## 残した論点
- 1 module内に複数Structurizr workspaceを置く必要が出た場合の命名規則は別途決める。
- DSL分割後にStructurizr JSON workspaceへ再同期する具体コマンド手順は別途決める。

## 参考
- https://docs.structurizr.com/dsl/language
