# Flask

Official documentation: https://flask.palletsprojects.com/en/stable/

Flask project made by me: [Financas](https://github.com/Iauar-repo/financas/blob/main/backend/README.md)

## Common libraries
- flask_sqlalchemy
- flask_jwt_extended
- flask_cors
- flask_limiter
- flask_bcrypt

## Archtecture

The best design you can use with Flask is **Application Factory with Blueprints**. This layout let you build large modular applications.

### Basic folder layout
```
app/
├── __init__.py          # Create app object
├── config.py            # Load .env and global variables
├── extensions.py        # Initiate instances
├── models.py            # Database models
├── auth/
│   ├── __init__.py      # Blueprint auth
│   ├── repository.py    # Data access layer
│   ├── routes.py        # Endpoints
│   ├── schemas.py       # Payload validator
│   ├── service.py       # Business logic
|   ├── utils.py         # Generic helper functions
```
<sub>`auth/` is an example blueprint

## CRUD Flow

1. Routes/Controllers →
2. Services →
3. Repositories/Models

_Check the example project to code references._

## Database connection / Migrations
extensions.py
```py
from flask_sqlalchemy import SQLAlchemy
from flask_migrate import Migrate

db = SQLAlchemy()
migrate = Migrate()
```

config.py
```py
SQLALCHEMY_DATABASE_URI = 'postgresql://user:pass@host:port/dbname'
SQLALCHEMY_TRACK_MODIFICATIONS = False
```

migration
```bash
flask db init     # first time only
flask db migrate  # generate script
flask db upgrade  # apply the changes
```

## Typing
To functions and variables is recommended, but not necessary, since Python is a dynamic language.

You can ensure data types and validation with schemas using **Marshmallow**.
```py
from marshmallow import Schema, fields, validate

class LoginSchema(Schema):
    email = fields.Email(required=True, load_only=True, validate=validate.Length(min=1))
    password = fields.Str(required=True, load_only=True, validate=validate.Length(min=8))

login_schema = LoginSchema()
```

## Routing

routes.py
```py
from flask import Blueprint

auth_bp = Blueprint('auth', __name__, url_prefix='/auth')

@auth_bp.route('/login', methods=['POST'])
def login(): ...
```

Register at the factory (app creating)
```py
app.register_blueprint(auth_bp)
```

Parameters and Query Strings
- `<int:id>`, `<string:slug>`
- `request.args.get('page', 1)`

## Task schedule
For simple jobs inside Flask application
```py
from apscheduler.schedulers.background import BackgroundScheduler

scheduler = BackgroundScheduler()
scheduler.add_job(func=clear_logs, trigger='cron', hour=3)
scheduler.start()
```

## Real-time communication
Flask allows real-time with `Flask‑SocketIO` and `grpcio`.

## Unit testing
- pytest + pytest‑flask
- Factories with factory_boy to make test data
- Coverage (pytest --cov) to ensure the minimum.

**e.g.**
```py
def test_create_user(client, db):
    response = client.post('/users', json={'email': 'a@b.com', 'pwd': '1234'})
    assert response.status_code == 201
    data = response.get_json()
    assert 'id' in data
```