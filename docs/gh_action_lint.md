# gh_action_lint

`gh_action_lint` reusable workflow [`.github/workflows/gh_action_lint.yml`](../.github/workflows/gh_action_lint.yml) は、GitHub Actions のワークフローファイルを 2 つの linter で検査する。

- [actionlint](https://github.com/rhysd/actionlint) は、構文や式、`run` のシェルスクリプトの誤りを検出する
- [ghalint](https://github.com/suzuki-shunsuke/ghalint) は、`permissions` や secret の扱いといったセキュリティポリシーへの準拠を見る。検査内容は [Policies](https://github.com/suzuki-shunsuke/ghalint#policies) を参照

それぞれ別のジョブなので、片方が失敗しても、もう片方の結果は得られる。

## Inputs

なし

## 使用例

```yml
name: Gh Action Lint

on:
  push:
    branches:
      - main
  pull_request:

jobs:
  gh_action_lint:
    uses: masutaka/actions/.github/workflows/gh_action_lint.yml@main
    permissions:
      contents: read
```

## 関連

- https://github.com/rhysd/actionlint
- https://github.com/suzuki-shunsuke/ghalint
