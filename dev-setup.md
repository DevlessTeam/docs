# Local Dev Environment

You may be interested in building a module or want to contribute back to the framework.  Both requires that you set up DevLess on your development machine.  

This guide is based on Docker. **Install Docker** if you don't have it. Download from [Docker's homepage](https://www.docker.com/community-edition#/download) or through your package manager. 

## Start local docker image

We start with opening one terminal and starting up the docker image:

```bash
# Clone DevLess git repo
git clone https://github.com/DevlessTeam/DV-PHP-CORE

# Change PWD
cd DV-PHP-CORE

# Run docker. Add -d if you prefer running detached. 
docker run -p 4545:80 -v "$PWD:/var/www/html" eddymens/devless
```
If you ran without the `-d` option, this terminal will now block, with DevLess running. To shut down DevLess, press ctrl+c. 

We have now pulled the DevLess Docker image. It contains all system dependencies for DevLess, including MySQL and an nginx server. When starting docker, we also specify that we want the DevLess source within the container to point to our checked-out directory outside of the container. 

## Set up php dependencies & fix permissions

For this we need to know the container id of the container we just started. Run `docker ps`, and look for the DevLess container.

```bash
# Find the docker image id
DOCKER_IMG=$(docker ps | awk '$2=="eddymens/devless" {print $1}')

# Install DevLess dependencies & fix permissions
docker exec -it $DEVLESS_IMG  bash -c "cd html ; composer install ; chmod -R a+rw ."
```

Above, we install the DevLess dependencies using composer. Then we change file permissions, to allow everyone to read & write to the source directory. This is needed so that both the docker service and your user outside the container can edit files. 

## Modify the database settings

The docker image comes with a MySQL database, while the default `.env` file assumes Postgres. Update database section the .env file to:

```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_DATABASE=devless_production
DB_USERNAME=admin
DB_PASSWORD=pass
```

## View & Modify DevLess

You should now be able to go to [localhost:4545](http://localhost:4545) in a browser, and see DevLess running. Any changes in the source files should be visible immediately if you refresh the browser.

Happy hacking!

