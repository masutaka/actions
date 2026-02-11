# codeql

`codeql` reusable workflow [`.github/workflows/codeql.yml`](../.github/workflows/codeql.yml) は、変更されたファイルの拡張子をチェックし、該当する言語の CodeQL 解析を実行する。

## 背景

Code Scanning のデフォルト設定を使用した場合、以下の課題がある。

- ドキュメントのみの修正にも関わらず、すべてのプログラミング言語用のジョブが動く
- 解析しなくてよいディレクトリ配下の指摘が多数見つかる

このワークフローは上記の課題を解決するために作成された。

## 対応言語

- GitHub Actions
- Go
- JavaScript
- Python
- Ruby
- TypeScript

> [!NOTE]
> [CodeQL が対応する言語](https://codeql.github.com/docs/codeql-overview/supported-languages-and-frameworks/#languages-and-compilers)のうち、使用するものだけ対応している。

## セットアップ

プライベートリポジトリでは、リポジトリの Settings > Code security から、GitHub Advanced Security を有効化する。

`https://github.com/{owner}/{repo}/settings/security_analysis`

## Inputs

なし

## 使用例

### 基本的な使用方法

```yml
name: CodeQL

on:
  push:

jobs:
  codeql:
    uses: masutaka/actions/.github/workflows/codeql.yml@main
    permissions:
      actions: read
      checks: read
      contents: read
      security-events: write
```

### マージキューを使用するリポジトリ

マージキューを使用しているリポジトリでは、ジョブの実行ブランチが以下の命名規則に沿ったブランチになることがある。

```txt
gh-readonly-queue/{base_branch}/pr-{pr_num}-{base_branch_hash}
```

このブランチはリモートには存在しないため、codeql.yml が使用する `dorny/paths-filter` が失敗する場合がある。その際は以下のようにブランチの除外条件を追加すればよい。

```yml
name: CodeQL

on:
  push:
    branches-ignore:
      - gh-readonly-queue/main/pr-*

jobs:
  codeql:
    uses: masutaka/actions/.github/workflows/codeql.yml@main
    permissions:
      actions: read
      checks: read
      contents: read
      security-events: write
```

## 関連

- [Finding security vulnerabilities and errors in your code with code scanning - GitHub Docs](https://docs.github.com/code-security/code-scanning)
- [CodeQL overview — CodeQL](https://codeql.github.com/docs/codeql-overview/)
- https://github.com/github/codeql-action
