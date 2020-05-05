![Shhh](https://i.imgur.com/0MPGbQj.png)

[![Build Status](https://travis-ci.com/smallwat3r/shhh.svg?branch=master)](https://travis-ci.com/smallwat3r/shhh)
[![codecov](https://codecov.io/gh/smallwat3r/shhh/branch/master/graph/badge.svg)](https://codecov.io/gh/smallwat3r/shhh)
[![Maintainability](https://api.codeclimate.com/v1/badges/f7c33b1403dd719407c8/maintainability)](https://codeclimate.com/github/smallwat3r/shhh/maintainability)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://github.com/smallwat3r/shhh/blob/master/LICENSE)

## What is it?

**Shhh** is a tiny Flask app to create encrypted secrets and share 
them securely with people. The goal of this application is to get rid
of plain text sensitive information into emails or chat logs.  

Shhh is deployed [here](https://shhh-encrypt.herokuapp.com/), but
**it's better for organisations and people to deploy it on their own
personal / private server** for even better security. You can find
in this repo everything you need to host the app yourself.  

Or you can **one-click deploy to Heroku** using the below button.
It will generate a fully configured private instance of Shhh 
immediately (using your own server and Postgres database, for free).  

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy?template=https://github.com/smallwat3r/shhh)

## How does it work?

The sender has to set an expiration date along with a passphrase to
protect the information he wants to share.  

A unique link is generated by Shhh that the sender can share with the
receiver in an email, alongside the temporary passphrase he created
in order to reveal the secret.  

The secret will be **permanently removed** from the database as soon 
as one of these events happens:  

* the expiration date has passed (max 7 days).  
* or the receiver has decrypted the message.  

The secrets are encrypted in order to make the data anonymous, 
especially in the database, and the passphrases are not stored 
anywhere  

_Encryption method used: Fernet with password, random salt value and
strong iteration count (100 000)._  

_Tip: For added security, avoid telling in Shhh what is the use of
the secret you're sharing. Instead, explain this in your email, and 
copy paste the Shhh link with the passphrase so the user can retrieve
it._  

## Is there an API?

Yes, you can find some doc [here](https://github.com/smallwat3r/shhh/tree/master/shhh/api).  


Also, checkout [shhh-cli](https://github.com/smallwat3r/shhh-cli), 
a Go client to interact with Shhh API via command line.  


## What's the stack?

### Core application
* **[Flask](https://flask.palletsprojects.com/en/1.1.x/)**, used as
our Python backend web-framework.  
* **[Postgres](https://www.postgresql.org/)** used to store only: 
the unique links, the encrypted messages, the creation and expiration
dates.  
* **[Bulma](https://bulma.io/)**, the CSS framework.  


### Tools
* **[Adminer](https://www.adminer.org/)**, check database records.  


## What are the dependencies?

You can find the list of the Python dependencies 
[here](https://github.com/smallwat3r/shhh/blob/master/requirements.txt).  

You can find the list of the frontend dependencies 
[here](https://github.com/smallwat3r/shhh/blob/master/package.json).

## How to launch Shhh locally?

These methods are for development purpose only. For production / 
public use you might want to use a more secure configuration.

<details>
<summary><b>Launch it natively</b></summary>

#### Postgres  

You will need a Postgres server running on localhost in the 
background. Create a database named Shhh.

```sql
CREATE DATABASE IF NOT EXISTS shhh;
```

#### Flask  

```sh 
git clone https://github.com/smallwat3r/shhh.git && cd shhh
```

We recommend that you create a Python virtual environment for this 
project, so you can install the required dependencies.  

```sh
python -m venv env 
source env/bin/activate
pip install -r requirements.txt
pip install -r test-requirements.txt  # optional
```

Install the frontend dependencies:
```sh
# need yarn (brew install yarn) or (sudo apt install yarn)
yarn install
```

Stay in the virtual environment created.  

You then need to set up a few environment variables. These will be 
used to configure Flask, as well as the application connection to the 
database.  

```sh
export FLASK_APP=shhh
export FLASK_ENV=dev-local
export FLASK_DEBUG=1

export POSTGRES_HOST=localhost
export POSTGRES_DB=shhh
export POSTGRES_USER=<username>
export POSTGRES_PASSWORD=<password>
export POSTGRES_PORT=<port>
```


You can now launch Shhh with:
```sh
flask run
```

or using gunicorn:
```sh
gunicorn -b :5000 -w 3 wsgi:app --preload
```

You can now access Shhh at http://localhost:5000  

</details>

<details>
<summary><b>Launch it with docker-compose (recommended)</b></summary>

#### docker-compose  

You will need Docker and docker-compose installed on your machine.

```sh
docker-compose up --build  # start app
docker-compose stop        # stop app
```

or via Makefile:

```sh
make dc-start  # start app
made dc-stop   # stop app
```

Once the container image has finished building and starting, you can
access:  

* Shhh at http://localhost:5000
* Database records using Adminer at http://localhost:8080

</details>

## Run the tests

You can run the tests using:  
```sh
make tests
```

Run Pylint report using:  
```sh
make lint
```

Run Bandit report using:  
```sh
make secure
```

## Credits

#### Existing cool apps that gave me the idea to develop my own version using Python and Flask

* [OneTimeSecret](https://github.com/onetimesecret/onetimesecret)
* [PasswordPusher](https://github.com/pglombardo/PasswordPusher)

#### Thanks to

* [@AustinTSchaffer](https://github.com/AustinTSchaffer) for 
contributing to set-up a Docker environment.
* [@kleinfelter](https://github.com/kleinfelter) for finding bugs 
and security issues.

## License

See [LICENSE](https://github.com/smallwat3r/shhh/blob/master/LICENSE)
file.  

## Contact

Please report issues or questions 
[here](https://github.com/smallwat3r/shhh/issues).
