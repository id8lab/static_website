# Static Website

This repo contain a guideline on how to create a static website.

Here is what you will find in this project

- What is HTML and CSS
- How to create a static web page
- How to install (deploy) the page to a server (nginx)

---

### What is HTML and CSS

HTML is the markup language, used to build up a web page.

an example of a html page code could be like this:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Page Title</title>
  </head>
  <body>

    <h1>This is a Heading</h1>
    <p>This is a paragraph.</p>

  </body>
</html>
```
CSS is the style code you can add to the page.
The code is normally stored in a .css file and may look like this:

```css
body {
    background-color: lightblue;
}

h1 {
    color: white;
    text-align: center;
}

p {
    font-family: verdana;
    font-size: 20px;
}
```
Normally the css code is included by referring to the stylesheet file:

```html
<!DOCTYPE html>
<html>
  <head>
    <link rel="stylesheet" type="text/css" href="style.css">
  </head>
..
...
</html>

```

You can find more information on [w3schools](https://www.w3schools.com/html/) website about HTML, CSS, Javascript and more.

---

### How to create a static web page
Let us log into vagrant and create a static website.

Remember that your Vagrantfile must have a forwarding code
```
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.network :forwarded_port, guest: 80, host: 4567
end
```

This means, that when you will access the website you will have to use port 4567 in the browser.

log in to your vagrant box:
```bash
$ vagrant ssh
```
now update the instance and install [nginx](https://www.nginx.com/resources/wiki/start/). nginx [guide](https://nginx.org/en/docs/beginners_guide.html).

```bash
$ sudo apt-get update
$ sudo apt-get install nginx
```

normally, you will want to place your website file in /usr/share/nginx/www/
create the folder mvpsite folder and create an index.html in that folder

```bash
$ vi /var/www/index.html
```
enter the following content

```html
<!DOCTYPE html>
<html>
  <head>
    <title>JCU MVP Club </title>
  </head>
  <body>

    <h1>This is my first static page</h1>
    <p>Welcome to my website.</p>

  </body>
</html>
```

---

### How to install (deploy) the page to a server (nginx)
With the page create, we need to setup the webserver. nginx has some configuration files. So lets go and create the pointer to this website.

Go to the nginx configuration folder:
```bash
$ cd /etc/nginx/sites-available
```
create your site configuration file
```bash
$ vi static-website
```
enter the following data

```
server {
  listen 0.0.0.0:80;
  server_name localhost domain.local domain;
	root /usr/share/nginx/www/mvpsite;

	index index.html index.htm;

	location / {
		try_files $uri $uri/ /index.html;
	}
}
```
Once created setup the symbolic link in sites-enabled

```bash
$ cd ..
$ cd sites-enabled
$ sudo ln -s ../sites-available/static_website .
$ ls -l
```


test and restart the webserver:
```
$ sudo nginx -t

$ sudo service nginx restart
```

test your website in a browser with URL
http://127.0.0.1:4567 or http://localhost:4567

---
