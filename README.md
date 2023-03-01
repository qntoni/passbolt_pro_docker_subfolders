# Passbolt EE on Docker with a NGINX Reverse-Proxy

This is a Docker Compose configuration file for running two instances of Passbolt Pro with separate databases using a single Nginx front-end as a reverse proxy.

Prerequisites
- Docker
- Docker Compose

## Usage

- Clone this repository.
- Fill the file named subscription_key.txt in the root directory and replace the placeholder with the copy of your subscription key into it.
- Open a terminal in the directory containing the docker-compose.yml file.
- Run docker-compose up -d to start the containers in the background.
- Access the Passbolt instances at https://passbolt.local:443/docker and https://passbolt.local:4443/k8s.

## Configuration

### Nginx

The Nginx container listens on ports 8080 and 4433 on the host and proxies requests to the Passbolt containers on ports 80 and 443 respectively. The configuration for Nginx is located in ./nginx/core.conf.d/proxy.conf and ./nginx/nginx.conf.

### Passbolt

There are two Passbolt containers, passbolt and passbolt2, each with their own database defined in the db container. The containers are configured with the following environment variables:

- APP_FULL_BASE_URL: The full URL for the Passbolt instance, including the protocol and port number.
- APP_BASE: The base path for the Passbolt instance.
- DATASOURCES_DEFAULT_HOST: The hostname of the database server.
- DATASOURCES_DEFAULT_USERNAME: The username for connecting to the database.
- DATASOURCES_DEFAULT_PASSWORD: The password for connecting to the database.
- DATASOURCES_DEFAULT_DATABASE: The name of the database.

The command section of each container defines a startup script that waits for the db container to be available before running the Passbolt entry point script.

### Volumes

The following volumes are defined:

- database_volume: The volume used by the db container to store its data.
- gpg_volume and gpg_volume2: The volumes used by the Passbolt containers to store GPG keys.
- jwt_volume and jwt_volume2: The volumes used by the Passbolt containers to store JWT authentication keys.

## License

Please note that this docker-compose configuration is provided as a reference for using Passbolt EE and is subject to the Passbolt EE license. The software and all materials contained in this repository are reserved to Passbolt SA.

For more information about Passbolt EE licensing, please refer to the Passbolt Pro License Agreement at https://www.passbolt.com/pro/licenses.