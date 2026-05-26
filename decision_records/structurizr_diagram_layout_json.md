# Structurizr図のレイアウト情報をJSONで管理する理由

## 決定
- Structurizr図のモデル正本は`workspace.dsl`とする。
- 手動レイアウトを含むStructurizr JSON workspaceは、`workspace.json`として管理する。
- SVG生成時は、手動レイアウトを再現するために`workspace.json`を`export -format svg -workspace <path>`の入力にする。
- DSL変更後は、既存`workspace.json`のレイアウト情報を反映した状態で`workspace.json`を更新する。
- レイアウト保持のため、Structurizr viewsには明示的で安定したview keyを付ける。
- view keyは`<view-type>-<target>[-<purpose>]`形式の英小文字kebab-caseにする。
- `view-type`の標準語彙は、`system-context`、`container`、`component`、`dynamic`、`deployment`、`filtered`、`image`、`custom`とする。

## 理由
- StructurizrのPNG/SVG exportはDSLまたはJSON workspaceを入力にできる。
- 手動レイアウトを使う場合はJSON workspaceが必要になるため、DSLだけでは見た目を安定して再現しにくい。
- 一方で、モデル変更の出発点をJSONに寄せると、図モデルの正本が重くなり、DSLを正本にする既存方針と衝突する。
- そのため、DSLをモデル正本に残し、JSONはレイアウト付きexport入力として管理する。
- Structurizrのレイアウトマージは要素やviewを対応付ける必要があるため、view keyを明示して安定させることで、図全体のレイアウト喪失を避けやすくなる。
- view keyは手動レイアウト保持の安定識別子なので、表示タイトルやarc42章番号のような変わりやすい情報ではなく、view種別と対象の意味に基づくslugで命名する。
- 既存view keyの変更はJSON側のレイアウト対応を壊しやすいため、命名規則は新規viewの標準とし、既存keyの変更は慎重に扱う。

## 残した論点
- view keyを変更する場合のJSONレイアウト移行手順は別途決める。
- Structurizr Docker image tagの更新方針は別途決める。
- 生成SVGの再生成差分をCIで検出する具体実装は別途決める。

## 参考
- https://docs.structurizr.com/export/png-and-svg
- https://docs.structurizr.com/binaries
- https://docs.structurizr.com/dsl/basics
- https://docs.structurizr.com/dsl/language
