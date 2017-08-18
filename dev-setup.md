You may be interested in building a module or want to contribute back to the framework.  
Both  requires that you set up DevLess on your development machine.  
Follow the steps below to setup locally

**Installing Docker: **The steps  for installing Docker varies on OS bases .

```bash
$ sudo apt-get install docker.io
```

For most linux distros the above should work fine. Checkout the [Docker web page](https://docs.docker.com/engine/installation/#supported-platforms ) page for a complete list of all installation options.

Once you have Docker installed \(confirm using `$ sudo docker -v`  \).

Clone the DevLess repo `$git clone https://github.com/devlessteam/dv-php-core`

Now that you have both Docker and the DevLess source code its time to hook them up.

Run  `$ sudo docker run -p <pick_a_port>:80 -d -v /home/eddymens/workarea/devless:/var/www/html eddymens/devless`

Replace `<pick_a_port>` with your desired port number eg:4545.

The above  command will pull in the DevLess Docker image which contains a mysql database and nginx server as well as everything needed to run DevLess. The command also sets up a Docker volume which replaces the DevLess repo within the Docker container with the one your cloned from GitHub.

If everything goes well you should see random alphanumeric appear on the screen. This is the container ID. 

Next we secure a bash terminal within the container. `$sudo docker exec -i -t <container_id>  /bin/bash`  .

Now that you are within the container cd into the `html `  directory. This is where you DevLess source from outside will reside. 

Go ahead and install the DevLess dependencies ie: `composer install` 

Once the dependencies are installed the next step is to set the environment variables. 

Open up the DevLess source in your favorite editor and open the .env file . 

Lets go ahead and connect to the  db within the container.  

Edit the  `.env ` file to look  like below 

```
APP_ENV=local
APP_DEBUG=true
APP_KEY=SomeRandomString

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_DATABASE=devless_production
DB_USERNAME=admin
DB_PASSWORD=pass

CACHE_DRIVER=file
SESSION_DRIVER=file
QUEUE_DRIVER=sync

REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379

MAIL_DRIVER=smtp
MAIL_HOST=mailtrap.io
MAIL_PORT=2525
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null

Next lets get into the container.  
```

With your enviroment set the last thing is to fix permissions 

From you terminal navigate to the DevLess repo you cloned and run `chmod -R 777 . `   



Now you should be able to access DevLess using the port you setup above `127.0.0.1:<pick_a_port>`  

You are ready to make changes to DevLess 



Pulling in the DevLess image  
Getting the source on to your local  
Pulling in Dependencies  
Creating a volumn  
Explaining the folder structure

