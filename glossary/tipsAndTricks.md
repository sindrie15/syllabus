# Tips And Tricks

### Database Migration
To be able to connect to the Postgres database when not running on your local machine (e.g. in docker) you will need to change the ```database.json``` file in your project.

```
{
  "dev": {
    "driver": "pg",
    "user": "postgres",
    "password": "mysecretpassword",
    "host": "localhost",
    "database": "postgres",
    "schema": "postgres",
    "port": "5432",
    "max": 10,
    "idleTimeoutMillis": 30000
  }
}
```

The ```database.json``` file includes environment settings that db-migrate uses when migrating your database (db-migrate is a database migration framework for node). ```database.json``` can include different values depending on whether you are running your application locally on your machine or externally (e.g. on an Amazon server).

Configuration documentation can be found at [db-migrate.readthedocs.io](https://db-migrate.readthedocs.io/en/latest/Getting%20Started/configuration/). There you will find how to configure your file for development (dev) and for production (prod). For this application the 'host' for the production environment would need to be 'postgres' not 'localhost'.

When you run the command   
``npm run migratedb``   
you are actually running   
``db-migrate --config ./server/database.json --migrations-dir ./server/migrations up``   
this script can be found in your main ```package.json``` file.   
When using two configurations in your ```database.json``` file you will need to tell db-migrate which configuration to use (dev or prod). E.g. this command runs db-migrate using the production values   
 ``db-migrate up --config config/database.json -e prod``   
 So one idea would be to have two scripts in your ``package.json`` file, one for migrating your database locally and one externally.

### Docker Compose
The docker-compose file should be setup in such a way that the server depends on the postgres container being up and running, but even if this is done correctly the postgres process inside the container might not have fully initialized so it's important for the server container to make sure the database is initialized before migrating the database schema ("migratedb").

This is a potential fix for this problem:
~~~
#!/bin/bash
set -e

sleep 10
npm run migratedb
node run.js

exit 0
~~~

I.e. use this script as CMD in your Dockerfile instead of ``node run.js``.
NOTE: this is not a great fix so you are encouraged to improve upon it, and remember to comment the script even if you don't change it.

### GIT Tag
The script given as an example for Day 2 has functionality creates a Git tag variable called GIT_COMMIT. I.e. the Git tag for your latest Git commit is exported to the GIT_COMMIT variable in the bash script.
```
if [ -z "$GIT_COMMIT" ]; then
  export GIT_COMMIT=$(git rev-parse HEAD)
  export GIT_URL=$(git config --get remote.origin.url)
fi
```
While the script is running the environment has access to this variable but after the script has finished it is no longer accessible.

This Tag is used to Tag the Docker images you build.
```
docker build -t STUDENTNAME/tictactoe:$GIT_COMMIT .
```
So when ``docker-compose`` wants to run your image it needs access to this variable. As described [here](https://docs.docker.com/compose/env-file/) Docker Compose has access to all environment variables that are stored in a file called ``.env``.
Within the file the variable can be used with the notation ``${VARIABLE_NAME}``.

So for your Docker Compose file to be able to access your GIT_COMMIT tag you need to:   
1. Add to your script commands that create a file called ```.env```   
2. Make your script add the variable to the file  ```GIT_COMMIT=$GIT_COMMIT```
Hint: Your code already creates a text file and places your Git tag into it:   
```
cat > ./dist/githash.txt <<_EOF_
$GIT_COMMIT
_EOF_
```
3. Make your Docker Compose file use the variable ``${GIT_COMMIT}`` when getting your Docker Image.

For those who have removed the Git commit tag from the script when building docker or have written their own scripts, remember to add the tag at the end of your image name, ```STUDENTNAME/tictactoe:$GIT_COMMIT```.


### AWS

You should not interact at all with Github from your AWS instance. You will need to copy all necessary files
from your project. For instance, copy docker-compose files using scp.

```
scp -o StrictHostKeyChecking=no -i "./ec2_instance/${SECURITY_GROUP_NAME}.pem" ./docker-compose.yaml ec2-user@${INSTANCE_PUBLIC_NAME}:~/docker-compose.yaml
scp -o StrictHostKeyChecking=no -i "./ec2_instance/${SECURITY_GROUP_NAME}.pem" ./docker-compose-and-run.sh ec2-user@${INSTANCE_PUBLIC_NAME}:~/docker-compose-and-run.sh
```


Create instance command line looks like this:

```
INSTANCE_ID=$(aws ec2 run-instances  --user-data file://ec2-instance-init.sh --image-id ami-9398d3e0 --security-group-ids ${SECURITY_GROUP_ID} --count 1 --instance-type t2.micro --key-name ${SECURITY_GROUP_NAME} --query 'Instances[0].InstanceId'  --output=text)
echo ${INSTANCE_ID} > ./ec2_instance/instance-id.txt
```

ec2-instance-init.sh is a script that AWS run on the newly started instance. Use this script to initialize
the instance (ec2-instance-init.sh):

You can ask with aws whether an instance has been created:

```
aws ec2 wait --region eu-west-1 instance-running --instance-ids ${INSTANCE_ID}
```
It will not necessarily be accessible through ssh right away.



```
sudo yum -y update
sudo yum -y install docker
sudo pip install docker-compose
sudo pip install backports.ssl_match_hostname --upgrade

sudo service docker start
sudo usermod -a -G docker ec2-user

touch ec2-init-done.markerfile
```

In order to halt script execution until the instance is available, use a loop like this:


```
status='unknown'
while [ ! "${status}" == "ok" ]
do
   echo Checking status of host, currently ${status}
   status=$(ssh -i "./ec2_instance/${SECURITY_GROUP_NAME}.pem"  -o StrictHostKeyChecking=no -o BatchMode=yes -o ConnectTimeout=5 ec2-user@${INSTANCE_PUBLIC_NAME} echo ok 2>&1)
   sleep 2
done
```

Note that this does not check if all installation is complete (docker, docker compose etc). You will
need more checks in order to check that (hint: ec2-init-done.markerfile is part of the solution).


In order to run a script remotely on the instance, use a command like this:

```
ssh -o StrictHostKeyChecking=no -i "./ec2_instance/${SECURITY_GROUP_NAME}.pem" ec2-user@${INSTANCE_PUBLIC_NAME} "cat ~/docker-compose-and-run.sh"
```

