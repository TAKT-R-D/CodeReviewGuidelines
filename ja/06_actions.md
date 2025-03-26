# 6. 具体的な Action（実行手順とタイミング＋具体例）

5 章で分類したそれぞれの役割ごとに、実際の運用手順・タイミングと具体的な設定・手順を以下に示す。

---

## ✅ CI/CD（gitlab-ci）で実行すべきものの具体的アクション

| 項目                       | アクション                                                       | 実行タイミング                 |
| -------------------------- | ---------------------------------------------------------------- | ------------------------------ |
| 静的解析（型・設計）       | `php artisan code:analyse` または `./vendor/bin/phpstan analyse` | Merge Request 作成時、自動実行 |
| 静的解析（セキュリティ）   | `./vendor/bin/psalm` 実行 + `composer audit`                     | Merge Request 作成時、自動実行 |
| ユニットテスト・カバレッジ | `./vendor/bin/phpunit --coverage-text`                           | Merge Request 作成時、自動実行 |

### `.gitlab-ci.yml` 設定例

```yaml
static_analysis:
  stage: test
  script:
    - composer install
    - ./vendor/bin/phpstan analyse --memory-limit=512M

security_check:
  stage: test
  script:
    - composer install
    - ./vendor/bin/psalm
    - composer audit

unit_test:
  stage: test
  script:
    - composer install
    - ./vendor/bin/phpunit --coverage-text
```

---

## ✅ ローカル実行（開発者が実行するもの）の具体的アクション

| 項目                           | アクション                                                     | 実行タイミング         |
| ------------------------------ | -------------------------------------------------------------- | ---------------------- |
| コーディング規約・フォーマット | VSCode 保存時に Laravel Pint / Prettier などで自動フォーマット | 各ファイル保存時に常時 |

### VSCode 設定例

```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "shufo.vscode-pint"
}
```

---

## ✅ ローカル実行（開発者・レビュー担当者が実行・確認するもの）の具体的アクション

| 項目            | アクション                                                                            | 実行タイミング                                    |
| --------------- | ------------------------------------------------------------------------------------- | ------------------------------------------------- |
| N+1 検知        | ローカルで `php artisan serve` → Telescope / Debugbar でクエリ内容確認                | 開発中およびコードレビュー時の任意タイミング      |
| AI 補助レビュー | PR 作成前に AI（ChatGPT / Copilot）へ `diff` または `コード全文` を投げてレビュー補助 | PR 作成前、レビュー前のタイミング                 |
| E2E テスト      | `php artisan dusk` 実行 or Cypress で E2E シナリオテスト実施                          | 開発完了後、PR 前・レビュー前など必要なタイミング |

### 実行手順・確認方法

#### N+1 検知手順

```bash
php artisan serve
```

- ブラウザで対象画面を操作
- `/telescope/requests` を確認し、クエリ・N+1 発生状況をチェック
- 必要に応じて `with()` や `load()` を追加し改善

#### AI 補助レビュー手順

```bash
git diff > diff.patch
```

- ChatGPT や Copilot に以下のテンプレートでレビュー依頼

**レビュー依頼テンプレート（ChatGPT 用）**

```
以下のコード差分（diff）をレビューしてください。

【確認してほしいポイント】
- バグの可能性がある箇所
- N+1やパフォーマンス問題
- 責務の分離が適切か（SRP違反がないか）
- テスト観点の漏れがないか

--- diff start ---
（ここにdiff内容を貼り付け）
--- diff end ---
```

- AI の指摘・提案を確認し、必要に応じて修正

#### E2E テスト手順

##### Laravel Dusk 実行手順

```bash
php artisan dusk
```

- テスト結果（ログ・スクショ）を確認

##### Cypress 実行手順

```bash
npx cypress open
```

- Cypress GUI が立ち上がるので、該当テストシナリオを実行
- 実行結果・スクリーンショット・動画を確認可能

---

この運用により、機械に任せるチェックは CI/CD 側で徹底し、人の判断が必要な項目はローカルやレビュー時に重点的に確認する運用を実現する。
