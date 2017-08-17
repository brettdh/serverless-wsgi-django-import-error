# Serverless WSGI app import error demonstration

Demonstrating https://github.com/logandk/serverless-wsgi/issues/24

## Prerequisites

Tested with these; other versions may also demonstrate the issue.
* Node 7.10.0
* npm 4.2.0
* Python 3.6.2
* [Pipenv](http://pipenv.org) 5.1.2

## Setup
```sh
$ npm install -g serverless
$ git clone <url>
$ cd serverless-wsgi-django-import-error
$ npm install
$ pipenv install
$ pipenv shell --compat
```

## To reproduce the issue:
```sh
$ cd api
$ ./manage.py migrate
$ ./manage.py runserver
# Notice that things seem fine
<Ctrl-C>
$ cd ..
$ sls wsgi serve
Traceback (most recent call last):
  File "/Users/brhiggins/src/serverless-wsgi-django-import-error/node_modules/serverless-wsgi/serve.py", line 41, in <module>
    serve(*sys.argv[1:])
  File "/Users/brhiggins/src/serverless-wsgi-django-import-error/node_modules/serverless-wsgi/serve.py", line 22, in serve
    wsgi_module = importlib.import_module(wsgi_fqn[0].replace('/', '.'))
  File "/Users/brhiggins/.local/share/virtualenvs/serverless-wsgi-django-import-error-N1ZX-7Yy/lib/python3.6/importlib/__init__.py", line 126, in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
  File "<frozen importlib._bootstrap>", line 978, in _gcd_import
  File "<frozen importlib._bootstrap>", line 961, in _find_and_load
  File "<frozen importlib._bootstrap>", line 950, in _find_and_load_unlocked
  File "<frozen importlib._bootstrap>", line 655, in _load_unlocked
  File "<frozen importlib._bootstrap_external>", line 678, in exec_module
  File "<frozen importlib._bootstrap>", line 205, in _call_with_frames_removed
  File "/Users/brhiggins/src/serverless-wsgi-django-import-error/api/api/wsgi.py", line 16, in <module>
    application = get_wsgi_application()
  File "/Users/brhiggins/.local/share/virtualenvs/serverless-wsgi-django-import-error-N1ZX-7Yy/lib/python3.6/site-packages/django/core/wsgi.py", line 13, in get_wsgi_application
    django.setup(set_prefix=False)
  File "/Users/brhiggins/.local/share/virtualenvs/serverless-wsgi-django-import-error-N1ZX-7Yy/lib/python3.6/site-packages/django/__init__.py", line 22, in setup
    configure_logging(settings.LOGGING_CONFIG, settings.LOGGING)
  File "/Users/brhiggins/.local/share/virtualenvs/serverless-wsgi-django-import-error-N1ZX-7Yy/lib/python3.6/site-packages/django/conf/__init__.py", line 56, in __getattr__
    self._setup(name)
  File "/Users/brhiggins/.local/share/virtualenvs/serverless-wsgi-django-import-error-N1ZX-7Yy/lib/python3.6/site-packages/django/conf/__init__.py", line 41, in _setup
    self._wrapped = Settings(settings_module)
  File "/Users/brhiggins/.local/share/virtualenvs/serverless-wsgi-django-import-error-N1ZX-7Yy/lib/python3.6/site-packages/django/conf/__init__.py", line 110, in __init__
    mod = importlib.import_module(self.SETTINGS_MODULE)
  File "/Users/brhiggins/.local/share/virtualenvs/serverless-wsgi-django-import-error-N1ZX-7Yy/lib/python3.6/importlib/__init__.py", line 126, in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
  File "<frozen importlib._bootstrap>", line 978, in _gcd_import
  File "<frozen importlib._bootstrap>", line 961, in _find_and_load
  File "<frozen importlib._bootstrap>", line 948, in _find_and_load_unlocked
ModuleNotFoundError: No module named 'api.settings'
```
