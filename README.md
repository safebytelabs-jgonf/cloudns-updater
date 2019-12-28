# ClouDNS-Updater
ClouDNS updater script is able to be run either as generic Python 3 script or as Docker container. 

### Note: .env file format to Docker variables
When this app runs out of a container it uses an .env file to load config. The file format is as follows:

```
export URL_CDNS = 'https://ipv4.cloudns.net/api/dynamicURL/?q=..........'
export URL_IPIO = 'https://ipinfo.io/json'
export LOG_FILE = 'iplog.txt'
export HOSTNAME = 'put.yourhostname.here'
export SLEEPTIME = 600
```
____Notes:____ 
- In this script [IPInfo](https://ipinfo.io) service for IP resolution is used. The URL used returns a JSON with a key named "ip" with an IP address as value. You can change this service provider transparently without altering the code meanwhile a JSON is returned with a key named "ip", in lower case letters, and an IP address is included as value. 
- In order to get the __URL_CDNS__ for your ClouDNS __HOSTNAME__ you must follow guidance in ClouDNS KB at https://www.cloudns.net/wiki/article/36/. Please note __URL_CDNS__ and __HOSTNAME__ are related. Each __HOSTNAME__ in ClouDNS will have a unique __URL_CDNS__. 
- __SLEEPTIME__ value is seconds, thus, 600 in seconds is 10 minutes. 

## Docker run command
In Docker, the way to export variables is to define them on the ```docker run``` execution, on a execution line similar to this:

```
docker run \
    -d --restart unless-stopped \
    --name cloudns-updater \
    -e URL_CDNS="https://ipv4.cloudns.net/api/dynamicURL/?q=.........." \
    -e URL_IPIO="https://ipinfo.io/json" \
    -e LOG_FILE="iplog.txt" \
    -e HOSTNAME="put.yourhostname.here" \
    -e SLEEPTIME=600 \
    ea1het/cloudns-updater:latest 
```

## Docker build command
```
docker build \
    --build-arg VCS_REF=`git rev-parse \
    --short HEAD` \
    --build-arg BUILD_DATE=`date -u +"%Y-%m-%d"` \
    --build-arg VERSION=v1.0 \
    -t ea1het/cloudns-updater:v1.0 .
```
