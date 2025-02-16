# Deploy a NextJS App on AWS EC2

Create an SSH key pair and upload to AWS before generating an EC2. (You'll need both the private and public keys.) Use this key whe setting up your EC2.

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
```

Example Files:
```text
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html/<project>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
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
sudo systemctl restart apache2
```

## Install Node and NPM

```commandline
sudo apt update
sudo apt install nodejs
sudo apt install npm
```

## Install Forever

```commandline
sudo npm install -g forever 
```

## Clone Project

Add your public key to GitLab through the Console.

```commandline
sudo chown -R $USER /var/www
git clone git@gitlab.com:<something>/<something>.git
```



## Start App

Add your variables with:
```commandline
sudo nano .env
```

Inside the project:

```commandline
npm install
npm run build
npx forever start -c "npm start" ./
sudo systemctl restart apache2
```

## Gotchas

### Cloning your Project

Do not use sudo when cloning your project.

### Make Changes

Pull, install, and:
```commandline
npx forever restart -c "npm start" ./
```