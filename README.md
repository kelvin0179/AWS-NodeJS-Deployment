# AWS-NodeJS-Deployment

## Instance Creation

* Create an EC-2 Instance in AWS using Ubuntu 18.04.
* Add Ports of Type SSH , http and https.
* Also Add Custom Ports of the Backend Localhost (Mine is 8080 and 8081).
* Create a new (.pem) file of your instance.

## Elastic IP Address

* The Purpose of Creating this is that everytime we reboot our Instance we are provided with a New IP address for our Site.
* So we create an Elastic IP and Assign it to our Instance Running.

## Start the Server

* In the path where we've downloaded the (.pem) file , open Terminal or Bash 

```bash
chmod 400 <fileName>.pem
ssh -i "<fileName>.pem" ubuntu@ec<Elastic IP>.us-east-2.compute.amazonaws.com
```

* This should start your server.

## Clone Your NodeJS Project

```bash
git clone <link>.git
cd <Cloned FolderName>
```

* After this you have to install all the Dependencies From your package.json file and mongoDB.
* To do that type

```bash
#install node 12.xx
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
sudo apt-get install -y nodejs

#install mongoDB
wget -qO - https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add -
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.4.list
sudo apt-get update
sudo apt-get install -y mongodb-org
sudo systemctl start mongod
```

* We also need to restart mongod when the server is rebooted 

```bash
sudo systemctl status mongod
sudo systemctl enable mongod
```

## Set Up PM2 Process Manager

```bash
sudo npm i pm2 -g
pm2 start <FileName>.js (usually server.js)

# Other pm2 commands
pm2 show app
pm2 status
pm2 restart app
pm2 stop app
pm2 logs (Show log stream)
pm2 flush (Clear logs)

# To make sure app starts when reboot
pm2 startup ubuntu
```

* The server should be up and running to try to access it through your IP and Port.

## Setup UFU Firewall

```bash
sudo ufw enable
sudo ufw allow ssh (Port 22)
sudo ufw allow http (Port 80)
sudo ufw allow https (Port 443)
sudo ufw status
```

## Install NGINX and Configure

```bash
sudo apt install nginx
sudo nano /etc/nginx/sites-available/default
```
* Now delete all the text and type the following.
* Be sure to edit the custom ports and endpoints.
```bash
server {
        root /var/www/html;

        index index.html index.htm index.nginx-debian.html;

        server_name xmeme.harsh-vardhan.codes www.xmeme.harsh-vardhan.codes;

        #For root directory and My Custom port is 8081 here
        location / {
        proxy_pass http://localhost:8081; #whatever port your app runs on
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
        }

        #For End Point memes and port 8081
        location =/memes {
        proxy_pass http://localhost:8081; #whatever port your app runs on
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
        }
        
        #For End Point swagger-ui and port 8080
        location =/swagger-ui/ {
        proxy_pass http://localhost:8080; #whatever port your app ru$
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
	      proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
        }
}
```
* Press `ctrl + x` and then 'y' and then press 'enter`

```bash
# Check NGINX config
sudo nginx -t

# Restart NGINX
sudo service nginx restart
```

* Everytime you edit your NGINX file you need to restart it.



