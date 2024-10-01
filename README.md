# Kong gateway
A docker compose file for Kong API gateway open-source


## Bootstrap the image
If you want to create a local Kong API Gateway you can use the docker compose file in this repository. When running for the first time, you will need to bootstrap the database. This can be done using the kong-migrations container in the compose file, but you will need to uncomment it first. It is also wise to comment out the Kong gateway, since it cannot run before the database is bootstrapped. When migrations is uncommented and Kong gateway is commented out run ```docker compose up```

If you are experiencing issues with the database being a bit slow and kong migrations already shutting down because it cannot find the database, please consider opening an additional terminal and runnning ```docker compose up ``` again, this will allow kong-migrations to bootstrap the database. You can verify that the database is running by logging into the database and checking that all relations have been created. Do this by running: ```docker exec -it $(docker ps -aqf "name=postgres") /bin/bash ``` (Not tested) or in Powershell ```docker exec -it $(docker ps --filter "name=postgres" -q) /bin/bash```

When logged into the container run ```psql -U kong_user kong``` and then ```\dt``` to verify realtions have been created in the database (you can see the tables here).

## Running the image
After bootstrap has been completed, comment out the kong-migrations service and enable Kong Gateway. Kong can now start by running ```docker compose up``` again.

Once the gateway is running, you can visit Kong manager via your localhost on: http://localhost:8001. Kong manager is the UI interface that enables you to configure the gateway with all services, routes, plugins and other components you need to manage Kong.


## Getting started with decK
An important concept when managing API GW is the everything-as-code principle. Kong also has the capability to manage it's config as code using a tool called decK. decK can export and import configurations of Kong.

Attached in the deck_container folder is a Dockerfile you can use to build a container that has deck (amongst other things). ```CD``` into the folder and run ```docker build -t deck:1.0 .```

The container will build and that run it via the docker compose file attached. 

Once the container is running you can exec into it: ```docker exec -it $(docker ps -aqf "name=deck") /bin/bash``` once in the container, you can connect via the service names. E.g. to try ping the gateway (on the admin API) using: ```deck gateway ping --kong-addr http://kong:8010```

Once connectivity is clear, you can start trying to getting the gateway config (deck dump) and edit it and synch it again (deck sync).