# 図ソース配置をtool-neutralに分離した理由

## 決定
- 図ソース全般は、対応するAntora moduleの`examples/diagrams/`配下に置く。
- 公開画像は、図の作成方式に関係なく`assets/images/diagrams/`配下に置く。
- C4/StructurizrのDSL/JSONは、`examples/diagrams/structurizr/`配下に置く。
- C4/Structurizr以外の図ソースは、実際に別形式の図を入れる時点で、その形式ごとに配置・生成・CI検査方針を決める。

## 理由
- `assets/images/diagrams/`はAntoraのimage familyとして公開画像を置く場所であり、図作成ツールに依存しない。
- `examples/diagrams/`直下にStructurizr固有の`workspace.dsl`と`workspace.json`を置くと、他形式の図ソースを追加するときに構造がStructurizr前提に見える。
- Structurizr固有ファイルを`structurizr/`配下へ分離すると、将来ほかの形式の図ソースを追加しても、公開画像の参照形式を変えずに拡張できる。
- 具体的な別形式が必要になる前にDraw.ioやMermaidなどの個別方針まで決めると、不要な論点が増える。

## 今後の扱い
- C4/Structurizr以外の図形式を実際に追加するときは、その形式に絞って配置、生成、CI検査方針を決める。

## 参考
- `decisions/architecture_diagrams.md`
