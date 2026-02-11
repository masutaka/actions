# pushover

`pushover` reusable workflow [`.github/workflows/pushover.yml`](../.github/workflows/pushover.yml) は、[Pushover API](https://pushover.net/api) を使用してワークフロー失敗用の Pushover 通知を送信する。

デフォルトブランチの CI が失敗した時に、すぐに気づくことを目的としたワークフロー。

## Inputs

なし

## Secrets

| 名前 | 必須 | 説明 |
| --- | --- | --- |
| `PUSHOVER_API_KEY` | Yes | Pushover の API Token |
| `PUSHOVER_USER_KEY` | Yes | Pushover の User Key または Group Key |

## 使用例

```yml
name: CI

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v6
      - run: npm run build

  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v6
      - run: npm test

  pushover:
    name: pushover if failure
    if: github.ref_name == github.event.repository.default_branch && failure()
    needs: [build, test]
    uses: masutaka/actions/.github/workflows/pushover.yml@main
    permissions:
      contents: read
    secrets:
      PUSHOVER_API_KEY: ${{ secrets.PUSHOVER_API_KEY }}
      PUSHOVER_USER_KEY: ${{ secrets.PUSHOVER_USER_KEY }}
```

## 関連

- https://pushover.net/
- https://github.com/umahmood/pushover-actions
