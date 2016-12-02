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
