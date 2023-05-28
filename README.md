# Steampipe Setup
Repo for Steampipe setup

## Steps Install AWS Cli
curl https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip -o awscliv2.zip
sudo apt install unzip
unzip awscliv2.zip
sudo ./aws/install
### Configure AWS 
aws configure

## Install Steampipe
sudo /bin/sh -c "$(curl -fsSL https://raw.githubusercontent.com/turbot/steampipe/main/install.sh)"
steampipe -v
steampipe plugin install steampipe
steampipe plugin install aws

### Get default dashboards
git clone https://github.com/turbot/steampipe-mod-aws-insights.git
cd steampipe-mod-aws-insights
steampipe dashboard

## Install & Configure nginx
sudo apt update
sudo apt install nginx -y
sudo nano /etc/nginx/sites-available/proxy.conf
```
log_format custom_log '"Request: $request\n Status: $status\n Request_URI: $request_uri\n Host: $host\n Client_IP: $remote_addr\n Proxy_IP(s): $proxy_add_x_forwarded_for\n Proxy_Hostname: $proxy_host\n Real_IP: $http_x_real_ip\n User_Client: $http_user_agent"';
  server {
    listen 80;
    server_name 54.235.57.47;   
    access_log /var/log/nginx/custom-access-logs.log custom_log;

    location / {
        proxy_pass http://127.0.0.1:9194;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }
  }
```
sudo ln -s /etc/nginx/sites-available/proxy.conf /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
