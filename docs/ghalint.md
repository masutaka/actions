# ghalint

`ghalint` reusable workflow [`.github/workflows/ghalint.yml`](../.github/workflows/ghalint.yml) は、GitHub Actions のワークフローファイルを [ghalint](https://github.com/suzuki-shunsuke/ghalint) で検査する。

構文を検査する [actionlint](./actionlint.md) とは異なり、`permissions` や secret の扱いといったセキュリティポリシーへの準拠を見る。検査内容は [Policies](https://github.com/suzuki-shunsuke/ghalint#policies) を参照。

## Inputs

なし

## 使用例

```yml
name: Ghalint

on:
  push:
    branches:
      - main
  pull_request:

jobs:
  ghalint:
    uses: masutaka/actions/.github/workflows/ghalint.yml@main
    permissions:
      contents: read
```

## バージョンの更新

ghalint のリリースには `v1.5.7-0` のようなプレリリース版が含まれる。prerelease でないタグを選ぶこと。

## 関連

- https://github.com/suzuki-shunsuke/ghalint
