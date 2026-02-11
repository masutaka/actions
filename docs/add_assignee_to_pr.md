# add_assignee_to_pr

`add_assignee_to_pr` reusable workflow [`.github/workflows/add_assignee_to_pr.yml`](../.github/workflows/add_assignee_to_pr.yml) は、PR 作成時に assignee を PR 作成者に設定する。

以下のケースでは assignee を設定しない：
- PR 作成者のユーザータイプが Bot である
- すでに PR 作成時に assignee が設定されている

## Inputs

なし

## 使用例

```yml
name: Setup PR

on:
  pull_request:
    types: [opened]

jobs:
  add-assignee-to-pr:
    permissions:
      pull-requests: write
    uses: masutaka/actions/.github/workflows/add_assignee_to_pr.yml@main
```

## 関連

- https://cli.github.com/manual/gh_pr_edit
