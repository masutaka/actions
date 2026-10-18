# review_deps_pr

`review_deps_pr` reusable workflow [`.github/workflows/review_deps_pr.yml`](../.github/workflows/review_deps_pr.yml) は、Dependabot / Renovate の PR を Claude でレビューし、結果を PR にコメントする。

レビューには [masutaka/dotfiles-public](https://github.com/masutaka/dotfiles-public) の `my-review-deps-pr` スキルを使う。Claude API の認証には Workload Identity Federation を使うので、API key は不要。

指定された PR の作成者が Dependabot / Renovate でなければ、レビューせずにエラーにする。

## 仕組み

- `review` job で Claude がレビューし、結果を artifact に保存する。Claude に渡す `GITHUB_TOKEN` は read-only
- `comment` job が artifact を PR にコメントする。この job では Claude を動かさない
- Claude に許可するツールは `ghro`（`gh` へのシンボリックリンク）・`gh pr checks`・`Read`・`Write` に限る

## セットアップ

Claude Console の federation rule は、`job_workflow_ref` をこの reusable workflow の main に限定している。そのため、呼び出し側は `@main` で参照すること。

## Inputs

| 名前 | 必須 | デフォルト | 説明 |
| --- | --- | --- | --- |
| `pr-number` | Yes | - | レビューする Dependabot / Renovate の PR 番号 |

## 使用例

### Dependabot を使うリポジトリ

```yml
name: Review deps PR

on:
  pull_request:
    types: [opened]
  workflow_dispatch:
    inputs:
      pr-number:
        description: レビューする Dependabot の PR 番号
        required: true
        type: number

jobs:
  review_deps_pr:
    if: github.event_name == 'workflow_dispatch' || github.event.pull_request.user.login == 'dependabot[bot]'
    uses: masutaka/actions/.github/workflows/review_deps_pr.yml@main
    permissions:
      contents: read
      id-token: write
      pull-requests: write
      security-events: read
    with:
      pr-number: ${{ inputs.pr-number || github.event.pull_request.number }}
```

### Renovate を使うリポジトリ

```yml
name: Review deps PR

on:
  pull_request:
    types: [opened]
  workflow_dispatch:
    inputs:
      pr-number:
        description: レビューする Renovate の PR 番号
        required: true
        type: number

jobs:
  review_deps_pr:
    if: github.event_name == 'workflow_dispatch' || github.event.pull_request.user.login == 'renovate[bot]'
    uses: masutaka/actions/.github/workflows/review_deps_pr.yml@main
    permissions:
      contents: read
      id-token: write
      pull-requests: write
      security-events: read
    with:
      pr-number: ${{ inputs.pr-number || github.event.pull_request.number }}
```

## 関連

- https://github.com/anthropics/claude-code-action
- https://platform.claude.com/docs/ja/manage-claude/wif-providers/github-actions
