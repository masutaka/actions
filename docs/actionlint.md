# actionlint

`actionlint` reusable workflow [`.github/workflows/actionlint.yml`](../.github/workflows/actionlint.yml) は、GitHub Actions のワークフローファイルを [actionlint](https://github.com/rhysd/actionlint) で静的解析する。

セキュリティポリシーへの準拠を検査する [ghalint](./ghalint.md) とは異なり、構文や式、`run` のシェルスクリプトの誤りを検出する。

## Inputs

なし

## 使用例

```yml
name: Actionlint

on:
  push:
    branches:
      - main
  pull_request:

jobs:
  actionlint:
    uses: masutaka/actions/.github/workflows/actionlint.yml@main
    permissions:
      contents: read
```

## 関連

- https://github.com/rhysd/actionlint
