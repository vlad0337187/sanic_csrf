### sanic_csrf

It's not ready yet!

Checks request for CSRF token.
If CSRF is absent or invalid:
    If it's a get request - installs new token in cookie and redirects to same page.
    In other cases - raises 403 exception (forbidden).

Requires some session middleware to be installed.
That session plugin must use such interface:
	```python
    request['session'].get('variable') or request['session']['variable']
    request['session']['variable'] = 4
    ```
For example, 'sanic_session' (https://github.com/subyraman/sanic_session) will fit.


## Example

A simple example:


```python
    from sanic import Sanic
    from sanic.response import text
    import sanic_session
    import sanic_csrf


    app = Sanic()
    sanic_session.install_middleware(app, 'InMemorySessionInterface')
    sanic_csrf.install_middleware(app)


    @app.route("/")
    async def index(request):
        # interact with the session like a normal dict
        if not request['session'].get('foo'):
            request['session']['foo'] = 0

        request['session']['foo'] += 1

        return text(request['session']['foo'])

    if __name__ == "__main__":
        app.run(host="0.0.0.0", port=8000, debug=True)
```


