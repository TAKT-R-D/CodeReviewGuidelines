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
