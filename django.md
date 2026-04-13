# Django Review Guidelines

## Auth & Permissions (SimpleJWT)
- Flag if: view uses IsAuthenticated but no object-level check on detail/update/delete → CRITICAL
- Flag if: SIMPLE_JWT ROTATE_REFRESH_TOKENS or BLACKLIST_AFTER_ROTATION disabled, or SIGNING_KEY = SECRET_KEY in prod settings → HIGH
- Flag if: get_queryset returns Model.objects.all() on user-scoped resource without filter(user=request.user) → CRITICAL

## Database & Queries (PostgreSQL)
- Flag if: serializer accesses obj.fk.field or obj.related_set without select_related/prefetch_related in view queryset → HIGH
- Flag if: SerializerMethodField loops queryset or calls .filter()/.count() per item → HIGH
- Flag if: loop over queryset with .save()/.delete() instead of bulk_update/bulk_create/queryset.update → MEDIUM
- Flag if: migration adds non-null column without default on large table, or adds index without Meta.indexes/CONCURRENTLY → HIGH

## Celery Tasks
- Flag if: task signature passes Django model instance instead of pk/id → HIGH
- Flag if: @shared_task without bind=True + autoretry_for/retry_backoff on network/IO task → HIGH
- Flag if: task triggered inside transaction.atomic block without transaction.on_commit wrapper → HIGH
- Flag if: signal handler (post_save etc.) calls task.delay without on_commit → HIGH

## AWS / botocore
- Flag if: boto3.client called inside view/task body instead of module-level → MEDIUM
- Flag if: S3/botocore call without try/except ClientError or BotoCoreError → HIGH
- Flag if: AWS credentials passed as kwargs to boto3.client instead of IAM role/env → CRITICAL
- Flag if: generate_presigned_url without ExpiresIn, or ExpiresIn > 3600 on private object → HIGH

## Serializers & Filtering
- Flag if: ModelSerializer with fields = '__all__' on model containing user/permission/internal fields → CRITICAL
- Flag if: FilterSet uses '__all__' or exposes filter on password/token/email lookup → HIGH
- Flag if: writable nested serializer without create/update override → HIGH
