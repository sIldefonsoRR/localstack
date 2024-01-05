# Localstack + Terraform
This is the initial setup with examples.

## 1.1 - Install Localstack docker image

$ ``
docker pull localstack/localstack
``

Note: This is the free image. To use the pro version we must use the following command:

$ ``
docker pull localstack/localstack-pro
``
Also, we must create a Localstack account here: https://app.localstack.cloud/sign-up

## 1.2 - Create/change docker-compose.yml
Here is an example. 
``````
version: "3.8"

services:
  localstack:
    image: localstack/localstack:1.3.1
    environment: 
      - AWS_DEFAULT_REGION=eu-west-1
      - AWS_ACCESS_KEY_ID=test
      - AWS_SECRET_ACCESS_KEY=test
      - EDGE_PORT=4566
      - DOCKER_HOST=unix:///var/run/docker.sock
    ports:
      - '4566-4587:4566-4587'
    volumes:
      - localstack-data:/tmp/localstack
      - "/var/run/docker.sock:/var/run/docker.sock"
    networks:
      - dev_to

volumes:
  localstack-data:
    name: localstack-data

networks:
  dev_to:
    name: dev_to
``````
In this example, we will be running an AWS image using the default region "eu-west-1" and the access key + secret = test


## 1.3 - Run the container
$ ``
docker run --rm -it -p 4566:4566 -p 4510-4559:4510-4559 localstack/localstack
``

## 2 - create / activate / update venv
To perform python srip commands,we should create a virtual environment.

$ ``
python3 -m venv .venv
``

$ ``
source .venv/bin/activate
``

$ ``
pip install -r requirements.txt
``


## 2.1 - Install Localstack cli
This is an example for MacOS. Please take a look at the official documentation for other operating systems: https://docs.localstack.cloud/getting-started/installation/

$ ``
brew install localstack/tap/localstack-cli
``


# 3 - Terraform commands
### Start local terraform config commands
$ ``
tflocal init
``
### Show run planning based on the config commands
Save the plan on a file so it can be (re)used

$ ``
tflocal plan -out plan001.plan
``

The plan file is named as **plan001.plan**. This is an example, but it is advisable to use a file with .plan extension.

### Run the planned actions from the file
$ ``
tflocal validate
``

### Run the planned actions from the file
$ ``
tflocal apply "plan001.plan"
``

