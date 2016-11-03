## #Installation

- [Requirements](#requirements)
- [installation](#installation-procedure)
- [One Click Install](#one-click-install)

<a name="requirements"></a>
## Requirements
* Database (mysql, postgres, sqlsrv)
* An HTTP server
* PHP 5.5.9 <> 5.6.10
* OpenSSL PHP Extension
* PDO PHP Extension
* Mbstring PHP Extension
* Tokenizer PHP Extension
* Composer

<a name="installation-procedure"></a>
## Installation procedure
* Clone the repo (git clone https://github.com/DevlessTeam/DV-PHP-CORE.git)
* cd /DV-PHP-CORE
* run ``composer install`` to grab dependecies. (Installing Composer](https://getcomposer.org/download/)
* copy ``.env.example`` to ``.env`` and update the database options within the file
* run migrations with php artisan migrate
* `` ./devless serve``

If everything goes on smoothly you should be able to access the setup screen at ``localhost:8000``

If you will need extra help setting up you may check out the laravel [installation](https://laravel.com/docs/5.1) guide as the DevLess core is based of laravel.

<a name="one-click-install"></a>
## One click Install
You  may also choose to run our  [Heroku click to deploy](https://heroku.com/deploy?template=https://github.com/DevlessTeam/DV-PHP-CORE/tree/heroku) option
