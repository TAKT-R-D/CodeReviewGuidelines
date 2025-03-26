# Comprehensive Code Review Operation Guidelines — A Practical Guide Covering Quality, Productivity, and AI Utilization

(2025/03/26 @kurab)

## Summary

This guideline systematically outlines the purpose, perspectives, role distribution, operation flow, and AI utilization (Model Context Protocol - MCP) of code review in software development. In addition to traditional code review for quality assurance, it promotes the effective use of AI and static analysis tools to balance development speed and quality. The guide also addresses handling legacy code, role separation between CI/CD and developers, and the future review model using MCP.

## Table of Contents

1. Purpose and Effect of Code Review
2. Code Review Perspectives
3. Role Distribution in Code Review
4. Considerations and Practical Operations for Legacy Code
5. Role Distribution between CI/CD and Local Execution
6. Concrete Actions (Execution Procedures and Examples)
7. Reference: AI Code Review Efficiency with Model Context Protocol (MCP)

---

# 1. Purpose and Effect of Code Review

## 1.1 Purpose of Code Review

Code review is the process of having a third party inspect source code to improve quality. The main objectives are:

- **Early detection of bugs**
- **Enhancement of code readability and maintainability**
- **Validation of design correctness**
- **Improvement of team-wide technical skills (knowledge sharing)**
- **Reduction of security risks**

## 1.2 Effects of Code Review

- **Quality Improvement**: Early detection and correction of bugs and design flaws
- **Prevention of Individual Dependency**: Enables multiple team members to understand the code
- **Ensuring Consistency**: Standardization of coding styles and design philosophies across the project
- **Learning Effect**: Provides learning opportunities for junior engineers

## 1.3 Contribution to KAIZEN (Continuous Improvement) in Agile Development

- **Strengthened Feedback Loops**: Sharing good and bad aspects of code in short cycles to improve subsequent development
- **Knowledge Accumulation within the Team**: Sharing and standardizing good design and implementation patterns
- **Continuous Reduction of Technical Debt**: Cultivating the habit of pointing out and fixing improvements as they are found
- **Fostering Refactoring Culture**: Embedding a mindset of continuous improvement into the development cycle

---

# 2. Code Review Perspectives

## 2.1 Common Review Perspectives

The following are fundamental review perspectives common across projects.

### Functional and Specification Accuracy

- Implementation meets requirements and specifications
- Consideration of boundary values and exceptional cases

### Readability and Maintainability

- Clear and meaningful variable and function names
- Adequate comments where necessary
- Avoidance of deep nesting and proper function decomposition

### Performance

- Avoidance of unnecessary loops or heavy processing
- Elimination of redundant database or external API calls

### Security

- Proper input validation and sanitization
- Prevention of SQL injection, XSS, CSRF, etc.

### Test Coverage

- Required tests are implemented
- Sufficient coverage including both normal and abnormal cases

### Design and Architecture

- Responsibility separation (Single Responsibility Principle)
- Consideration of reusability and scalability

### Coding Conventions and Style

- Adherence to team or project coding conventions
- Application of Linters and Formatters

### Dependency Management

- Utilization of minimal necessary libraries
- Appropriate versions of dependency libraries

### Future Change and Impact Range

- Design that tolerates future specification changes or additions
- Clear understanding of the impact on existing code

### PR Description and Documentation

- Clear PR description including specifications, background, and impact
- Inclusion of necessary migration information or configuration changes

## 2.2 Laravel-Specific Review Perspectives

Additional review points specific to Laravel, considering common pitfalls.

- N+1 problem in Eloquent ORM (`with` statement usage)
- Use of FormRequest class for validation
- Avoiding raw SQL prone to SQL injection
- Avoid excessive use of Facades that hinder testing
- Sound design of migrations and seeding
- Correct routing definitions and prevention of unintended exposure
- No missing middleware (Authorization, Authentication, CSRF)

## 2.3 Angular-Specific Review Perspectives

Review perspectives specific to Angular projects.

- Ensuring type safety (TypeScript)
- Appropriate use of two-way binding (`[(ngModel)]`)
- Avoiding unsubscription or memory leaks in RxJS (Observables)
- Preventing bloated components by separating smart/dumb roles
- Proper module design (including Lazy Loading)
- Correct implementation of form validation
- Adequate consideration for accessibility (a11y)

---

# 3. Role Distribution in Code Review

## 3.1 Items Checkable by Static Analysis or Tools (Recommended for Automation)

- Coding rules and formatting (Pint / phpcs)
- Lint checks (PHPStan / Larastan / ESLint)
- Type checks (PHPStan / TypeScript)
- Dead code detection
- Basic security checks (SQL injection, XSS) (Psalm / Snyk / SonarQube)
- N+1 detection (Telescope / Debugbar)
- Test coverage measurement (PHPUnit / Jest)
- Vulnerable dependency detection (`composer audit` / Snyk)

## 3.2 Areas AI and Assistant Tools Can Support (Future Expansion)

- Design flaw suggestions based on diff (AI review bots)
- PR description generation support (LLM utilization)
- Automatic impact analysis and visualization support (AI + Call Graph)
- Recommendations based on past reviews and knowledge

## 3.3 Items Requiring Human Check (Critical Areas)

- Correctness of specifications and design
- Validity of business logic
- Validity of design intent and separation of responsibilities
- Tolerance for future specification changes and expansion
- Deep dive into impact range (especially legacy interference)
- Confirmation of PR description, background, and intent
- UX / UI design (for frontend)

## 3.4 Responsibility Matrix by Review Item

