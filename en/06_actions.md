# 6. Concrete Actions (Execution Procedures and Examples)

## ✅ CI/CD Actions Example (gitlab-ci)

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

## ✅ Local Developer Actions

```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "shufo.vscode-pint"
}
```

Run: `php artisan serve` → Check `/telescope/requests`

AI review flow example:

```bash
git diff > diff.patch
```

Send to ChatGPT with template:

```text
Please review the following diff:
- Potential bugs
- N+1 or performance issues
- SRP violations
- Missing test perspectives

--- diff start ---
(diff content here)
--- diff end ---
```

E2E Example:

```bash
php artisan dusk
npx cypress open
```
