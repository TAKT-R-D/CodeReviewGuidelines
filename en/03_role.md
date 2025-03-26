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
