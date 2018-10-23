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


## Laravel Application

Let us embark to our main objective. So we have an application with has a database structure and has migrations with it. 

So as to fit in your shoes I will be assuming you have an application is fully ready to be in production.

After installing the Heroku CLI you may do the following command.

```bash
$ heroku login
heroku: Enter your login credentials
Email [ben.developer.kenny@gmail.com]: 


```

Fill in your credentials and we now set to launch our production application.

### Laravel Application Setup




