# Serverless WSGI app import error demonstration

Demonstrating https://github.com/logandk/serverless-wsgi/issues/24

Worked around by dev-installing local Django project package.

## Prerequisites

Tested with these; other versions may also demonstrate the issue.
* Node 7.10.0
* npm 4.2.0
* Python 3.6.2
* [Pipenv](http://pipenv.org) 5.1.2

## Setup
```sh
# Install dependencies
$ npm install -g serverless
$ git clone https://github.com/brettdh/serverless-wsgi-django-import-error.git
$ cd serverless-wsgi-django-import-error
$ npm install
$ pipenv install
$ pipenv shell --compat

# to dev-install local modules
$ pip install -r requirements.txt
```

## Verifying that it works:
```sh
$ cd api
$ ./manage.py migrate
$ ./manage.py runserver
# Notice that things seem fine
<Ctrl-C>
$ cd ..
$ sls wsgi serve
 * Running on http://localhost:5000/ (Press CTRL+C to quit)
 * Restarting with stat
 * Debugger is active!
 * Debugger PIN: 436-332-544
<Ctrl-Z>
$ bg

$ http POST http://localhost:5000/ping <<EOF
{"test":"test"}
EOF
127.0.0.1 - - [17/Aug/2017 19:37:57] "POST /ping HTTP/1.1" 200 -

HTTP/1.0 200 OK
Allow: OPTIONS, POST
Content-Length: 17
Content-Type: application/json
Date: Thu, 17 Aug 2017 19:37:57 GMT
Server: Werkzeug/0.12.2 Python/3.6.2
Vary: Accept, Cookie
X-Frame-Options: SAMEORIGIN

{
    "hello": "world"
}
