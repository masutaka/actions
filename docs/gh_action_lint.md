# gh_action_lint

`gh_action_lint` reusable workflow [`.github/workflows/gh_action_lint.yml`](../.github/workflows/gh_action_lint.yml) は、GitHub Actions まわりの設定ファイルを 3 つの linter で検査する。

- [actionlint](https://github.com/rhysd/actionlint) は、構文や式、`run` のシェルスクリプトの誤りを検出する
- [ghalint](https://github.com/suzuki-shunsuke/ghalint) は、`permissions` や secret の扱いといったセキュリティポリシーへの準拠を見る。検査内容は [Policies](https://github.com/suzuki-shunsuke/ghalint#policies) を参照
    - `ghalint run` は `.github/workflows/*.{yml,yaml}` を検査する
    - `ghalint run-action` は `action.{yml,yaml}` を検査する。リポジトリルート直下だけでなく、`.github/actions/foo/action.yml` のようにサブディレクトリへ切り出した composite action も対象になる
- [zizmor](https://github.com/zizmorcore/zizmor) は CI/CD 設定の静的解析ツールで、テンプレート展開によるコード注入や pin の不備といった、より踏み込んだセキュリティ上の問題を検出する。検査内容は [Audit rules](https://docs.zizmor.sh/audits/) を参照
    - ghalint のポリシーをほぼ包含したうえで、`template-injection` や `ref-version-mismatch`、`known-vulnerable-actions` といった zizmor 固有の検査を持つ
    - ワークフローと action 定義に加えて、`.github/dependabot.yml` や `.pre-commit-config.yaml` も検査対象になる。たとえば `dependabot-cooldown` は Dependabot の `cooldown` 未設定を検出する
    - GitHub API を使うオンライン検査を含むため `GITHUB_TOKEN` を渡す。特別な権限は不要で、主に rate limit 回避の認証に使われる

それぞれ別のジョブなので、1 つが失敗しても、残りの結果は得られる。

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
