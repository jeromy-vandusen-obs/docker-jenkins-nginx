# Docker Containers: Jenkins with Nginx Reverse Proxy and SSL

This sets up two docker containers, one running Jenkins on the default port 8080, and the other running
Nginx as a reverse proxy to Jenkins with a self-signed certificate, and an automatic redirect to enforce
HTTPs, using the domain name `jenkins.local`.

## Customization

To change the domain that Jenkins will respond to (example below uses `mydomain.com`):

```console
$ cd vol/nginx
$ export DOMAIN=mydomain.com
$ envsubst \$DOMAIN < default.conf.template > default.conf
```

## Setup

Generate the self-signed certificate with:

```console
$ cd vol/nginx/ssl
$ ./create-certificate.sh
```

To use an existing certificate instead, simply copy the certificate and key files to the `vol/nginx/ssl`
directory, and ensure that they are named `cert.pem` (certificate file) and `key.pem` (key file).

## Usage

Start the containers with:

```console
$ docker-compose up -d
```

Stop the containers with:
```console
$ docker-compose down
```

Access Jenkins while the containers are running by navigating to `https://jenkins.local` using any web browser.
You will also need to create an entry in your local hosts file to map `jenkins.local` to `localhost` (or
`192.168.99.100` if you're using Docker Toolbox). If you are using a your own domain name instead (as described
in the Customization section above), then use that URL.

When Jenkins first initializes, it generates a random password that is required to log in the first time. The
password can be obtained with:

```console
$ cat vol/jenkins/home/secrets/initialAdminPassword
```
