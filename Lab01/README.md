# Vulnerabilities in Applications: SQL Injections

## Environment setup

1. `sudo docker-compose up`
2. The appliction is now available at: [http://127.0.0.1:8000](http://127.0.0.1:8000) (inside the VM)

**Note 1:** In all exercises, adding the `?debug=true` argument to the URL will print some debug information.

**Note 2:** In all cases, you must terminate all SQL queries by using `-- //`. The objective is to discard all text after your payload.


## SQL Injection Attacks

### Basic SQL Injections I

The login validation is made by executing the following line:

`SELECT * FROM users where username='".$user."' AND password ='.md5($password).'`

Following this query structure, we put a valid username in the username input like this: `admin' -- //`.

This will make the atribution `username='admin` and ,with `--`, comment the rest of the query (`//` is necessary server language so that this strategy works).

### Basic SQL Injections II

In this exercise we are testing another login page.

In this one, the login validation is made by executing the following line:
`SELECT * FROM users where (username='".$user."') AND (password ='.md5($password).')`.

Following this query structure, we put a valid username in the username input like this: `admin') -- //`. In this cae the closing paranthesis is necessary in order to bypass the query structure.

This will make the atribution `(username='admin)` and ,with `--`, comment the rest of the query (`//` is necessary server language so that this strategy works).

**Note:** In the past two sub-sections we exploited the username field because the pasword field is enctrypted with a hash before it goes to the server, so that field cannot be used to bypass the system.

### SQL Injections 

In this section we are testing a form that enables the search for products by partial matching of a string or character.

The query is made by executing the following line:

`"SELECT * FROM products WHERE product_name LIKE'".$query."%'`

There are several ways to take advantage of this and extract information:

 1. `b%' order by 5 -- //`
	 
    The `order by N` directive is ordering the displayed information by the **column N**, in this way we can manipulate the order of data as we want.
    
    The first part of the query (`CHARS%`) is necessary and lets us search for products that start with the pattern defined by **CHARS**.
	 
2. `'union select null,id,username,password,fname from users -- // `
	
    Following the same logic as in the first one, we can use the `union` directive followed by a `select` to populate the table we the information we want to see.
    
    In this specific example, we are able to access and see information about all users stored in the database.

3.  `'union select 1,'<img src="http://address"><img>',3,4,5 -- //`
	
    In this example (a little bit like the previous one), we are changing the information displayed and adding html code to display an image of our choice.

4. `'union select null,null,null,null,table_name from information_schema.tables -- //`
	
    One last example where we change the table's information and diplay the name of all the tables stored in the database.
