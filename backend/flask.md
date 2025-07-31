# Flask

Official documentation: https://flask.palletsprojects.com/en/stable/

Flask project made by me: [Financas](https://github.com/Iauar-repo/financas/blob/main/backend/README.md)

## CRUD Flow

### How to create routes

- GET as default

    ```py
    @app.route("/")
    def hello_world():
    return "<p>Hello, World!</p>"
    ```
- Specifying methods
    ```py
    @app.route("/", methods=['GET', 'POST'])
    def hello_world():
        return "<p>Hello, World!</p>"
    ```
- Different methods into different functions
    ```py
    @app.get('/login')
    def login_get():
        return show_the_login_form()

    @app.post('/login')
    def login_post():
        return do_the_login()
    ```