| Category                      | Review Item                                       | Tool Support               | Human Review | AI Assistance        |
| ----------------------------- | ------------------------------------------------- | -------------------------- | ------------ | -------------------- |
| Functional / Specification    | Specification compliance and requirement coverage |                            | ✅           | ⚠️                   |
| Functional / Specification    | Boundary and exception case handling              |                            | ✅           | ⚠️                   |
| Readability / Maintainability | Naming convention check                           | ✅ (PHPStan / Lint)        | ⚠️           | ⚠️                   |
| Readability / Maintainability | Adequate comments                                 |                            | ✅           | ⚠️                   |
| Readability / Maintainability | Proper nesting and function decomposition         |                            | ✅           | ⚠️                   |
| Readability / Maintainability | DRY / Duplication check                           | ✅ (PHPStan)               | ⚠️           | ⚠️                   |
| Performance                   | Loop redundancy / heavy processing                | ✅ (PHPStan)               | ⚠️           | ⚠️                   |
| Performance                   | Database access optimization                      | ✅ (Larastan)              | ⚠️           | ⚠️                   |
| Performance (Laravel)         | N+1 query detection                               | ✅ (Telescope / Debugbar)  | ⚠️           | ⚠️                   |
| Security                      | Input validation correctness                      | ✅ (Psalm / PHPStan)       | ⚠️           | ⚠️                   |
| Security                      | SQL Injection prevention                          | ✅ (Psalm / Snyk)          | ⚠️           | ⚠️                   |
| Security                      | XSS / CSRF mitigation                             | ✅ (SonarQube)             | ⚠️           | ⚠️                   |
| Test                          | Test coverage measurement                         | ✅ (PHPUnit Coverage)      |              |                      |
| Test                          | Test case quality / coverage                      |                            | ✅           | ⚠️                   |
| Design / Architecture         | Responsibility separation (SRP)                   |                            | ✅           | ⚠️ (Deptrac)         |
| Design / Architecture         | Reusability / Extensibility                       |                            | ✅           | ⚠️                   |
| Coding Convention / Style     | Formatting / Style compliance                     | ✅ (Pint / phpcs)          |              |                      |
| Coding Convention / Style     | Type checking                                     | ✅ (PHPStan / TypeScript)  |              |                      |
| Coding Convention / Style     | Dead code detection                               | ✅ (PHPStan / Psalm)       |              |                      |
| Dependency Management         | Vulnerable library detection                      | ✅ (composer audit / Snyk) | ⚠️           |                      |
| Change Impact / Extensibility | Impact analysis and consideration                 |                            | ✅           | ⚠️ (Call Graph)      |
| PR / Documentation            | PR description and background clarity             |                            | ✅           | ⚠️ (Auto generation) |
| UI/UX (Angular)               | UI/UX design quality                              |                            | ✅           | ⚠️ (ESLint / a11y)   |
| UI/UX (Angular)               | RxJS subscription / memory leak check             | ✅ (ESLint)                | ⚠️           | ⚠️                   |
| Architecture (Angular)        | Angular Module design (Lazy Loading)              |                            | ✅           | ⚠️                   |
| Frontend Implementation       | Form validation                                   |                            | ✅           | ⚠️                   |
| UI/UX (Angular)               | Accessibility (a11y) compliance                   | ✅ (a11y lint)             | ⚠️           | ⚠️                   |

### Legend

| Symbol | Meaning                                |
| ------ | -------------------------------------- |
| ✅     | Fully covered / Primary responsibility |
| ⚠️     | Partial support / Secondary role       |

---

# 4. Considerations and Practical Operations for Legacy Code

## 4.1 Recognize the Gap between Ideal and Reality

- Maintaining all code ideally is difficult
- Prioritize "status quo" or "limited-scope improvements" around legacy code

## 4.2 Focus on New or Modified Code

- Limit review to newly added or modified code
- Register legacy problems as technical debt for separate refactoring

## 4.3 Emphasize Impact Range Identification and Verification

- Legacy code can have tight coupling risks
- Cover changes with unit tests and integration tests as much as possible

## 4.4 Prioritize Continuous Improvement over Perfection

- Do not aim for perfection in one review, prioritize continuous improvement
- Share consensus on trade-offs and improvement points within the team

---

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

---

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

---

# 7. Reference: AI Code Review Efficiency with Model Context Protocol (MCP)

## 7.1 What is MCP?

Model Context Protocol (MCP) structures and provides the context necessary for LLM to conduct accurate reviews.

## 7.2 MCP Settings Example

```json
{
  "model": "claude-3-opus",
  "system_prompt": "You are an expert Laravel reviewer...",
  "context": {
    "diff": "git diff content",
    "spec": "GitLab ticket content",
    "models": ["User.php", "Order.php"],
    "services": ["OrderService.php"],
    "db_schema": "orders, users tables",
    "test_cases": ["tests/Feature/OrderTest.php"]
  },
  "user_prompt": "Please review the diff for risks, SRP violations, and missing tests."
}
```

## 7.3 Execution Example (Python)

```python
from anthropic import Anthropic

client = Anthropic(api_key="sk-xxxx")
response = client.messages.create(
    model="claude-3-opus",
    system="You are a senior Laravel reviewer...",
    messages=[{"role": "user", "content": str(context)}],
    max_tokens=4000
)
print(response.content)
```

## 7.4 Operational Points

| Area                  | Recommendation                                 |
| --------------------- | ---------------------------------------------- |
| **Primary Operation** | Run locally or via script per PR as needed     |
| **CI/CD Operation**   | Use as optional/manual job, not full automatic |

Example GitLab manual job:

```yaml
mcp_ai_review:
  stage: review
  script:
    - python ai_review.py
  when: manual
```

## 7.5 Conclusion

MCP enhances review accuracy by providing context to AI, enabling detection of design flaws, auto-visualizing impact, and balancing quality with speed.

---
