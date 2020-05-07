# MyVoice.ai BioCore and Service Layer Setup

Repo containing file to allow the creation of a local MyVoice.ai BioCore instance and associated API Service layer

NB: you need an access token (only provided on approved request by MyVoice.ai) to use the installer contained in this repository

Please ensure that the latest versions of node and npm are installed:

`curl -sL https://deb.nodesource.com/setup_13.x | sudo bash -`

`sudo apt-get install -y nodejs`

## Easy mongo setup

(note that if you prefer a non-docker installation for mongo then instructions are available here:

https://www.digitalocean.com/community/tutorials/how-to-install-mongodb-on-ubuntu-18-04

If this is the case then please move to step _Add root user for database using:_ once mongo is installed.

## Install docker on the server the API Layer and Core will be running

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

# now install the MyVoice.ai BioCore and API Service layer  and ensure that Intel MKL is installed (NB: this also requires the latest version of node & npm to be installed locally, and the script should be run as sudo)

> clone this repo, cd to containing folder once done

> `sudo ./mvSetup.sh {your-access-token}`

> make sure that Intel MKL=libs are installed (note that if you do not have a C compiler installed that you need to install one - this can be achieved using `apt install gcc`:

> `sudo ./install_mkl.sh`

> You can confirm the middleware endpoints are available using:

> `curl localhost:3000`

> If all is running successfully you will see the following returned:

> `MyVoice API`

# Endpoints available (full specification detail available from MyVoice.ai)

`/speakerEnrol`

`/speakerVerify`

`/speakerIdentify`

No specific Authorization is needed in API request headers in this version (please add e.g. myvoice:myvoice)


