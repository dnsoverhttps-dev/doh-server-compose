stream {
	upstream dns-servers {
        server 10.40.0.30:53;
    }

	server {
        listen 853 ssl;
	    listen [::]:853 ssl;
        
        proxy_pass dns-servers;

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
    }
}
