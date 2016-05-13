#Set up Local DNS Server Using Docker-compose on OSX
This repo introduce a way to setup a DNS server inside docker-compose for other containers
in the compose to use. The third-party docker image sameersbn/bind:latest uses
DNS software "bind".

##Install docker-engine and docker-compose
Check official documentation about set up [docker](https://docs.docker.com/engine/installation/) 
and [docker-compose](https://docs.docker.com/compose/install/).

Please note that we need version feature of docker-compose. Therefore, we need Compose 1.6.0+ and require a Docker Engine of version 1.10.0+. 
Details can be found [here](https://docs.docker.com/compose/compose-file/#version-2).
##Pull docker-bind Image
```
docker pull sameersbn/bind:latest
```
Documentation of this image can be find [here](https://github.com/sameersbn/docker-bind)

##Steps to Setup
###Step 1:
Create an empty bind-data directory.
```
$ mkdir bind-data
```
###Step 2:
Run the docker-compose in background. You can check the status and logs of bind container.
```
$ docker-compose up -d
$ docker-compose logs bind
$ docker-compose ps
```
###Step 3:
Get docker-machine IP by
```
$ docker-machine ip
```
Open a browser and go to https://{docker-machine-ip}:10000. 

Default username is "root" and 
password is "password". Go to bindServer, then you can add zones here. Zone file will be saved in the bind-data 
directroy you just created. Therefore, if you launch the DNS server next time, the zones will not disappear.

###Step 4:
Check whether the DNS server is working in another container. I add a test zone file contains "test.com". If the result is 123.123.123.123, it works.
```
$ docker-compose run --rm test bash
# apt-get update
# apt-get install dnsutils
# host test.com
```
Install dnsutils, because clean ubuntu image does not have dig, host command. You can use any other images you want.

##Where to Put Zone Files
You should put them inside ./bind-data/lib/. In addition, please update named.conf.local in ./bind-data/etc/. You can refer to the existing example in this repo.

However, I suggest you create zone files using webmin.
