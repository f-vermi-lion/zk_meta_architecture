# Structurizr直接SVG生成を標準経路にする理由

## 決定
- アーキテクチャ図の標準生成経路は、Structurizr DSL/JSONからStructurizrの`export -format svg`で直接SVGを生成する方式にする。
- PlantUML / C4-PlantUML経由は標準経路にしない。
- CI上の標準Docker imageは`structurizr/structurizr:2026.05.22-playwright`とする。
- CIの標準入力は、手動レイアウトを含む`workspace.json`とする。
- 生成SVGは、Antora assetとしてリポジトリ管理する。

## 理由
- PlantUML / C4-PlantUML経由で生成した図の見た目は、Fの利用目的では許容範囲外だった。
- Structurizrのブラウザベースレンダラで生成するSVGの方が、Structurizrで確認する図の見た目を維持しやすい。
- 現行StructurizrはPNG/SVG exportを提供しており、DSLまたはJSON workspaceからSVGを生成できる。
- PNG/SVG exportはPlaywright依存を持つため、CIではPlaywright依存を含むStructurizr Docker imageを第一候補にする。
- `2026.05.22-playwright`はPlaywright依存を含む公式Docker tagであり、初期のCI標準として固定しやすい。
- 生成SVGをリポジトリ管理すると、Antora build時に外部生成手順へ依存せず、レビュー時に図の差分も確認しやすい。

## 残した論点
- Structurizr Docker image tagの更新方針は別途決める。
- 生成SVGの再生成差分をCIで検出する具体実装は別途決める。

## 参考
- https://docs.structurizr.com/export/png-and-svg
- https://docs.structurizr.com/binaries
