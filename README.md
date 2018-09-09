# brmkan

## Test

Test instace is currently running on AWS EC2 instance: https://brmkan.malanius.net/ managed by **malanius**.
Content of this branch reflects that instance and it's settings.

## Useful links

* [wekan-mongo in AWS](https://github.com/wekan/wekan/wiki/AWS)
* [wekan-mongo repository](https://github.com/wekan/wekan-mongodb)
* [example nginx conf](https://github.com/wekan/wekan/wiki/Nginx-Webserver-Config)
* [letsencrypt certbot](https://certbot.eff.org/)
* [test SSL configuratioon](https://www.ssllabs.com/ssltest/index.html)

## Install

1. Install docker-compose.
1. Clone this repo:
    ```bash
    git clone https://github.com/brmlab/brmkan.git
    cd brmkan
    ```
1. Checkout `brmkan-test` branch:
    ```bash
    git checkout brmkan
    ```
1. Generate letsencrypt certificate with certbot.
1. Generate SSL dhparam
    ```bash
    sudo openssl dhparam -out /etc/ssl/dh_param.pem 2048
    ```
1. Copy nginx configuration files.
1. Create a dedicated user for Wekan, for example:
    ```bash
    sudo useradd -d /home/wekan -m -s /bin/bash wekan
    ```
1. Add this user to the docker group:
    ```bash
    sudo usermod -aG docker wekan
    ```
1. Switch login as user wekan:
    ```bash
    sudo su - wekan
    ```
1. Copy docker-compose.yml file to /home/wekan/docker-compose.yml
1. Modify the ROOT_URL and MAIL_* parameters as required.
1. Run in detached mode:
    ```bash
    docker-compose up -d
    ```

## Maitenance

### Check the logs

```bash
docker ps
docker logs CONTAINER-ID-OF-Wekan-or-MongoDB-HERE
```

### Data save/restore

* [Backup all data from MongoDB](https://github.com/wekan/wekan/wiki/Export-Docker-Mongo-Data)
* [Restore MongoDB data](https://github.com/wekan/wekan/wiki/Export-Docker-Mongo-Data)

### Upgrading wekan

1. Always backup the db before upgrading (see above)!
2. In brmkan directory, stop the running services:
    ```bash
    docker-compose stop
    ```
3. Remove wekan container:
    ```bash
    docker-compose rm wekan
    ```
4. Update the wekan image
    ```bash
    docker-compose pull wekan
    ```
5. Start it again:
    ```bash
    docker-compose up -d
    ```

### Upgrading MonmgoDB

1. Always backup the db before upgrading (see above)!
1. In brmkan directory, stop the running services:
    ```bash
    docker-compose stop
    ```
1. Remove wekan container:
    ```bash
    docker-compose rm wekandb
    ```
1. Update the MongoDB image
    ```bash
    docker-compose pull wekandb
    ```
1. Start it again:
    ```bash
    docker-compose up -d
    ```
1. Restore the data from created backup
