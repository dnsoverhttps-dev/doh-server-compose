# doh-server-compose

[English](https://github.com/dnsoverhttps-dev/doh-server-compose/blob/master/README.md)  [Korean](https://github.com/dnsoverhttps-dev/doh-server-compose/blob/master/README.ko.md)

## 필요 프로그램

 - docker
 - docker-compose
 - certbot (works on host)
 - openssl

## 라이선스

AGPL-3.0

## 패키지 타입

### Type-1 DoH-Client가 제외된 형태

```bash
docker-compose up -d
```

bind9는 DoH-Client를 거치지 않고 DNS 정보를 받아오며, 빠르게 데이터를 받아옵니다. 하지만 한국과 같은 DNS 오염이 시행되는 곳에서는 DNS 레코드를 가져오는 데 있어 에러가 생길 수 있습니다.

### Type-2 DoH-Client가 포함된 형태

```bash
docker-compose up -f docker-compose-with-client.yml -d 
```

bind9는 DoH-Client를 거치고 DNS 정보를 받아오며, 정확하고 오염되지 않은 정보를 받아옵니다. 하지만 DoH-Client를 거치므로 데이터를 받아오는 데 조금 느립니다.

### Type-3 Nginx ReverseProxy Server가 비포함된 형태

```bash
docker-compose up -f docker-compose-without-nginx.yml -d 
```

Nginx가 이미 설정되어 있는 환경에서 사용하세요. Nginx 설정파일 중 nginx.conf 78~83 라인 및 conf.stream.d/dnsovertls.conf, conf.d/dnsoverhttps.conf 파일을 기존의 Nginx에 적용해야 합니다.

또 기존의 Nginx 서버를 dns network에 연결한 뒤, ip를 10.40.0.40 로 설정해야 합니다.

## 설정 방법

### 1. 다음과 같은 파일을 설정하여야 합니다.

- /nginx/conf.d/dnsoverhttps.conf
- /nginx/conf.stream.d/dnsovertls.conf

위의 파일은 각각 dnsoverhttps.inc, dnsovertls.inc 파일을 복사한 뒤, 인증서 관련 설정을 하여야 합니다.

 - /bind9/named.conf.local

위의 파일은 named.conf.local.inc 파일을 복사한 뒤, bind9의 zone 설정을 할 수 있습니다. 필요한 경우 설정하시기 바랍니다. zone 파일은 Docker container 안에서 `/etc/bind/zones/` 를 가집니다.

- /bind9/zones/

위의 폴더는 zone 설정 파일들이 위치합니다. 필요한 경우 설정하시기 바랍니다.

### 2. let's encrypt로 인증서를 받아주세요.

최초로 인증서를 받을 경우에는 standalone 모드로 인증서를 받아주세요. 본 compose가 실행되어 nginx가 가동된 이후, webroot 모드로 인증서를 받을 수 있습니다. /webroot 폴더를 Webroot로 지정하시기 바랍니다. 자세한 사항은 다음에서 볼 수 있습니다. [https://serverfault.com/questions/900960/certbot-renew-certificates-with-autoprovided-webroot](https://serverfault.com/questions/900960/certbot-renew-certificates-with-autoprovided-webroot)

### 3. dhparam 파일을 생성해주세요.

openssl은 모든 리눅스 배포판에 이미 설치되어 있습니다. docker 설치시 자동으로 업데이트도 될 것입니다.

dhparam을 생성하여 보안을 더욱 더 높여줍니다.

```bash
openssl dhparam -out dhparam.pem 4096
```

/dhparam/dhparam.pem 경로에 위치하여야 하며, 4096비트로 실행하는 것을 권장합니다.