# Laravel-Deploymernt
Detailed description on how to deploy laravel application to heroku


# Creating a Laravel application

composer create-project laravel/laravel --prefer-dist hello_laravel_heroku Installing laravel/laravel (v5.1.11)

Installing laravel/laravel (v5.1.11) Downloading: 100%
Created project in hello_laravel_heroku ... ...

cd hello_laravel_heroku

Initializing a Git repository

It’s now time to initialize a Git repository and commit the current state:

git init Initialized empty Git repository in ~/hello_laravel_heroku/.git/

git add .

git commit -m "new laravel project" [master (root-commit) 6ae139d] new laravel project 76 files changed, 5458 insertions(+) ...

# Deploying to Heroku

To deploy your application to Heroku, you must first create a Procfile, which tells Heroku what command to use to launch the web server with the correct settings. After you’ve done that, you’re going to create an application instance on Heroku, configure some Laravel environment variables, and then simply git push to deploy your code! Creating a Procfile

By default, Heroku will launch an Apache web server together with PHP to serve applications from the root directory of the project.

However, your application’s document root is the public/ subdirectory, so you need to create a Procfile that configures the correct document root:

echo web: vendor/bin/heroku-php-apache2 public/ > Procfile

git add .

git commit -m "Procfile for Heroku" [master 1eb2be6] Procfile for Heroku 1 file changed, 1 insertion(+) create mode 100644 Procfile

# Creating a new application on Heroku

To create a new Heroku application that you can push to, use the heroku create command:

heroku create Creating mighty-hamlet-1982... done, stack is cedar-14 http://mighty-hamlet-1982.herokuapp.com/ | git@heroku.com:mighty-hamlet-1982.git Git remote heroku added

As you can see, a random name was automatically chosen for your application (in the example above, it’s mighty-hamlet-1982).

You are now almost ready to deploy the application. Follow the next section to ensure your Laravel app runs with the correct configuration. Declaring a buildpack

Heroku supports a variety of languages, and attempts to detect which one your code is written in when you deploy. This happens in a particular order, and because the Laravel code also contains a package.json file, Heroku will assume your application is written in Node.js unless you explicitly declare it as a PHP application:

heroku buildpacks:set heroku/php Buildpack set. Next release on mighty-hamlet-1982 will use heroku/php. Run git push heroku master to create a new release using this buildpack.

Setting a Laravel encryption key

The application’s encryption key is used by Laravel to encrypt user sessions and other information. Its value will be read from the APP_KEY environment variable.

As it must comply with the rules of the selected cipher in the configuration, the easiest way to generate a valid key is using the php artisan key:generate --show command, which will print a key that you can copy and then paste into the next step.

You can simply set environment variables using the heroku config command, so run a heroku config:set as the last step before deploying your app for the first time:

$ heroku config:set APP_KEY=… Setting config vars and restarting mighty-hamlet-1982... done, v3 APP_KEY: ZEqur46KTEG91iWPhKGY42wtwi3rtkx2

Instead of manually replacing the … placeholder in the command above, you can also run heroku config:set APP_KEY=$(php artisan --no-ansi key:generate --show). Pushing to Heroku

Next, it’s time to deploy to Heroku:

git push heroku master Counting objects: 4, done. Delta compression using up to 4 threads. Compressing objects: 100% (4/4), done. Writing objects: 100% (4/4), 379 bytes | 0 bytes/s, done. Total 4 (delta 3), reused 0 (delta 0) remote: Compressing source files... done. remote: Building source: remote: remote: -----> Fetching custom git buildpack... done remote: -----> PHP app detected remote: -----> Resolved 'composer.lock' requirement for PHP to version 5.6.14. remote: -----> Installing system packages... remote: - PHP 5.6.14 remote: - Apache 2.4.10 remote: - Nginx 1.6.0 remote: -----> Installing PHP extensions... remote: - mbstring (composer.lock; bundled) remote: - zend-opcache (automatic; bundled) remote: -----> Installing dependencies... remote: Composer version 1.0.0-alpha10 2015-04-14 21:18:51 remote: Loading composer repositories with package information remote: Installing dependencies from lock file ... remote: - Installing laravel/framework (v5.1.19) remote: Downloading: 100% remote: remote: Generating optimized autoload files remote: Generating optimized class loader remote: Compiling common classes remote: -----> Preparing runtime environment... remote: -----> Discovering process types remote: Procfile declares types -> web remote: remote: -----> Compressing... done, 74.5MB remote: -----> Launching... done, v5 remote: https://mighty-hamlet-1982.herokuapp.com/ deployed to Heroku remote: remote: Verifying deploy... done. To https://git.heroku.com/mighty-hamlet-1982.git 1eb2be6..1b70999 master -> master

And that’s it! If you now open your browser, either by manually pointing it to the URL heroku create gave you, or by using the Heroku CLI to launch a new browser window, the application will respond.

heroku open Opening mighty-hamlet-1982... done
