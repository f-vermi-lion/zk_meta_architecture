# Structurizr Docker image tagの更新方針を決めた理由

## 決定
- CI上の標準Docker imageは、引き続き`structurizr/structurizr:2026.05.22-playwright`とする。
- Docker image tagは、Playwright依存を含む日付付き`-playwright` tagを明示固定する。
- `latest`や未修飾tagは使わない。
- Docker image tagの更新は、月次確認、CI失敗、脆弱性対応、GitHub Actions runner互換性対応、Structurizr export修正の取り込みが必要な場合に、専用変更として手動で行う。
- Docker image tagを更新するときは、標準Docker imageのtag文字列、図生成CI設定、生成SVG、関連文書を同じ変更単位で更新する。
- 更新候補で生成に失敗する場合、または生成SVG差分が大きすぎて判断できない場合は、現行tagを維持し、具体的な未解決論点を`inbox.md`へ戻す。

## 理由
- Structurizrの直接SVG生成は、Playwrightとブラウザベースレンダラに依存するため、CI実行環境の再現性が重要である。
- 固定tagにすると、上流Docker imageの更新によってCI結果や生成SVGが予告なく変わることを避けやすい。
- 日付付き`-playwright` tagは、Playwright依存を含む実行環境であることと、採用時点を追跡しやすいことの両方を満たす。
- tag更新を専用変更にすると、実行環境変更による生成SVG差分と、文書・workflowの変更をまとめてレビューしやすい。
- 自動PRは、生成SVG差分やレイアウト崩れの判断に人の確認が必要な段階では、運用ノイズを増やす可能性があるため初期標準にしない。
- `latest`や未修飾tagは、再現性を落とし、失敗原因が文書変更か実行環境変更かを切り分けにくくするため採用しない。

## 残した論点
- Docker image更新の頻度や負荷が増えた場合、Renovateなどによる自動PR化は別途検討する。

## 参考
- `decisions/architecture_diagrams.md`
- `decision_records/structurizr_direct_svg_generation.md`
