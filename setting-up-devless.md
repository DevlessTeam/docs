# Setting up DevLess

## INSTALLATION

* [Requirements](setting-up-devless.md#requirements)
* [One Click Heroku Install](setting-up-devless.md#one-click-install)
* [Docker Procedure](setting-up-devless.md#docker-procedure)
* [Local Installation](setting-up-devless.md#local-procedure)

## **ONE CLICK INSTALL**



* First, you will need an [Heroku account.](https://www.heroku.com/)
* Next, make sure you are [logged in to Heroku.](https://id.heroku.com/login)
* Click =&gt; Heroku [click to deploy.](https://signup.heroku.com/deploy?redirect-url=https%3A%2F%2Fdashboard.heroku.com%2Fnew%3Ftemplate%3Dhttps%3A%2F%2Fgithub.com%2FDevlessTeam%2FDV-PHP-CORE%2Ftree%2Fheroku3)

What will we do without Heroku?

## **DOCKER PROCEDURE**



* You will need [Docker](https://docs.docker.com/engine/installation) installed.
* Run the following command on your terminal or command prompt,`docker run -p 4545:80 -d --restart always eddymens/devless`.
* You can visit [`http://localhost:4545`](http://localhost:4545) in your browser of choice.
* You may change the port '4545' to whichever one you prefer provided its available.

**Quick Try Out Option**

**NB: **In case you still have troubles installing Docker on your machine \(which is something that happens often for linux users\), you may try DevLess out on [Play with Docker.](http://labs.play-with-docker.com/)

Docker is already installed, all you need to do is:

* First bypass the `I'm not a robot captcha`🙄
* Next, click on `+ ADD NEW INSTANCE` to gain access to a terminal with Docker already installed.
* Now, run the following command on the terminal; \`docker run -p 4545:80 -d --restart always eddymens/devless\`.
* You can now click on `4545` up at the top to open up DevLess.

By the way, `Play with Docker` provides you up to 4 hours to play around.

Don't you love [Play with Docker](http://labs.play-with-docker.com/) already?

## **Installation Procedure**

I will be honest and tell you the truth. This procedure is a pain😤.The last time I did a manual install was when I committed code for DevLess. Please, just use Docker or Heroku, only use manual install if you know your way around it. Please holla in case you've got this manual installation thing down. [DevLess Slack](https://slack.devless.io/). Look for `@eddymens`

## **REQUIREMENTS**

* Database \(MySQL, PostgreSQL, SQLSRV\)
* An HTTP server
* PHP 5.6.10 or greater
* OpenSSL PHP Extension
* MBstring PHP Extension
* Tokenizer PHP Extension
* Composer

## **LOCAL PROCEDURE**

* Clone the repo `git clone`[`https://github.com/DevlessTeam/DV-PHP-CORE.git`](https://github.com/DevlessTeam/DV-PHP-CORE)
* Change into directory `cd DV-PHP-CORE`
* Run `composer install` to grab dependencies. [Installing composer](https://getcomposer.org/download/)
* Copy
* .env.example\* to
* .env\* `cp .env.example .env` and update the database options within the file
* run migrations with `./devless migrate`
* .`/devless serve`

If everything goes smoothly you should be able to access the setup screen at [localhost:8000](http://localhost:8000)

If you will need extra help setting up you may check out the [Laravel installation](https://laravel.com/docs/5.1) guide as the DevLess core is based on Laravel.

