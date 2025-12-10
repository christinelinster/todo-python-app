# 📋 Todo List Manager

Todo list management application built with Flask and PostgreSQL.

## Installation

```bash
git clone https://github.com/christinelinster/todo-python-app.git
cd todo-python-app
poetry install

# Database setup
psql postgres -c "CREATE DATABASE todos;"
psql -d todos -f schema.sql

# Run application
poetry run python app.py
```

**Requirements:** Python 3.11+, PostgreSQL

**Access:** `http://localhost:5003`

## Features

- Create and manage multiple todo lists
- Add, edit, and delete todos within lists
- Mark individual todos as complete/incomplete
- Mark all todos in a list as complete
- Sort lists and todos by completion status
- Flash messages for user feedback
- Custom decorators for resource validation
- PostgreSQL persistence with database abstraction layer

## Tech Stack

- **Backend**: Flask, psycopg2, Gunicorn
- **Frontend**: HTML5, CSS3, JavaScript (Jinja2 templates)
- **Database**: PostgreSQL
- **Deployment**: WSGI production-ready

## Architecture

### Custom Decorators

```python
@require_list  # Validates list exists, injects into route
@require_todo  # Validates todo exists within list, injects both
```

### Database Abstraction

- `DatabasePersistence` class handles all database operations
- Context (`g.storage`) for request-scoped database access
- Utility functions for validation and sorting

### Route Structure

```python
GET    /                           # Redirect to lists
GET    /lists                      # View all lists
POST   /lists                      # Create new list
GET    /lists/new                  # New list form
GET    /lists/:id                  # View single list with todos
POST   /lists/:id                  # Update list title
GET    /lists/:id/edit             # Edit list form
POST   /lists/:id/delete           # Delete list
POST   /lists/:id/todos            # Create todo in list
POST   /lists/:id/todos/:id/toggle # Toggle todo completion
POST   /lists/:id/todos/:id/delete # Delete todo
POST   /lists/:id/complete_all     # Mark all todos complete
```

## Project Structure

```
todo-python-app/
├── app.py                    # Main application and routes
├── wsgi.py                   # WSGI configuration
├── todos/
│   ├── database_persistence.py  # Database operations
│   └── utils.py              # Validation and sorting utilities
├── templates/                # Jinja2 templates
│   ├── lists.html
│   ├── list.html
│   ├── new_list.html
│   └── edit_list.html
├── static/
│   ├── css/
│   └── js/
├── schema.sql                # Database schema
└── pyproject.toml            # Poetry dependencies
```

## Deployment

Production-ready with Gunicorn:

```bash
gunicorn --bind 0.0.0.0:8000 wsgi:app
```

Environment variable support:

```bash
export FLASK_ENV=production  # Disables debug mode
```

## License

MIT

## Contact

Christine Lin  
[GitHub](https://github.com/christinelinster) | [LinkedIn](https://linkedin.com/in/christinelin19/) | [Portfolio](https://christine-lin.vercel.app/)  

Built with ☑️ for the list lovers.
