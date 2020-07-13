# d8tam-indy-cloud-agent-poc



## Demos

Home Page:

![home](readme_media/home.gif "home")

Registering User:

![registering](readme_media/register.gif "register")

Admin Editing Page:

![edit page](readme_media/editpage.gif "editpage")

Admin Editing Users:

![edit user](readme_media/edituser.gif "edituser")


##### Clone the repository 

```
$ git clone https://github.com/YOUR_USERNAME/REPO_NAME.git
$ cd REPO_NAME
```

##### Initialize a virtual environment

Unix/MacOS:
```
$ python3 -m venv venv
$ source venv/bin/activate
```
Learn more in [the documentation](https://docs.python.org/3/library/venv.html#creating-virtual-environments).

##### (If you're on a Mac) Make sure xcode tools are installed

```
$ xcode-select --install
```

##### Add Environment Variables

Create a file called `config.env` that contains environment variables. **Very important: do not include the `config.env` file in any commits. This should remain private.** You will manually maintain this file locally, and keep it in sync on your host.

Variables declared in file have the following format: `ENVIRONMENT_VARIABLE=value`. You may also wrap values in double quotes like `ENVIRONMENT_VARIABLE="value with spaces"`.

1. In order for Flask to run, there must be a `SECRET_KEY` variable declared. Generating one is simple with Python 3:

   ```
   $ python3 -c "import secrets; print(secrets.token_hex(16))"
   ```

   This will give you a 32-character string. Copy this string and add it to your `config.env`:

   ```
   SECRET_KEY=Generated_Random_String
   ```

   2. The mailing environment variables can be set as the following.
   We recommend using [Sendgrid](https://sendgrid.com) for a mailing SMTP server, but anything else will work as well.

   ```
   MAIL_USERNAME=SendgridUsername
   MAIL_PASSWORD=SendgridPassword
   ```

Other useful variables include:

| Variable        | Default   | Discussion  |
| --------------- |-------------| -----|
| `ADMIN_EMAIL`   | `flask-base-admin@example.com` | email for your first admin account |
| `ADMIN_PASSWORD`| `password`                     | password for your first admin account |
| `DATABASE_URL`  | `data-dev.sqlite`              | Database URL. Can be Postgres, sqlite, etc. |
| `REDISTOGO_URL` | `http://localhost:6379`        | [Redis To Go](https://redistogo.com) URL or any redis server url |
| `RAYGUN_APIKEY` | `None`                         | API key for [Raygun](https://raygun.com/raygun-providers/python), a crash and performance monitoring service |
| `FLASK_CONFIG`  | `default`                      | can be `development`, `production`, `default`, `heroku`, `unix`, or `testing`. Most of the time you will use `development` or `production`. |

##### Install the dependencies

```
$ pip install -r requirements.txt
```

**Redis:**

_Mac (using [homebrew](http://brew.sh/)):_

```
$ brew install redis
```

You will also need to install **PostgresQL**

_Mac (using homebrew):_

```
brew install postgresql
```
##### Create the database

```
$ python manage.py recreate_db
```

##### Other setup (e.g. creating roles in database)

```
$ python manage.py setup_dev
```

Note that this will create an admin user with email and password specified by the `ADMIN_EMAIL` and `ADMIN_PASSWORD` config variables. If not specified, they are both `flask-base-admin@example.com` and `password` respectively.

##### [Optional] Add fake data to the database

```
$ python manage.py add_fake_data
```

## Running the app

```
$ source env/bin/activate
$ honcho start -e config.env -f Local
```

## Gettin up and running with Docker

Currently we have a `Dockerfile` intended for testing purposes and it automates the whole cycle of running the application, setting up the database and redis. 

##### How to use the docker file 
In only three simple steps :
- change the variables `MAIL_USERNAME` , `MAIL_PASSWORD` and `SECRET_KEY`
- `docker build -t <image_name> . 
- `docker run -it -d -p 5000:5000 --name <container name> <image_name> /bin/bash`
- To run in foreground mode `docker run -it -p 5000:5000 --name <container name> <image_name> /bin/bash`

##### Note

A more robust version with docker-compose is being developed to separate redis in separate container and allow the deployment of production-level applications automatically without the need of manual provisioning

## Formatting code

Before you submit changes to flask-base, you may want to autoformat your code with `python manage.py format`