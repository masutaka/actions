# actionlint

`actionlint` reusable workflow [`.github/workflows/actionlint.yml`](../.github/workflows/actionlint.yml) は、GitHub Actions のワークフローファイルを [actionlint](https://github.com/rhysd/actionlint) で静的解析する。

actionlint のバイナリ実行ファイルを GitHub Releases から取得し、SHA-256 チェックサムで検証した上で実行する。バージョンとチェックサムはワークフロー最上位の `env` で指定されており、更新時は `.github/workflows/actionlint.yml` の当該値を書き換える。

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
