# Plan: Bootstrap django-htmx Stack for claude-monitor

## Summary

Bootstrap a Django + HTMX application within the claude-monitor project.

## Configuration

- **Stack**: django-htmx
- **Folder**: `app/`
- **App name**: claude-monitor
- **Target path**: `/workspace/z/projects/personal/claude-monitor/app/`
- **Database**: SQLite (development)
- **Package manager**: uv (per Rule 050)

## Implementation Steps

### 1. Create project directory structure

```
app/
├── config/
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── core/
│   ├── __init__.py
│   ├── models.py
│   ├── views.py
│   ├── urls.py
│   └── templates/core/
├── templates/
│   └── base.html
├── static/
├── manage.py
├── pyproject.toml
├── Dockerfile
├── docker-compose.yml
└── .env.example
```

### 2. Initialize Python project with uv

```bash
cd /workspace/z/projects/personal/claude-monitor
mkdir -p app && cd app
uv init --name claude-monitor
```

### 3. Add dependencies

```bash
uv add django django-htmx whitenoise
uv add --dev pytest pytest-django
```

### 4. Create Django project

```bash
uv run django-admin startproject config .
uv run python manage.py startapp core
```

### 5. Configure Django settings

- Add `django_htmx` to INSTALLED_APPS
- Configure SQLite database (default)
- Add HTMX middleware
- Configure static files with whitenoise
- Add structlog for logging (per Rule 050)

### 6. Create base template with HTMX

- Include HTMX CDN in base.html
- Set up template inheritance structure

### 7. Create Dockerfile and docker-compose.yml

- Multi-stage Python build
- Development compose configuration

### 8. Verify installation

```bash
uv run python manage.py migrate
uv run python manage.py runserver
```

## Files to Create

| File | Purpose |
|------|---------|
| `app/pyproject.toml` | Python project configuration |
| `app/manage.py` | Django management script |
| `app/config/settings.py` | Django settings |
| `app/config/urls.py` | URL routing |
| `app/config/wsgi.py` | WSGI configuration |
| `app/core/models.py` | Data models |
| `app/core/views.py` | View handlers |
| `app/core/urls.py` | App URL patterns |
| `app/templates/base.html` | Base template with HTMX |
| `app/Dockerfile` | Container build |
| `app/docker-compose.yml` | Development environment |
| `app/.env.example` | Environment template |

## Issue Identified

**Stack-manager improvement needed**: The agent should not offer `/workspace` as a location option. It should always default to `/workspace/{SLUG_PATH}` pattern.

## Post-Implementation

1. Run migrations and verify server starts
2. Invoke git-manager skill to commit changes
