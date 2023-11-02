# OpenCart Docker development setup

## Intro

All the docker-related files are in the `docker` folder.
This package contains PHP 8.0 with Apache2, MySQL, phpMyAdmin, Mailcatcher services.

## Setup

Create an .env file, and define your variables. See `.env.example`.
In `docker-compose.yml` and in `docker/php-apache/Dockerfile` change the versions for PHP and MySQL if you need to.

In `docker/php-apache/sites/default.apache.conf` set your ServerName to your custom domain of choice (the default is `opencart.test`). Make sure to include this line in your `hosts` file: `127.0.0.1 opencart.test` with your domain.

cd to `docker` folder and run to build the containers:

```bash
(set -a; source .env; docker-compose up --build)
```

## OpenCart installation

Download the zip file from the website.
Extract it into the root folder of your project.
Start the containers if they are not running.

Visit `opencart.test/upload` (or your custom domain) in your browser and go through the installation steps.
The db credentials should match with the values in your .env file.
DB host should be `mysql` instead of `localhost`.

You can choose either PDO or mysqli as the db driver (both are installed).

You can of course rename the uploads folder, and setup the software differently.


## Logs

Apache2 server logs are in the `docker/logs` folder


## Mailcatcher

Set up on admin page at **System -> Settings -> Edit store -> Mail** tab:

Mail Engine: SMTP
Hostname: mailcatcher
Port: 1025

Username and Password should be empty.

In `/var/www/html/your-folder/system/library/mail/smtp.php` -> comment out these conditions temporarily:

```php
if (empty($this->option['smtp_username'])) {
	throw new \Exception('Error: SMTP username required!');
}

if (empty($this->option['smtp_password'])) {
	throw new \Exception('Error: SMTP password required!');
}
```


## Docker

Start containers:

```bash
(docker-compose up)
```

Destroy the running containers:

```bash
(docker-compose down)
```