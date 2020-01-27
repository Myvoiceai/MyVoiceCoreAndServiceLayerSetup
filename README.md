# MyVoice.ai BioCore and Service Layer Setup

Repo containing file to allow the creation of a local MyVoice.ai BioCore instance and associated API Service layer

NB: you need an access token (only provided on approved request by MyVoice.ai) to use the installer contained in this repository

Please ensure that the latest versions of node and npm are installed.

Please also install git lfs (on Ubuntu `apt-get install -y git-lfs` )

## Easy mongo setup

## install docker on the server the API Layer and Core will be running

> `sudo curl -fsSL get.docker.com -o get-docker.sh && sh get-docker.sh`

- **Create a mongo container using base Docker mongo image**

_NB: 27017 is the port that you (and the middleware) will use to connect to the mongo docker container, in a full production version this is configurable - as are the database connection credentials

> `docker run --name mongo -v "$PWD"/mongo/data:/data/db -p 27017:27017 -d mongo`

- **Once this is complete, connect to the running mongo container:**

> `docker run -it --link mongo:mongo --rm mongo sh -c 'exec mongo "mongo:27017/test"'`

- **When connected to the container, create and connect to the the database using**

> `use mvdb`

- **Add root user for database using:**

> `db.createUser({user: "mvadmin",pwd: "mvadmin",roles: [{ role: "root", db: "admin" }]})`

- **Exit the mongo container using** 

> `exit`

# now install the MyVoice.ai BioCore and API Service layer (NB: this also requires the latest version of node & npm to be installed locally)

> `./mvSetup {your-access-token}

> You can confirm the middleware endpoints are available using:

> `curl localhost:3000`

> If all is running successfully you will see the following returned:

> `MyVoice API`

# Endpoints available

`/speakerEnrol`

`/speakerVerify`

`/speakerIdentify`

No Authorization is needed in API request headers in this version; full specification detail available following approved request from MyVoice.ai


