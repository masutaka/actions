# create_gh_issue

`create_gh_issue` reusable workflow [`.github/workflows/create_gh_issue.yml`](../.github/workflows/create_gh_issue.yml) は、指定された引数で GitHub Issue を作成する。

## Inputs

| 名前 | 必須 | デフォルト | 説明 |
| --- | --- | --- | --- |
| `title` | Yes | - | 作成する Issue のタイトル |
| `description_template_path` | Yes | - | Issue の説明に利用するテンプレートファイルのパス |
| `assignees` | No | なし | Issue の Assignees（カンマ区切り） |
| `labels` | No | なし | Issue の Labels（カンマ区切り） |

## 使用例

### 基本的な使用法

```yml
name: Create GitHub Issue

on:
  schedule:
    # 毎週水曜 13:00 JST に実行する
    - cron: "0 4 * * wed"

jobs:
  create_issue:
    uses: masutaka/actions/.github/workflows/create_gh_issue.yml@main
    permissions:
      contents: read
      issues: write
    with:
      title: Sample Issue
      description_template_path: .github/workflows/templates/sample_issue.md
```

### Assignees と Labels を指定

```yml
name: Create GitHub Issue

on:
  schedule:
    # 毎週水曜 13:00 JST に実行する
    - cron: "0 4 * * wed"

jobs:
  create_issue:
    uses: masutaka/actions/.github/workflows/create_gh_issue.yml@main
    permissions:
      contents: read
      issues: write
    with:
      title: Sample Issue
      description_template_path: .github/workflows/templates/sample_issue.md
      assignees: alice,bob
      labels: bug,wontfix
```
