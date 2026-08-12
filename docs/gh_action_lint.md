# gh_action_lint

`gh_action_lint` reusable workflow [`.github/workflows/gh_action_lint.yml`](../.github/workflows/gh_action_lint.yml) は、GitHub Actions まわりの設定ファイルを 3 つの linter で検査する。

linter ごとにジョブを分けてあるので、1 つが失敗しても残りの結果は得られる。

## 検査する linter

| linter | 何を見るか | 検査対象 |
| --- | --- | --- |
| [actionlint](https://github.com/rhysd/actionlint) | 構文や式、`run` のシェルスクリプトの誤り | ワークフロー |
| [ghalint](https://github.com/suzuki-shunsuke/ghalint) | `permissions` や secret の扱いといったセキュリティポリシーへの準拠 | ワークフロー、action 定義 |
| [zizmor](https://github.com/zizmorcore/zizmor) | テンプレート展開によるコード注入や pin の不備 | ワークフロー、action 定義、Dependabot 設定、pre-commit 設定 |

検査対象のファイルはそれぞれ、ワークフローが `.github/workflows/*.{yml,yaml}`、action 定義が `action.{yml,yaml}`、Dependabot 設定が `.github/dependabot.yml`、pre-commit 設定が `.pre-commit-config.yaml` を指す。

検査項目の一覧は ghalint の [Policies](https://github.com/suzuki-shunsuke/ghalint#policies) と zizmor の [Audit rules](https://docs.zizmor.sh/audits/) を参照。

ghalint だけはコマンドが分かれていて、`ghalint run` がワークフローを、`ghalint run-action` が action 定義を検査する。後者はリポジトリルート直下だけでなく、`.github/actions/foo/action.yml` のようにサブディレクトリへ切り出した composite action も対象になる。

### ghalint と zizmor を併用している理由

zizmor は ghalint のポリシーをほぼ包含する。それでも両方を動かしているのは、`job_timeout_minutes_is_required` と `action_shell_is_required` が ghalint にしかないため。SHA pin のように、両方から同じ指摘が出るものもある。

### zizmor に GITHUB_TOKEN を渡している理由

`known-vulnerable-actions` のように GitHub API を叩くオンライン検査があるため。特別な権限は不要で、主に rate limit 回避の認証に使われる。

## Inputs

なし

## 使用例

```yml
name: Gh Action Lint

on:
  push:
    branches:
      - main
  pull_request:

jobs:
  gh_action_lint:
    uses: masutaka/actions/.github/workflows/gh_action_lint.yml@main
    permissions:
      contents: read
```

## 関連

- https://github.com/rhysd/actionlint
- https://github.com/suzuki-shunsuke/ghalint
- https://github.com/zizmorcore/zizmor
- https://github.com/zizmorcore/zizmor-action
