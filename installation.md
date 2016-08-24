## #NAV

- [Requirements](#requirements)
- [installation](#installation-procedure)
- [Local Install](#local-install)

<a name="requirements"></a>
## Requirements
* Database (mysql, postgres, sqlsrv etc..)
* An HTTP server
* PHP >= 5.5.9
* OpenSSL PHP Extension
* PDO PHP Extension
* Mbstring PHP Extension
* Tokenizer PHP Extension
* Composer

<a name="installation-procedure"></a>
## Installation procedure
* Clone the repo (git clone https://github.com/DevlessTeam/DV-PHP-CORE.git)
* cd ../DV-PHP-CORE
* run ``composer install`` to grab dependecies. (Installing Composer](https://getcomposer.org/download/)
* copy .env.example to .env and update the database options
* run migrations with php artisan migrate
* `` php artisan serve``

If everything goes on smoothly you should be able to access the setup screen at localhost:8000

If you will need extra help setting up you may check out the laravel [installation](https://laravel.com/docs/5.1) guide as the Devless core is based of laravel.

<a name="local-install"></a>
## Local Installs
You  may also choose to run one of our [local installers](https://devless.io/#!/get-started). Supported OS include Windows, OS X, Ubuntu.
