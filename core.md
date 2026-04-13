# Core Review Guidelines

## SOLID
- Flag if: React component mixes fetch + state + logic + JSX, >150 lines → HIGH
- Flag if: DRF view inlines ORM query + business logic + response shaping → HIGH
- Flag if: if/elif chain >3 branches on type/status string → MEDIUM
- Flag if: boto3/firebase client instantiated inside view, component, or task → MEDIUM

## Clean Code
- Flag if: function >50 lines or >4 params → MEDIUM
- Flag if: nesting depth >3 → MEDIUM
- Flag if: magic number/string in logic, not named constant → MEDIUM
- Flag if: commented-out code, unused import/var, or code after return → MEDIUM

## Security
- Flag if: API key, JWT, password, AWS/Firebase secret, or DB URL hardcoded → CRITICAL
- Flag if: token/PII written to localStorage, sessionStorage, or console.log → CRITICAL
- Flag if: DRF view lacks permission_classes, uses AllowAny on write, or serializer fields='__all__' → CRITICAL
- Flag if: .raw(), .extra(), or cursor.execute with f-string/%/concat of user input → CRITICAL
- Flag if: dangerouslySetInnerHTML, eval, or new Function on non-constant input → CRITICAL
- Flag if: CORS_ALLOW_ALL_ORIGINS=True, DEBUG=True, or ALLOWED_HOSTS=['*'] committed → CRITICAL

## Error Handling
- Flag if: bare except or except Exception with pass/log-only/print → HIGH
- Flag if: fetch/axios without .catch, try/catch, or response.ok check → HIGH
- Flag if: await without try/catch and error not propagated → HIGH
- Flag if: Celery task lacks retry or try/except around external I/O → MEDIUM
