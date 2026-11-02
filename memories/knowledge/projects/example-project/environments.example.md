---
type: fact
project: example-project
evidence_status: provisional
source: "<source document or attributed request>"
updated_at: "<actual write timestamp>"
# verified_at: "<actual verification timestamp; add only after checking>"
# verification: "<source revision or check and observed result>"
---

Template only. Replace illustrative values; mark verified only after checking the source and recording the actual `verified_at`.

# Environments & Infrastructure

## Staging
- **Auth Service:** `http://auth.staging.internal:8080` (Vault: `secret/data/staging/auth`)
- **Postgres DB:** `postgres.staging.internal:5432`, db: `auth_db`, user: `app_user` (Vault: `secret/data/staging/postgres#password`)
- **Redis Cache:** `redis.staging.internal:6379`, db: 0 (Vault: `secret/data/staging/redis#token`)
