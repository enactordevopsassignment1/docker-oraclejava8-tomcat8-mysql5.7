## Usage

To create the image `your-login/your-new-image`, execute the following command on the root folder where the Dockerfile lies:

```shell
docker build -t your-login/your-new-image .
```

You can then push your new image to the Docker servers (A.K.A. registry):

```shell
docker push your-login/your-new-image
```

## Running your tomcat-mysql docker image

Start your image binding the external ports 8080:

```shell
docker run --name TomcatServerOrWhatever -d -p 8080:8080 your-login/your-new-image
```

If you want to expose also the mysql, do:

```shell
docker run --name TomcatServerOrWhatever -d -p 8080:8080 -p 3306:3306 your-login/your-new-image
```

War file that need to be deployed should be given as a build argument. 
That file should reside in the same directory as the Dockerfile. 
Here it will be added/copied to TOMCAT_HOME/webapps directory at the build.
```shell
 docker build --build-arg web_app_war_file=simple_web_app.war -t simple_web_app_container .
```

Table creation of 'simple_db' is done at the mysql-setup.sh.
The table creation 'simple_db.sql' script should be there in the directory as the Dockerfile at the build phase, 
so that it get added/copied into the container. 


(this will overwrite the tomcat manager, so use it wisely)

That's all folks!

## Characteristics of this image

 - Ubuntu 16.04
 - Oracle Java 8
 - MySQL 5.7
 - Tomcat 8.0.50
 - Run with jdpa for debug

## Connecting to the bundled MySQL server from outside the container

The first time that you run the container, a new user `defaultUser` with all privileges
will be created in MySQL with a default password. To get the password, check the logs
of the container by running:

```shell
docker logs $CONTAINER_ID
```

You will see an output like the following:

```shell
========================================================================
You can now connect to this MySQL Server using:

    mysql -udefaultUser -p47nnf4FweaKu -h<host> -P<port>

Please remember to change the above password as soon as possible!
MySQL user 'root' has no password but only allows local connections
========================================================================
```
