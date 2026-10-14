# Claim Check – Lost and Found System

Flask + OOP final project. Data lives in Python lists (no database), so it resets on restart.

## Run locally
    pip install -r requirements.txt
    python app.py        # http://127.0.0.1:5000

## Test accounts
| Role | Username | Password |
|---|---|---|
| Administrator | admin | admin123 |
| Lost & Found Officer (staff) | staff | staff123 |
| Student | student | student123 |

## Roles
- **Administrator**: full CRUD on found items, lost reports, claims and accounts.
- **Officer**: logs and edits found items, searches everything, approves or rejects claims.
- **Student**: reports lost items (edit/delete own while open), browses and searches found items, claims items, tracks claims.

## OOP map
- Inheritance: `Record` → `User` → `Administrator` / `Staff` / `Student`; `Record` → `Item` → `FoundItem` / `LostReport`; `Record` → `Claim`
- Encapsulation: private `__username`, `__password_hash` (User); private `__identifier` on `FoundItem` (hidden detail used to verify claims); status changes only through methods
- Polymorphism: each user class overrides `permissions()`; `LostReport` overrides `can_edit()` / `can_delete()` so owners can manage their own reports; all records override `summary()`
- Exceptions: `LostFoundError` → `DuplicateError`, `NotFoundError`, `InvalidOperationError`

## Deploy (Render)
1. Push to GitHub. 2. New Web Service → connect repo. 3. Build: `pip install -r requirements.txt`; Start: `gunicorn app:app --workers 1 --threads 4`.
4. Add env var `SECRET_KEY` (any random string).
**Keep `--workers 1`**: list storage is per process, so more workers would each hold different data.
