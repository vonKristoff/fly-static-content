Steps done to manage the SSL Certificate via Certbot and Nginx

### Prerequisites:

1. **Domain Name**: Ensure your domain name (e.g., example.com) is correctly pointed to the server where you will run Certbot.
2. **Web Server**: Certbot typically works with Apache or Nginx, so make sure your web server is installed and configured correctly. In our case, we choose Nginx.

### Installments
1.  **Install Certbot:**
       - Certbot provides specific instructions for various operating systems and web servers. Visit the Certbot website or use your package manager to install it.
2. **Obtain the Certificate:**
	**Nginx:**    
```shell
sudo certbot --nginx
```
	    
	    Similarly, follow the prompts to select the domain(s) and Certbot will handle Nginx configuration.

### Manage
- After running `sudo certbot --nginx`, Certbot will show you the list of domains it detected in your nginx configuration that you can secure with HTTPS.
- If `web.page` is the domain you want to secure, simply press `Enter` or type `1` and then `Enter` to select it.
- Certbot will then proceed to obtain and install the SSL certificate for the selected domain(s) and modify your nginx configuration according to your needs.

To edit your nginx you can use:
```shell
sudo nano /etc/nginx/sites-available/default
```

It should be something like this:
server {
    listen 80;
    server_name backend.foopy.xyz;

    location / {
        proxy_pass http://<backend-server-ip>:<backend-server-port>;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}

server {
    listen 443 ssl;
    server_name backend.foopy.xyz;

    ssl_certificate /etc/letsencrypt/live/backend.foopy.xyz/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/backend.foopy.xyz/privkey.pem;

    location / {
        proxy_pass http://<backend-server-ip>:<backend-server-port>;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
Obviously, replace with the correct names.

Once done, verify the configuration with:
```shell
sudo nginx -t
```

If everything is fine, you can restart Nginx to apply the changes with:
```shell
sudo systemctl restart nginx
```

You can check the Nginx status with:
```shell
sudo systemctl status nginx
```
