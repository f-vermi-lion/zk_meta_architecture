# Structurizr図生成CI workflowの配置と起動条件を決めた理由

## 決定
- Structurizr図生成CI workflowは、図を持つ各ソースリポジトリの`.github/workflows/diagrams.yml`に置く。
- workflowは`pull_request`、`main`への`push`、手動の`workflow_dispatch`で起動する。
- 初期標準では、GitHub Actionsの`paths` filterは使わず、workflow内のworkspace自動検出で図生成対象なしを成功扱いにする。
- サイト用リポジトリ側では、標準ではStructurizr SVGを再生成しない。

## 理由
- 生成SVGはソースリポジトリで管理するため、再生成差分の検出もソースリポジトリ側で行うのが自然である。
- `pull_request`で検出すると、生成SVGの未反映差分をmerge前に止められる。
- `main`への`push`で検出すると、サイト再生成依頼前にmain上の図資産が整っていることを確認できる。
- `workflow_dispatch`を残すと、失敗後の再確認や実行環境更新後の手動検証がしやすい。
- `paths` filterを初期標準にしないことで、required check運用時のskip扱いを避け、workspace自動検出によるno-op成功に寄せられる。
- サイト用リポジトリで再生成すると、content sourceから取得した他リポジトリの成果物を更新できず、正本と派生成果物の責務が曖昧になる。

## 残した論点
- サイト再生成依頼と図生成CIの依存関係は、サイト再生成用のGitHub Actions権限とトークン管理方針と合わせて決める。

## 後続実装
- Structurizr図生成CI workflowの標準YAMLテンプレートは、`docs/modules/ROOT/examples/github-actions/diagrams.yml`に作成した。

## 参考
- `decisions/architecture_diagrams.md`
- `decisions/architecture_document_publication.md`
