 How to Deploy to laravel Applications on Heroku.
==============================

![Heroku-Laravel](https://res.cloudinary.com/practicaldev/image/fetch/s--qKHxphj6--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://bosnadev.com/wp-content/uploads/2014/09/laravel_heroku.jpg)
## Introduction
Welcome to a series of Deploying a Laravel Application.

Laravel applications are deployed on many sites.

I will be taking you through on how to deploy a laravel application which has a database and to be specific, Postgresql Database.

## Prerequisites
+ PHP (and ideally some Laravel) knowledge.
+ A Heroku user account: [SignUp is free and instant](https://signup.heroku.com/signup/dc)
+ Familiarity with [getting started with PHP on Heroku](https://devcenter.heroku.com/articles/getting-started-with-php) guide, with the following things installed:
		+PHP.
		+Composer.
		+Heroku CLI.

#### Heroku CLI

![heroku-cli](https://pbs.twimg.com/media/CzuuE7dXAAA7np5.jpg)
To have complete success, we would need the Heroku CLI in our local machine.

To do that we need to run this set of commands in our command-line which will be found [here](https://cli.heroku.com/).


# Laravel Application

Let us embark to our main objective. So we have an application with has a database structure and has migrations with it. 

So as to fit in your shoes I will be assuming you have an application is fully ready to be in production.

After installing the Heroku CLI you may do the following command.

```bash
$ heroku login
heroku: Enter your login credentials
Email [ben.developer.kenny@gmail.com]: 


```

Fill in your credentials and we now set to launch our production application.

# Laravel Application Setup For Deployment

We will head to the project which you want to deploy as shown below:

```bash
$ cd <application name>

```

We will then create or initialize a git repository  and commit our current state

```bash
$git init
$git add .
$git commit -a -m "new Laravel project"
```
We will then add the remote git repo for the application by creating the application.

```bash
$heroku create <application-name>
```
This will consist of the git repo from heroku where we will host of application files.

To deploy your application to Heroku, you must first create a Procfile, which tells Heroku what command to use to launch the web server with the correct settings. 

#### Creating Procfile
By default, Heroku will launch an Apache web server together with PHP to serve applications from the root directory of the project.
However, your application’s document root is the public/ subdirectory, so you need to create a Procfile that [configures the correct document root](https://devcenter.heroku.com/articles/custom-php-settings#setting-the-document-root):

```bash
$ echo web: heroku-php-apache2 public/ > Procfile
$ git commit -a -m "Procfile for Heroku"
```

## Setting environmental variables
By default Laravel offers the environmental variables in a file called .env. We will need to send the information. This is an example of how the env should look like

```txt
APP_NAME=Laravel
APP_ENV=local
APP_KEY=
APP_DEBUG=true
APP_URL=http://localhost

LOG_CHANNEL=stack

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=homestead
DB_USERNAME=homestead
DB_PASSWORD=secret

BROADCAST_DRIVER=log
CACHE_DRIVER=file
QUEUE_CONNECTION=sync
SESSION_DRIVER=file
SESSION_LIFETIME=120

REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379

MAIL_DRIVER=smtp
MAIL_HOST=smtp.mailtrap.io
MAIL_PORT=2525
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null

PUSHER_APP_ID=
PUSHER_APP_KEY=
PUSHER_APP_SECRET=
PUSHER_APP_CLUSTER=mt1

MIX_PUSHER_APP_KEY="${PUSHER_APP_KEY}"
MIX_PUSHER_APP_CLUSTER="${PUSHER_APP_CLUSTER}"

```

We will need to push the appropriate data to heroku using a regex that i found from a great developer called [Jakhax](https://github.com/jakhax)

```bash
$ heroku config:set $(cat .env | sed '/^$/d; /#[[:print:]]*$/d')

```

Remember to set your `DEBUG` to `false` so as to prevent leak of data.

# Configuring the Database

First, add the Heroku add-on for Postgresql. If you were using mysql I request you go to this [link](https://mattstauffer.com/blog/laravel-on-heroku-using-a-mysql-database).

```bash
$ heroku addons:create heroku-postgresql:hobby-dev

```

you should see an output like this:

```bash
Adding heroku-postgresql:hobby-dev on app-name-here... done, v14 (free)
Attached as HEROKU_POSTGRESQL_COLOR_URL
Database has been created and is available
 ! This database is empty. If upgrading, you can transfer
 ! data from another database with pgbackups:restore.
Use `heroku addons:docs heroku-postgresql` to view documentation.

```

As you can see the add-ons creates a database which is empty and doesn't contain any information. We would need to store information into it.

By default you should have `DATABASE_URL` configuration created after installing postgres addon to heroku.

At this point your database should be up and running. Now, let's edit your Laravel config to point to the PostgreSQL database.


#### Configuring the laravel to use PostgresSQL

Once again, if this is real app, you're going to want to only be making these changes in your production configuration settings, but for now we're just hacking at a dummy app.

First, change the value of 'default' in app/config/database.php to 'pgsql'.

```php
'default'=>'pgsql',
```

Set the following at the top of your database.php above the return values:

```php
$url = parse_url(getenv("DATABASE_URL"));

$host = $url["host"];
$username = $url["user"];
$password = $url["pass"];
$database = substr($url["path"], 1);
```

Then change your pgsql entry in that same file to be the following:

```php
'pgsql' => array(
        'driver'   => 'pgsql',
        'host'     => $host,
        'database' => $database,
        'username' => $username,
        'password' => $password,
        'charset'  => 'utf8',
        'prefix'   => '',
        'schema'   => 'public',
    ),
```

That is it! Commit and we are ready to push our changes.

```bash
$ git add .
$ git commit -m "Convert to use Heroku PostgreSQL database"
$ git push heroku master
```

# Pushing The Database

We have two ways of pushing the database.

## First Option
If you want to start a fresh without anything you may the following command:

```bash
$ heroku run php /app/artisan migrate
```

## Second Option
This is done if you want to push your local db to the database in heroku:

```bash
heroku pg:push <The name of the db in the local psql> DATABASE_URL --app <heroku-app>
```


## Open App

You may now open your application :

```bash
$ heroku open
```

# Final Remarks

This process is really confusing but I hope will help in your endevours.

Remember heroku does not offer support for media files in the free tier subscription so find some where else to store those e.g UploadCare(I am making a [documentation](https://github.com/Benard18/Laravel-CloudShare-Intergration) based on this).


# Very Helpful Articles

+[Configuring Postgre](https://mattstauffer.com/blog/laravel-on-heroku-using-a-postgresql-database/)
+[Getting started with heroku laravel](https://devcenter.heroku.com/articles/getting-started-with-laravel)
+[Best Practices](https://devcenter.heroku.com/articles/getting-started-with-laravel#best-practices)
+[Further Reading](https://devcenter.heroku.com/articles/getting-started-with-laravel#further-reading)