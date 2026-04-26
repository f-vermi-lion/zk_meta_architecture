# docs草稿からアーキテクチャ文書公開基盤の方針へ進めた理由

## 変更
- `docs/architecture-documentation-publishing-platform`の草稿を、アーキテクチャ文書公開基盤の決定へ反映した。
- 文書配置規則をAntora標準構成へ更新した。
- GitHub Pages公開方針に、GitHub Actions、Antora build、複数リポジトリ集約の流れを加えた。

## 理由
- 草稿は、AsciiDoc、Antora、docToolchain、Structurizr、GitHub Actions、GitHub Pagesを組み合わせる前提を具体化していた。
- 既存のAsciiDoc + GitHub Pages方針、日英運用方針、図をDSL正本にする方針と整合していた。
- Antora標準構成へ寄せることで、複数リポジトリ集約、ページ間参照、partial includeを既存ツールの流儀に乗せやすくなる。
