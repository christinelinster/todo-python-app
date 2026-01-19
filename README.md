# Todos Application (PY175)

**Repository:** [https://github.com/christinelinster/todo-python-app](https://github.com/christinelinster/todo-python-app)

This is a dynamic CRUD (Create, Read, Update, Delete) web application built using the **Flask** microframework. Unlike the stateless echo servers built earlier in the course, this application utilizes **Client-Side Sessions** to persist state across HTTP requests, simulating a multi-user environment where each user maintains their own list of tasks.

## Core Features

### Task Management (CRUD)
* **Create:** Users can create new Todo Lists and add individual tasks to them.
* **Read:** View a dashboard of all lists and drill down into specific lists to see pending tasks.
* **Update:** Mark tasks as "Complete" or "Incomplete" and rename existing lists.
* **Delete:** Remove tasks or entire lists with a confirmation step to prevent accidental deletion.


### User Experience (UX)
* **Flash Messages:** Provides immediate feedback to the user after actions (e.g., "The list has been created," "Task updated").
* **Input Validation:** Prevents invalid data submission (e.g., creating a list with an empty title or a duplicate name).
* **Confirmation Dialogs:** Intercepts destructive actions (like deletion) to request user confirmation.
* **Responsive Layout:** Uses a consistent `layout.html` template with Jinja2 blocks to render content dynamically.

### Technical Implementation
* **Session Persistence:** Uses signed cookies (`session` object) to store data, allowing the application to "remember" the state of lists and tasks between requests.
* **Jinja2 Templating:** extensive use of template filters (custom filters for sorting lists) and macros.
* **Request Hooks:** Utilizes `@app.before_request` to load global session data before each view function executes.

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

## Installation & Setup

1. **Clone the repository:**
   ```bash
   git clone [https://github.com/christinelinster/todo-python-app.git](https://github.com/christinelinster/todo-python-app.git)
   cd todo-python-app
   ```

2. **Install dependencies:**
   ```bash
   poetry install
   ```

3. **Set up database:**
   ```bash
   psql postgres -c "CREATE DATABASE todos;"
   psql -d todos -f schema.sql
   ```
4. **Run application:**
   ```bash
   poetry run python app.py
   ```

   **Requirements:** Python 3.11+, PostgreSQL
   The app will be accessible at `http://localhost:5003`

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

