# Deploy a NextJS App on AWS EC2

Create an SSH key pair and upload to AWS before generating an EC2. Use this key whe setting up your EC2.

## Install and Configure Apache

```commandline
sudo apt update
sudo apt install apache2
sudo systemctl start apache2
sudo systemctl enable apache2
sudo a2enmod proxy_http
sudo systemctl restart apache2
```

Modify files in /etc/apache2/sites-available accordingly and enable and restart, if necessary:

```commandline
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/<domain>.conf
sudo cp /etc/apache2/sites-available/default-ssl.conf /etc/apache2/sites-available/<domain>-ssl.conf
sudo rm -R /var/www/html
```

Example Files:
```text
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/<project>

    ...
    
    ProxyPass / http://localhost:3000/
    ProxyPassReverse / http://localhost:3000/
</VirtualHost>

<VirtualHost *:443>
    ServerAdmin webmaster@localhost

    DocumentRoot /var/www/html

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined

    SSLEngine on

    SSLCertificateFile      /etc/ssl/certs/ssl-cert-snakeoil.pem
    SSLCertificateKeyFile   /etc/ssl/private/ssl-cert-snakeoil.key
    <FilesMatch "\.(?:cgi|shtml|phtml|php)$">
        SSLOptions +StdEnvVars
    </FilesMatch>
    <Directory /usr/lib/cgi-bin>
        SSLOptions +StdEnvVars
    </Directory>
</VirtualHost>
```

```commandline
sudo a2dissite 000-default
sudo a2ensite <domain>
sudo systemctl reload apache2
```

## Generate an SSL Certificate

```commandline
sudo apt update
sudo apt-get install certbot python3-certbot-apache
sudo certbot --apache
```

## Install Node and NPM

```commandline
sudo apt update
sudo apt install nodejs
sudo apt install npm
```

## Install PM2

```commandline
sudo npm install pm2 -g 
```

## Clone Project

```commandline
ssh-keygen
```

Add your public key to GitLab through the Console.

```commandline
sudo chown -R $USER /var/www

git clone git@gitlab.com:<something>/<something>.git
git fetch
git checkout <branch>
git pull
```

## Start App

Add your variables with:
```commandline
sudo nano .env
```

Inside the project run
```commandline
npm install
npm run build
pm2 start npm --name nextjs-app -- run start -- -p 3000
```

## Gotchas

### Cloning your Project

Do not use sudo when cloning your project.

### Make Changes

Pull, install, and:
```commandline
pm2 restart nextjs-app
```