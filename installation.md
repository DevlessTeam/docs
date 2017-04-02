## #Installation

- [Requirements](#requirements)
- [One Click Heroku Install](#one-click-install)
- [Docker Procedure](#docker-procedure)
- [Manual](#installation-procedure)




<a name="one-click-install"></a>
## One click Install
You  may also choose to run our  [Heroku click to deploy](https://heroku.com/deploy?template=https://github.com/DevlessTeam/DV-PHP-CORE/tree/heroku2) option


<a name="docker-procedure"></a>
## Docker procedure
* You will need [Docker](https://docs.docker.com/engine/installation/) installed 
* Run the following command on your terminal or command prompt `docker run -p 4545:80 -d --restart always eddymens/devless`.
* You can now visit `http://localhost:8000:4545` in your browser of choice.  
* You may change the port `4545` to whichever one you prefer provided its available. 


<a name="installation-procedure"></a>

<a name="requirements"></a>
## Requirements
* Database (MySQL, PostgreSQL, SQLSRV)
* An HTTP server
* PHP 5.6.10 or greater
* OpenSSL PHP Extension
* PDO PHP Extension
* Mbstring PHP Extension
* Tokenizer PHP Extension
* Composer

## Local procedure
* Clone the repo `git clone https://github.com/DevlessTeam/DV-PHP-CORE.git`
* Change into directory `cd DV-PHP-CORE`
* run `composer install` to grab dependecies. (Installing Composer](https://getcomposer.org/download/)
* copy *.env.example* to *.env* `cp .env.example .env` and update the database options within the file
* run migrations with `./devless migrate`
* `./devless serve`

If everything goes smoothly you should be able to access the setup screen at [localhost:8000](http://localhost:8000)

If you will need extra help setting up you may check out the [Laravel installation](https://laravel.com/docs/5.1) guide as the DevLess core is based of Laravel.
