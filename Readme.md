PoC for STF deployment on a single machine
===========
# Installation

* install docker
* install docker-compose
* clone this repo
* install the systemd files [optional]

# Usage
choose an IP your deployment should use, usually that will be the IP of your host.  
choose a secret to be used for inter-service authentication.  
Update the `.env` file accordingly

Run `docker-compose up -d --build`  
Point your browser to the IP you chose,  
login by providing any username and valid e-mail.


A little write-up on this setup:  
https://medium.com/@nikosch86/getting-started-with-automated-in-house-testing-on-android-smartphones-using-stf-dafecee4a8ee  
If you clap it will make me happy :)


# Addendum 
Additional work was done in the original code to facilitate a greater degree of configuration to the docker-compose.yml. More specifically,
the following configuration environment variables were added:
- OPENSTF_VERSION - the tag of the devicefarmer/stf docker image to use
- ADB_VERSION - the tag of the sorccu/adb docker image to use
- ADB_KEY_PATH - the path to the adb keys
- RETHINKDB_VERSION - the tag of the rethinkdb docker image to use
- RETHINKDB_DATA_PATH - the path to the `rethinkdb_data` that contains the rethinkdb data 
- HTTP_PORT - the HTTP port to bind the http server to
- PROVIDER_MIN_PORT - the provider starting port range that will be exposed
- PROVIDER_MAX_PORT - the provider ending port range that will be exposed

**WARNING** The variables used for configuring the individual docker image versions, should be docker tags and not docker image ids. 

## Systemd integration
Copy the file `systemd/docker-compose@.service` to your `/etc/systemd/system` folder. Note that the docker-compose installation used
for development was performed according to the instructions found [in the official Docker documentation](https://docs.docker.com/compose/install/). 
As such, the systemd service, assumes that the docker-compose binary is located in `/usr/local/bin/docker-compose` . You should update the systemd file, 
if your installation differs. Furthermore, the systemd file was created according the discussion found [in this issue](https://github.com/docker/compose/issues/4266) 
and can be used to run other docker-compose based services (it is trivial to modify it to work for this docker-compose installation only). You should 
also run `systemctl daemon-reload` at this point.

Create the folder /etc/docker/compose/stf-app and copy the following files/folders:
- the docker-compose.yml file
- the .env file (change it accordingly)
- the nginx folder
- the storage-temp folder



You should be able to control the service with the following commands:
`systemctl [start|stop|restart|status] docker-compose@stf-app`
