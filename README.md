# SSL


# Step-by-Step: Enable HTTPS with Let’s Encrypt SSL
### (certbot)

```bash
sudo apt update
# Install Certbot
sudo apt install -y certbot python3-certbot-nginx
# Obtain and install the SSL certificate
sudo certbot --nginx -d example.com -d www.example.com
# Auto-renew SSL certificate
sudo systemctl status certbot.timer
or
sudo systemctl enable --now certbot.timer
```


## Add nginx to work with https (force)
```bash
server {
    listen 80;
    server_name example.com www.example.com;

    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl;
    server_name example.com www.example.com;

    ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;
    include ssl-params.conf;

    location / {
        include proxy_params;
        proxy_pass http://unix:/var/www/myflaskapp/myflaskapp.sock;
    }
}
```

# reload the nginx
```bash
sudo nginx -t
sudo systemctl reload nginx
```

