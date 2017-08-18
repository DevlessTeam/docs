You may be interested in building a module or want to contribute back to the framework.  
Both  requires that you set up DevLess on your development machine.  
Follow the steps below to setup locally

**Installing Docker: **The steps  for installing Docker varies on OS bases .

```bash
$ sudo apt-get install docker.io
```

For most linux distros the above should work fine. Checkout the [Docker web page](https://docs.docker.com/engine/installation/#supported-platforms ) page for a complete list of all installation options.

Once you have Docker installed \(confirm using `$ docker -v`  \).

Clone the DevLess repo `$git clone https://github.com/devlessteam/dv-php-core ` 

Now that you have both Docker and the DevLess source code its time to hook them up.

Run  `$docker run -p <pick_a_port>:80 -d -v /home/eddymens/workarea/devless:/var/www/html eddymens/devless` 

Replace `<pick_a_port>` with your desired port number eg:4545.

The above  command will pull in the DevLess Docker image which contains a mysql database and nginx server as well as everything needed to run DevLess. The command also sets up a Docker volume which replaces the DevLess repo within the Docker container with the one your cloned from GitHub.

Next  pull in the DevLess image using `$ docker pull eddymens/devless`  .

Now run the

Pulling in the DevLess image  
Getting the source on to your local  
Pulling in Dependencies  
Creating a volumn  
Explaining the folder structure

