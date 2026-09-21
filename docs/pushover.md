# pushover

`pushover` reusable workflow [`.github/workflows/pushover.yml`](../.github/workflows/pushover.yml) は、[Pushover API](https://pushover.net/api) を使用して Pushover 通知を送信する。

デフォルトブランチの CI が失敗した時に、すぐに気づくことを目的としたワークフロー。Inputs を指定すれば、デプロイ完了などの成功通知にも使える。

## Inputs

すべて省略でき、その場合は失敗通知になる。

| 名前 | 必須 | デフォルト | 設定可能な値 | 説明 |
| --- | --- | --- | --- | --- |
| `status` | No | `failure` | `success`, `failure` | ジョブの結果。`success` を渡すと成功通知になる |
| `title` | No | `Failed <repository>'s workflow (<workflow>)` | 任意の文字列 | 通知のタイトル |
| `priority` | No | `1`（High Priority） | [API Docs > Message Priority](https://pushover.net/api#priority) 参照 | 通知の優先度 |
| `sound` | No | `falling` | [API Docs > Notification Sounds](https://pushover.net/api#sounds) 参照 | 通知音 |

## Secrets

| 名前 | 必須 | 説明 |
| --- | --- | --- |
| `PUSHOVER_API_KEY` | Yes | Pushover の API Token |
| `PUSHOVER_USER_KEY` | Yes | Pushover の User Key または Group Key |

## 使用例

### 失敗時のみ通知する

```yml
name: CI

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v7
      - run: npm run build

  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v7
      - run: npm test

  pushover:
    name: pushover if failure
    if: github.ref_name == github.event.repository.default_branch && failure()
    needs: [build, test]
    uses: masutaka/actions/.github/workflows/pushover.yml@main
    permissions: {}
    secrets:
      PUSHOVER_API_KEY: ${{ secrets.PUSHOVER_API_KEY }}
      PUSHOVER_USER_KEY: ${{ secrets.PUSHOVER_USER_KEY }}
```

### 成功時にも通知する

```yml
  pushover_success:
    name: pushover if success
    if: success()
    needs: [build, test]
    uses: masutaka/actions/.github/workflows/pushover.yml@main
    permissions: {}
    with:
      status: success
      title: "Deployed ${{ github.repository }}"
      priority: 0 # Normal
      sound: pushover
    secrets:
      PUSHOVER_API_KEY: ${{ secrets.PUSHOVER_API_KEY }}
      PUSHOVER_USER_KEY: ${{ secrets.PUSHOVER_USER_KEY }}
```

## 通知内容

タイトルは `title` input（省略時は `Failed <repository>'s workflow (<workflow>)`）、URL は通知を送ったワークフロー実行へのリンクになる。本文は以下の 4 行。

```
Repo: masutaka/actions
Commit: 8e2b830a
Ref: refs/heads/main
Status: failure
```

## 関連

- https://pushover.net/
