# create_gh_issue

`create_gh_issue` reusable workflow [`.github/workflows/create_gh_issue.yml`](../.github/workflows/create_gh_issue.yml) は、指定された引数で GitHub Issue を作成する。

## Inputs

| 名前 | 必須 | デフォルト | 説明 |
| --- | --- | --- | --- |
| `title` | Yes | - | 作成する Issue のタイトル。`date_format` 指定時は `{{date}}` が日付に置換される |
| `date_format` | No | なし | `title` 内の `{{date}}` を置換する `date` コマンドのフォーマット（例: `+%Y年%m月`） |
| `description_template_path` | Yes | - | Issue の説明に利用するテンプレートファイルのパス |
| `assignees` | No | なし | Issue の Assignees（カンマ区切り） |
| `labels` | No | なし | Issue の Labels（カンマ区切り） |

## 使用例

### 基本的な使用方法

```yml
name: Create GitHub Issue

on:
  schedule:
    # 毎週水曜 13:00 JST に実行する
    - cron: "0 13 * * wed"
      timezone: "Asia/Tokyo"

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

### タイトルに日付を含める

`date_format` を指定すると、`title` 内の `{{date}}` が Asia/Tokyo タイムゾーンの日付に置換される。

```yml
name: Create GitHub Issue

on:
  schedule:
    # 毎月1日 10:00 JST に実行する
    - cron: "0 10 1 * *"
      timezone: "Asia/Tokyo"

jobs:
  create_issue:
    uses: masutaka/actions/.github/workflows/create_gh_issue.yml@main
    permissions:
      contents: read
      issues: write
    with:
      title: "{{date}} の月次タスク"
      date_format: "+%Y年%m月"
      description_template_path: .github/workflows/templates/monthly_task.md
      assignees: alice
```

### Assignees と Labels を指定

```yml
name: Create GitHub Issue

on:
  schedule:
    # 毎週水曜 13:00 JST に実行する
    - cron: "0 13 * * wed"
      timezone: "Asia/Tokyo"

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
