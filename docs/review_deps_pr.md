# review_deps_pr

`review_deps_pr` reusable workflow [`.github/workflows/review_deps_pr.yml`](../.github/workflows/review_deps_pr.yml) は、Dependabot / Renovate の PR を Claude でレビューし、結果を PR にコメントする。

レビューには [masutaka/dotfiles-public](https://github.com/masutaka/dotfiles-public) の `my-review-deps-pr` スキルを使う。Claude API の認証には Workload Identity Federation を使うので、API key は不要。

指定された PR の作成者が Dependabot / Renovate でなければ、レビューせずにエラーにする。

## 仕組み

- `review` job で Claude がレビューし、結果を artifact に保存する
- `GITHUB_TOKEN` では Dependabot alert を参照できないので、Claude には GitHub App のトークンを渡す。権限はそのリポジトリの読み取りだけ
- `comment` job が artifact を PR にコメントする。この job では Claude を動かさない
- Claude に許可するツールは `ghro`（`gh` へのシンボリックリンク）・`gh pr checks`・`Read`・`Write` に限る

## セットアップ

Claude Console の federation rule は、`job_workflow_ref` をこの reusable workflow の main に限定している。そのため、呼び出し側は `@main` で参照すること。

GitHub App `masutaka-review-deps-pr`（Repository permissions: Checks・Commit statuses・Contents・Dependabot alerts・Pull requests の Read-only）を、呼び出し側のリポジトリにインストールする。

`https://github.com/settings/installations`

App の private key を、呼び出し側のリポジトリの secret `REVIEW_DEPS_PR_APP_PRIVATE_KEY` に登録する。Dependabot がトリガーした実行は Dependabot secrets しか参照できないので、Dependabot を使うリポジトリは Actions secrets と Dependabot secrets の両方に登録する。

```sh
gh secret set REVIEW_DEPS_PR_APP_PRIVATE_KEY --repo masutaka/<repo> < private-key.pem
gh secret set REVIEW_DEPS_PR_APP_PRIVATE_KEY --repo masutaka/<repo> --app dependabot < private-key.pem
```

## Inputs

| 名前 | 必須 | デフォルト | 説明 |
| --- | --- | --- | --- |
| `pr-number` | Yes | - | レビューする Dependabot / Renovate の PR 番号 |
| `language` | No | `ja` | レビューを書く言語。`ja` か `en` |

## Secrets

| 名前 | 必須 | 説明 |
| --- | --- | --- |
| `REVIEW_DEPS_PR_APP_PRIVATE_KEY` | Yes | GitHub App `masutaka-review-deps-pr` の private key |

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
    with:
      pr-number: ${{ inputs.pr-number || github.event.pull_request.number }}
    secrets:
      REVIEW_DEPS_PR_APP_PRIVATE_KEY: ${{ secrets.REVIEW_DEPS_PR_APP_PRIVATE_KEY }}
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
    with:
      pr-number: ${{ inputs.pr-number || github.event.pull_request.number }}
    secrets:
      REVIEW_DEPS_PR_APP_PRIVATE_KEY: ${{ secrets.REVIEW_DEPS_PR_APP_PRIVATE_KEY }}
```

## 関連

- https://github.com/anthropics/claude-code-action
- https://platform.claude.com/docs/ja/manage-claude/wif-providers/github-actions
