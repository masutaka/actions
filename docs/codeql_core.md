# codeql_core

`codeql_core` reusable workflow [`.github/workflows/codeql_core.yml`](../.github/workflows/codeql_core.yml) は、CodeQL 解析を実行する。

## 背景

[Code Scanning のデフォルト設定](https://docs.github.com/code-security/code-scanning/enabling-code-scanning/configuring-default-setup-for-code-scanning#about-default-setup)では、push/pull_request をトリガーとした実行だけでなくスケジュール実行も行われる。

[codeql.yml](./codeql.md) は push/pull_request で変更されたファイルに基づいて言語を自動選択するが、スケジュール実行には対応していない。スケジュール実行を行う場合は、このワークフローを直接使用する必要がある。

## Inputs

| 名前 | 必須 | 設定可能な値 | 説明 |
| --- | --- | --- | --- |
| `language` | Yes | `actions`, `go`, `javascript`, `python`, `ruby` | 解析対象のプログラミング言語 |

## 使用例

### push/pull_request トリガー

変更されたファイルに応じて解析する場合は、[codeql.yml](./codeql.md) を使用すること。

### スケジュール実行

```yml
name: CodeQL Schedule

on:
  schedule:
    - cron: "00 10 * * 5" # every friday 19:00 JST

jobs:
  codeql:
    strategy:
      matrix:
        language: [actions, ruby]
    permissions:
      actions: read
      checks: read
      contents: read
      security-events: write
    uses: masutaka/actions/.github/workflows/codeql_core.yml@main
    with:
      language: ${{ matrix.language }}
```

## 関連

- [Finding security vulnerabilities and errors in your code with code scanning - GitHub Docs](https://docs.github.com/code-security/code-scanning)
- [CodeQL overview — CodeQL](https://codeql.github.com/docs/codeql-overview/)
- https://github.com/github/codeql-action
