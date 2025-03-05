# Deploy an Express.js App on AWS EC2

Be sure to use the desired region.

Create an SSH key pair and upload to AWS before generating an EC2. Use this key whe setting up your EC2.

## Install and Configure Apache

```
sudo apt update
sudo apt install apache2
sudo systemctl start apache2
sudo systemctl enable apache2
sudo a2enmod proxy_http
sudo systemctl restart apache2
```

Modify files in /etc/apache2/sites-available accordingly and enable and restart, if necessary:

```
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/<domain>.conf
sudo cp /etc/apache2/sites-available/default-ssl.conf /etc/apache2/sites-available/<domain>-ssl.conf
sudo rm -R /var/www/html
```

Example File:
```text
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    ServerName <domain>
    ServerAlias <subdomain>.<domain>
    DocumentRoot /var/www/<project>

    ...
    
    ProxyPass / http://localhost:4000/
    ProxyPassReverse / http://localhost:4000/
</VirtualHost>
```

```
sudo a2dissite 000-default
sudo a2ensite <domain>
sudo systemctl reload apache2
```

## Generate an SSL Certificate

Make sure to add the elastic IP address to the DNS records so the app resolves before completing the next steps.

```
sudo apt-get install certbot python3-certbot-apache
sudo certbot --apache
```

## Install Node and NPM

```
sudo apt install nodejs
sudo apt install npm
```

## Install PM2

```
sudo npm install pm2 -g 
```

## Clone Project

```
ssh-keygen
```

Add your public key to GitHub through the Console.

```
sudo chown -R $USER /var/www

git clone git@github.com:<organization>/<project>.git
git fetch
git checkout <branch>
git pull
```

## Start App

Add your variables with:
```
sudo nano .env
```

Inside the project run
```
npm install
pm2 start index.js --name <project> -- --port 4000
```

or, if using Sentry:
```
NODE_OPTIONS="--import ./instrument.js" pm2 start index.js --name <project> -- --port 4000
```

## Gotchas

### Cloning your Project

Do not use sudo when cloning your project.

### Make Changes

Pull, install, and:
```
pm2 restart <project>
```