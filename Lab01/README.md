# Vulnerabilities in Applications: SQL Injections

## Environment setup

1. `sudo docker-compose up`
2. The appliction is now available at: [http://127.0.0.1:8000](http://127.0.0.1:8000) (inside the VM)

**Note 1:** In all exercises, adding the `?debug=true` argument to the URL will print some debug information.

**Note 2:** In all cases, you must terminate all SQL queries by using `-- //`. The objective is to discard all text after your payload.


## SQL Injection Attacks

### Basic SQL Injections I

The login validation is made by executing the following line:

```SELECT * FROM users where username='".$user."' AND password ='.md5($password).'```

Following this query structure, we put a valid username in the username input like this: `admin' -- //`.

This will make the atribution `username='admin` and ,with `--`, comment the rest of the query (`//` is necessary server language so that this strategy works).

### Basic SQL Injections II

In this exercise we are testing another login page.
In this one, the login validation is made by executing the following line:
`SELECT * FROM users where (username='".$user."') AND (password ='.md5($password).')`.

Following this query structure, we put a valid username in the username input like this: `admin') -- //`. In this cae the closing paranthesis is necessary in order to bypass the query structure.

This will make the atribution `(username='admin)` and ,with `--`, comment the rest of the query (`//` is necessary server language so that this strategy works).

**Note:** In the past two sub-sections we exploited the username field because the pasword field is enctrypted with a hash before it goes to the server, so that field cannot be used to bypass the system.

### SQL Injections 1
