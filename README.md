# An Internal or Private Container Registry Stack using Docker-Compose

This repository will help you to starting-up an internal or private container registry on your own server. This Container Registry Stack is built-in with Redis as Memory Blob Cache, Container Registry UI, and HAProxy as SNI and TLS termination.

## Getting Started

These instructions will get you this repository can be used in your server.

### Prerequisites

Prequisites Packages:
* Docker (Application Containerization)
* Docker-Compose (Docker Orchestration)

### Using This Package

Below is the instructions to use this package:
* Install Docker and Docker-Compose CE (Please Read The Official Guideline in [here](https://docs.docker.com))
* Install `htpasswd` tool to generate authentication
```sh
# On Debian/Ubuntu
sudo apt-get install -y apache2-utils

# On Red Hat/CentOS
sudo yum install -y httpd-tools
```
* Clone this repository
```sh
# Asumming Git already installed
git clone -b master https://github.com/dimaskiddo/docker-registry-stack.git
cd docker-registry-stack
```
* Generate authentication file
```sh
# HTPasswd authentication need to use Bcrypt mode
# Repalce <username> with your 'username' that will be used for the auth
htpasswd -B registry/auth/htpasswd <username>
```
* Prepare your SSL/TLS certificate
```sh
# Put your SSL/TLS certificate in ./haproxy/certs with format .crt and .key
# This will be used by the Container Registry
vi haproxy/certs/example_com.crt
vi haproxy/certs/example_com.key

# Generate all-in-one PEM certificate
# This will be used by the HAProxy
cat haproxy/certs/example_com.crt haproxy/certs/example_com.key > haproxy/certs/example_com.pem
```
* Configure your docker-compose.yml
```sh
# Configure the registry service environment variable section (line 25, 26, 43, 45)
# In this case related to the SSL/TLS certficate file name (if you are using different name rather than example_com.*)
# Also any Domain FQDN used need to be changed
vi docker-compose.yml
```
* Configure your haproxy/haproxy.cfg
```sh
# Configure the SSL/TLS termination and the Hostname for Registry and the Registry UI (line 36, 41, 42)
# In this case related to the SSL/TLS certficate file name (if you are using different name rather than example_com.*)
# Also any Domain FQDN used need to be changed
vi haproxy/haproxy.cfg
```
* Start the Container Registry Stack
```sh
docker-compose up -d
```
* Check the Container Registry Stack
```sh
docker-compose ps
docker-compose logs
```
* To Stop the Container Registry Stack
```sh
docker-compose down
```

## Built With

* [Docker](https://www.docker.com/) - Application Containerization

## Authors

* **Dimas Restu Hidayanto** - *Initial Work* - [DimasKiddo](https://github.com/dimaskiddo)

See also the list of [contributors](https://github.com/dimaskiddo/docker-registry-stack/contributors) who participated in this project
