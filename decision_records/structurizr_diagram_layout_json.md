# Structurizr図のレイアウト情報をJSONで管理する理由

## 決定
- Structurizr図のモデル正本は`workspace.dsl`とする。
- 手動レイアウトを含むStructurizr JSON workspaceは、`workspace.json`として管理する。
- SVG生成時は、手動レイアウトを再現するために`workspace.json`を`export -format svg -workspace <path>`の入力にする。

## 理由
- StructurizrのPNG/SVG exportはDSLまたはJSON workspaceを入力にできる。
- 手動レイアウトを使う場合はJSON workspaceが必要になるため、DSLだけでは見た目を安定して再現しにくい。
- 一方で、モデル変更の出発点をJSONに寄せると、図モデルの正本が重くなり、DSLを正本にする既存方針と衝突する。
- そのため、DSLをモデル正本に残し、JSONはレイアウト付きexport入力として管理する。

## 残した論点
- DSL変更をJSONレイアウトworkspaceへ反映する同期手順は別途決める。
- CIで使う具体的なStructurizrコマンド、Docker image tag、成果物配置手順は別途決める。

## 参考
- https://docs.structurizr.com/export/png-and-svg
- https://docs.structurizr.com/binaries
