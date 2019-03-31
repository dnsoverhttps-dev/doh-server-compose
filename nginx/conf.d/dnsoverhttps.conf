server {
	listen 80;
	listen [::]:80;

	server_name __require_change_domain;

	location / {
		proxy_pass http://10.40.0.10:8080;
		proxy_http_version 1.1;
		proxy_set_header Upgrade $http_upgrade;
		proxy_set_header Connection 'upgrade';
		proxy_set_header Host $host;
		proxy_set_header X-Real-IP $remote_addr;
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		proxy_set_header X-Forwarded-Proto $scheme;
		proxy_set_header X-Scheme $scheme;
		proxy_cache_bypass $http_upgrade;
	}
	
	location /.well-known/acme-challenge {
		default_type "text/plain";
		root /var/www/webroot;
	}
  
	listen 443 ssl http2;
	listen [::]:443 ssl http2;

	# Certificate(s) and private key
	ssl_certificate /etc/letsencrypt/live/__require_change_domain/fullchain.pem;
	ssl_certificate_key /etc/letsencrypt/live/__require_change_domain/privkey.pem;
	ssl_trusted_certificate /etc/letsencrypt/live/__require_change_domain/fullchain.pem;

	ssl_dhparam /etc/dhparam/dhparam.pem;

	ssl_protocols TLSv1.3 TLSv1.2;
	ssl_prefer_server_ciphers on;
	ssl_ciphers 'TLS13+AESGCM+AES128:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA:ECDHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA256:DHE-RSA-AES256-SHA:ECDHE-ECDSA-DES-CBC3-SHA:ECDHE-RSA-DES-CBC3-SHA:EDH-RSA-DES-CBC3-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:DES-CBC3-SHA:!DSS';
	
	ssl_session_cache shared:TLS:2m;
	ssl_session_timeout 1d;
	ssl_session_tickets off;
	ssl_ecdh_curve X25519:sect571r1:secp521r1:secp384r1;

	# OCSP stapling
	ssl_stapling on;
	ssl_stapling_verify on;
	resolver 8.8.8.8 8.8.4.4 [2001:4860:4860::8888] [2001:4860:4860::8844];

	# Set HSTS to 365 days
	add_header Strict-Transport-Security 'max-age=31536000; includeSubDomains';
	
	if ($scheme = http) {
		return 302 https://$server_name$request_uri;
	}
}
