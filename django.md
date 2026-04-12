# Django REST Framework Code Review Guidelines

## Views & ViewSets

- Use `ViewSet` for standard CRUD. Use `APIView` only for non-standard logic.
- Flag permission classes missing from views — every view must declare `permission_classes`.
- Flag missing `authentication_classes` on sensitive endpoints.

## Serializers

- Validate input in serializers, not in views. Flag validation logic placed in view methods.
- Flag serializers using `fields = "__all__"` — fields should be explicit.
- Flag missing `read_only_fields` for fields that should not be writable (e.g., `id`, `created_at`).

## Database & Queries

- **N+1 Queries (CRITICAL)**: Flag querysets accessing related objects inside loops without `select_related()` or `prefetch_related()`.
- Flag `Model.objects.all()` where filtering is more appropriate.
- Flag database queries inside serializer `to_representation` or `validate_*` methods that will fire per-object.

## API Design

- Flag endpoints not following REST conventions (e.g., action verbs in URLs like `/get_user/`).
- Flag list endpoints without pagination.
- Flag inconsistent HTTP status codes (e.g., returning 200 for a created resource instead of 201).

## Security

- Flag views missing permission checks that expose data to unauthenticated users.
- Flag raw SQL queries using string formatting — use parameterised queries or the ORM.
- Flag sensitive fields (passwords, tokens) returned in API responses.

## Error Handling

- Flag bare `except:` blocks or `except Exception:` with no logging.
- Flag endpoints returning 500 errors for predictable user input errors — these should return 400.
