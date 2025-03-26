# 5. Role Distribution between CI/CD and Local Execution

## Reason for Role Separation

- Running all validations on CI/CD increases execution time and degrades speed
- Developers can improve efficiency by running checks locally when possible
- Tasks like formatting and linting should integrate into IDEs or plugins

## ✅ Recommended CI/CD (gitlab-ci) Tasks

| Task                          | Tool                   | Purpose                                    |
| ----------------------------- | ---------------------- | ------------------------------------------ |
| Static Analysis (Design/Type) | PHPStan (Larastan)     | Detect design issues and type errors early |
| Static Analysis (Security)    | Psalm / Composer Audit | Automatically detect vulnerabilities       |
| Unit Testing / Coverage       | PHPUnit                | Ensure test coverage and maintain quality  |

## ✅ Local Execution Tasks (Developer)

### 🔹 By Developer (integrated in development flow)

| Task                 | Tool                         | Purpose                                   |
| -------------------- | ---------------------------- | ----------------------------------------- |
| Coding Rule & Format | Laravel Pint / VSCode plugin | Auto-format on save, enforce coding rules |

### 🔹 By Developer and Reviewer

| Task             | Tool                         | Purpose                                               |
| ---------------- | ---------------------------- | ----------------------------------------------------- |
| N+1 Detection    | Laravel Telescope / Debugbar | Hard to detect without execution, check at dev/review |
| AI Review Assist | ChatGPT / Copilot / LLM Tool | Assist specification/design review                    |
| E2E Test         | Laravel Dusk / Cypress       | Run only when needed, including in review             |
