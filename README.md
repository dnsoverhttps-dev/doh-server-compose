# doh-server-compose

[English](https://github.com/dnsoverhttps-dev/doh-server-compose/blob/master/README.md)  [Korean](https://github.com/dnsoverhttps-dev/doh-server-compose/blob/master/README.ko.md)

## Require

 - docker
 - docker-compose
 - certbot (works on host)
 - openssl

## License

AGPL-3.0

## package

### Type-1 without DoH-Client

```bash
docker-compose up -d
```

bind9 gets DNS information without going through DoH-Client and gets data quickly. However, in places where DNS pollution such as Korea is implemented, there may be an error in importing DNS records.

### Type-2 with DoH-Client

```bash
docker-compose up -d -f docker-compose-with-client.yml
```

bind9 gets the DNS information through the DoH-Client and receives accurate and uncontaminated information. However, it is a bit slow to receive data because it goes through DoH-Client.

### Type-3 Nginx ReverseProxy Server is not included.

```bash
docker-compose up -d -f docker-compose-without-nginx.yml
```

In the Nginx configuration file, nginx.conf lines 78 ~ 83 and conf.stream.d/dnsovertls.conf and conf.d/dnsoverhttps.conf must be applied to the existing Nginx.

You will also need to connect your existing Nginx server to the dns network and set ip to 10.40.0.40.

## How to set up

### 1. The following files should be set.

- /nginx/conf.d/dnsoverhttps.conf
- /nginx/conf.stream.d/dnsovertls.conf

The above file will be copied back to the respective dnsoverhttps.inc, dnsovertls.inc files and certificate-related settings.

 - /bind9/named.conf.local

The above file is then copied to named.conf.local.inc file, you can set up a zone in the bind9. Please set if necessary. The zone file has `/etc/bind/zones/` in the Docker container.

- /bind9/zones/

The folder above is where the zone configuration files are located. Please set if necessary.

### 2. Please accept the certificate with let's encrypt.

If you receive the certificate for the first time, please accept the certificate in standalone mode. This is compose is running, you can get after the nginx is running, a certificate webroot mode. Please specify /webroot folder as Webroot. More information can be found at: [https://serverfault.com/questions/900960/certbot-renew-certificates-with-autoprovided-webroot](https://serverfault.com/questions/900960/certbot-renew-certificates-with-autoprovided-webroot)

### 3. Create a dhparam file.

openssl is already installed on all Linux distributions. The docker will be automatically updated when you install it.

Generate dhparam for even greater security.

```bash
openssl dhparam -out dhparam.pem 4096
```

It should be placed in the path /dhparam/dhparam.pem and it is recommended to run it with 4096 bits.