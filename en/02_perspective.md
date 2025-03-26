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
