# dependency_review

`dependency_review` reusable workflow [`.github/workflows/dependency_review.yml`](../.github/workflows/dependency_review.yml) は、GitHub Advanced Security の Dependency Review 機能を実行する。

このワークフローは Pull Request で追加・変更された依存関係のセキュリティ脆弱性をチェックし、デフォルトブランチへの脆弱性の混入を防ぐ。

## Inputs

| 名前 | 必須 | デフォルト | 設定可能な値 | 説明 |
| --- | --- | --- | --- | --- |
| `comment-summary-in-pr` | No | `on-failure` | `always`, `on-failure`, `never` | PR にサマリーをコメントとして投稿するかどうかを決定する |

## 使用例

### 脆弱性が見つかったら、サマリーを投稿する（デフォルト）

```yml
name: Dependency Review

on: [pull_request]

jobs:
  dependency_review:
    uses: masutaka/actions/.github/workflows/dependency_review.yml@main
    permissions:
      contents: read
      pull-requests: write
```

### 常にサマリーを投稿する

```yml
name: Dependency Review

on: [pull_request]

jobs:
  dependency_review:
    uses: masutaka/actions/.github/workflows/dependency_review.yml@main
    permissions:
      contents: read
      pull-requests: write
    with:
      comment-summary-in-pr: always
```

### 常にサマリーを投稿しない

```yml
name: Dependency Review

on: [pull_request]

permissions:
  contents: read

jobs:
  dependency_review:
    uses: masutaka/actions/.github/workflows/dependency_review.yml@main
    permissions:
      contents: read
    with:
      comment-summary-in-pr: never
```

## 関連

- [Configuring dependency review - GitHub Docs](https://docs.github.com/code-security/supply-chain-security/understanding-your-software-supply-chain/configuring-dependency-review)
- https://github.com/actions/dependency-review-action
