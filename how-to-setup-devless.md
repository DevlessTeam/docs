# INSTALLATION

Currently, there are three main options for setting up DevLess: Heroku, Docker or a traditional setup.


## Heroku Setup

DevLess can be set up on Heroku using a 1-click install. You can try it by clicking [Right Here](https://signup.heroku.com/deploy?redirect-url=https%3A%2F%2Fdashboard.heroku.com%2Fnew%3Ftemplate%3Dhttps%3A%2F%2Fgithub.com%2FDevlessTeam%2FDV-PHP-CORE%2Ftree%2Fheroku3) or by clicking the "Deploy to Heroku" button on [devless.io](http://devless.io/).

If you do not already have a Heroku account, you will need to create one and log in. DevLess can be run in the free tier, so it won't cost you a penny. 

There is a video guide for the Heroku setup [here](https://www.youtube.com/watch?v=f5BwbMNoXGw)


## Docker Setup

A DevLess image is available on the Docker hub as `eddymens/devless`. The image includes a database. DevLess will listen on port 80.

You can either chose to run docker locally or to use [Docker Playground](http://labs.play-with-docker.com/). For a more visual guide, have a look at the [video walk-through](https://www.youtube.com/watch?v=0_82kez2JMk).

### Running Docker locally

1. Install docker. Download from [Docker's homepage](https://www.docker.com/community-edition#/download) or through your package manager.
2. Run DevLess using `docker run -p 4545:80 eddymens/devless`. To make DevLess run in the background, you can add the `d --restart always` flags.
3. Open a browser, and point it to [localhost:4545](http://localhost:4545). 

### Running docker in Docker Playground

1. Go to [http://labs.play-with-docker.com/](http://labs.play-with-docker.com/). Bypass the captcha and click on the "Add new instance" on the left
3. In the Terminal that just popped up, run `docker run -p 4545:80 eddymens/devless`. Wait for the pull to complete.
4. Above the terminal, a link with the numbers **4545** should have appeared. Click this to access your DevLess Instance. 

## Manual Installation

This is **not** a recommended way to run DevLess. DevLess is built on top of PHP and uses a few extensions. The installation process for PHP with extensions is far from trivial. Borrowing the old saying: [Here be dragons. Thou art forewarned.](https://github.com/facebook/hhvm/blob/39415630e84538fb553cd325a85ce837bcf6cb70/src/runtime/ext/ext_imagesprite.cpp#L649)

If you need help, feel free to join the [DevLess Slack](https://slack.devless.io/). Another great resource is the [Laravel installation guide](https://laravel.com/docs/5.1). DevLess is built on top of Laravel. 

### Requirements

We require a set of dependencies on the system level. How to install these depends on your OS. 

* A SQL Database ([MySQL](https://www.mysql.com/), [PostgreSQL](https://www.postgresql.org/) or [MS SQL Server](https://www.microsoft.com/en-us/sql-server/sql-server-2016))
* An HTTP server with PHP support, such as (Apache)[https://httpd.apache.org/) or [Nginx](https://nginx.org/en/).
* PHP 5.6.10 or greater.
* PHP Extensions:
  * [OpenSSL](http://php.net/manual/en/book.openssl.php)
  * [MBstring](http://php.net/manual/en/book.mbstring.php)
  * [Tokenizer](http://php.net/manual/en/book.tokenizer.php)
* [Composer](https://getcomposer.org/download/)

### DevLess Setup

* Clone the repo `git clone`[`https://github.com/DevlessTeam/DV-PHP-CORE.git`](https://github.com/DevlessTeam/DV-PHP-CORE)
* Change into directory `cd DV-PHP-CORE`
* Run `composer install` to grab dependencies. [Installing composer](https://getcomposer.org/download/ "https://getcomposer.org/download/")
* Copy `env.example` to `env` (`$ cp .env.example .env`) and update the database options within the file.
* Run migrations with `./devless migrate`
* Start up DevLess with .`/devless serve`
* Go to [localhost:8000](http://localhost:8000) in your a browser. DevLess should be greeting you. 
