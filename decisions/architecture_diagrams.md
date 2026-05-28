# アーキテクチャ図の作成と埋め込み
- アーキテクチャ図の正本はStructurizr DSLとする。
- arc42文書には、Structurizr DSLから生成した図を画像として埋め込む。
- arc42本文では、DSL断片を主たる図表現として直接埋め込まない。

## 配置
- 標準では、1つのAntora moduleにつき1つのStructurizr workspaceを置く。
- Structurizr DSL/JSONは、対応するAntora module配下の`examples/diagrams/`に置く。
- 標準のモデル正本は、対応するAntora module配下の`examples/diagrams/workspace.dsl`とする。
- 手動レイアウトを含むStructurizr JSON workspaceは、対応するAntora module配下の`examples/diagrams/workspace.json`に置く。
- `workspace.json`はモデル正本ではなく、Structurizrの見た目を再現するためのレイアウト付きexport入力として扱う。
- 小規模なうちは、DSLを`workspace.dsl`に集約し、分割しない。
- DSL分割が必要になった場合は、`examples/diagrams/`直下に`model.dsl`、`views.dsl`、`styles.dsl`を置き、`workspace.dsl`から`!include`する。
- さらに大きい場合だけ、`examples/diagrams/model/`や`examples/diagrams/views/`配下へ細分化する。
- 生成SVGは、対応するAntora module配下の`assets/images/diagrams/`に置く。
- 生成SVGは派生成果物だが、公開サイトでAntoraが読むassetとしてリポジトリ管理する。
- 1つの図のモデルを更新するときは、まずDSLソースを更新し、既存`workspace.json`のレイアウトを反映した状態で`workspace.json`を更新し、その結果として生成SVGを更新する。
- 手動レイアウトを使う図では、DSL変更と対応する`workspace.json`更新を同じ変更単位で扱う。
- 生成SVGの変更も、対応するDSL/JSON変更と同じ変更単位で扱う。
- レイアウトだけを調整する場合は、`workspace.json`の変更だけを許容する。
- モデル変更は必ず`workspace.dsl`から始める。
- `workspace.json`は手編集しない。StructurizrのUIまたは公式ツールのmerge/export結果として更新する。
- レイアウト保持のため、Structurizr viewsには明示的で安定したview keyを付ける。
- view keyは`<view-type>-<target>[-<purpose>]`形式の英小文字kebab-caseにする。
- `view-type`の標準語彙は、`system-context`、`container`、`component`、`dynamic`、`deployment`、`filtered`、`image`、`custom`とする。
- `target`と`purpose`は、対象システム、コンテナ、シナリオなどを表す安定したslugにする。
- 例は、`system-context-docs-site`、`container-docs-site`、`dynamic-publish-flow`、`deployment-docs-site-production`とする。
- arc42章番号や一時的な表示名はview keyに入れない。
- 既存view keyの変更は、`workspace.json`に保持した手動レイアウトを失うリスクがあるため原則として避ける。

## 埋め込み形式
- arc42文書へ埋め込む図の標準形式は`svg`とする。
- 標準生成経路は、`Structurizr DSL/JSON -> Structurizr export -format svg -> Antora images -> image::diagrams/<file>.svg[]`とする。
- 手動レイアウトを再現するSVG生成では、`workspace.json`を`export -format svg -workspace <path>`の入力にする。
- PlantUML / C4-PlantUML経由は、生成される図の見た目が目的に合わないため標準経路にしない。
- SVGを標準生成物とし、PNGは必要時の補助出力に留める。
- CI上の標準実行環境は、Playwright依存を含むStructurizr Docker imageを第一候補にする。
- ローカル検証では、Playwright同梱のStructurizr `.war`を使ってもよい。
- 図は該当するAntoraページ（`docs/modules/<module>/pages/*.adoc`）で生成画像を埋め込むことを基本にする。
- AntoraページではAsciiDocの`image::diagrams/<file>.svg[]`で生成画像を埋め込む。
- `README.md`では必要な代表図だけを扱ってよい。
- 必要なら対応するDSLソースへのリンクを併記してよい。

## CIでの標準生成手順
- 実行位置は各ソースリポジトリのrootとする。
- Structurizr図生成CI workflowは、図を持つ各ソースリポジトリの`.github/workflows/diagrams.yml`に置く。
- workflowは`pull_request`、`main`への`push`、手動の`workflow_dispatch`で起動する。
- 初期標準では、GitHub Actionsの`paths` filterは使わず、workflow内のworkspace自動検出で図生成対象なしを成功扱いにする。
- `pull_request`では、生成SVGの未反映差分をmerge前に検出する。
- `main`への`push`では、サイト再生成依頼前に生成SVGが反映済みであることを確認する。
- `workflow_dispatch`は、失敗後の再確認や環境更新後の手動検証に使う。
- 標準Docker imageは`structurizr/structurizr:2026.05.22-playwright`とする。
- 標準コマンドは次とする。

```sh
docker run --rm -v "$PWD:/usr/local/structurizr" structurizr/structurizr:2026.05.22-playwright export -format svg -workspace docs/modules/<module>/examples/diagrams/workspace.json -output docs/modules/<module>/assets/images/diagrams
```

- 手動レイアウト再現を標準にするため、CIの入力は`workspace.json`に固定する。
- CIではSVGを再生成できることを確認し、生成SVGに差分がある場合は文書変更に含める。
- 生成対象は`find docs/modules -path '*/examples/diagrams/workspace.json' -type f -print`で自動検出する。
- 検出した`workspace.json`ごとに、同じAntora moduleの`assets/images/diagrams/`へSVGを出力する。
- 対象`workspace.json`が0件の場合は、図生成対象なしとして成功扱いにする。
- SVG export後に`git status --porcelain -- docs/modules/<module>/assets/images/diagrams`を実行し、出力があれば未反映差分としてCIを失敗させる。
- 失敗時は、`git status --short -- docs/modules/<module>/assets/images/diagrams`と`git diff -- docs/modules/<module>/assets/images/diagrams`をlogに出す。
- untracked SVGも検出対象にする。

## `!include` の扱い
- `!include`を使う場合は、対応するAntora moduleの`examples/diagrams/`配下のDSLソースツリー内で相対参照が完結するように構成する。
- `!include`では、`pages/`、`partials/`、`assets/images/`、別module、リポジトリ外ファイルを参照しない。
- 生成画像の置き場やarc42本文ファイルを`!include`の対象にしない。
