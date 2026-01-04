# actions

@masutaka が個人的に使用する、[GitHub Actions の Reusable workflows](https://docs.github.com/actions/how-tos/reuse-automations/reuse-workflows)

## Reusable workflows

- [codeql.yml](./.github/workflows/codeql.yml) ([docs](./docs/codeql.md)) - 変更されたファイルから言語を検知し CodeQL 解析を実行する
- [codeql_core.yml](./.github/workflows/codeql_core.yml) ([docs](./docs/codeql_core.md)) - 指定された言語の CodeQL 解析を実行する
- [create_gh_issue.yml](./.github/workflows/create_gh_issue.yml) ([docs](./docs/create_gh_issue.md)) - テンプレートから GitHub Issue を作成する
- [dependency_review.yml](./.github/workflows/dependency_review.yml) ([docs](./docs/dependency_review.md)) - PR の依存関係をレビューする
- [pushover.yml](./.github/workflows/pushover.yml) ([docs](./docs/pushover.md)) - ワークフロー失敗時に Pushover 通知を送信する
