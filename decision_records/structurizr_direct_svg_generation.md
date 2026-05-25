# Structurizr直接SVG生成を標準経路にする理由

## 決定
- アーキテクチャ図の標準生成経路は、Structurizr DSL/JSONからStructurizrの`export -format svg`で直接SVGを生成する方式にする。
- PlantUML / C4-PlantUML経由は標準経路にしない。

## 理由
- PlantUML / C4-PlantUML経由で生成した図の見た目は、Fの利用目的では許容範囲外だった。
- Structurizrのブラウザベースレンダラで生成するSVGの方が、Structurizrで確認する図の見た目を維持しやすい。
- 現行StructurizrはPNG/SVG exportを提供しており、DSLまたはJSON workspaceからSVGを生成できる。
- PNG/SVG exportはPlaywright依存を持つため、CIではPlaywright依存を含むStructurizr Docker imageを第一候補にする。

## 残した論点
- CIで使う具体的なコマンド、Docker image tag、成果物配置手順は別途決める。
- 手動レイアウトを維持する場合のレイアウト情報管理は、`decision_records/structurizr_diagram_layout_json.md`で後続決定として扱う。

## 参考
- https://docs.structurizr.com/export/png-and-svg
- https://docs.structurizr.com/binaries
