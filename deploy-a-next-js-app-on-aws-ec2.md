# Deploy a NextJS App on AWS EC2

## Install and Configure Apache

```commandline
sudo apt update
sudo apt install apache2
sudo systemctl start apache2
sudo systemctl enable apache2
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
    DocumentRoot /var/www/html

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
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

```commandline
sudo chown -R $USER /var/www
```

Use Git to clone

```commandline
npm install
```


Inside the project:
```commandline
npx forever start -c "npm start" ./
```

```commandline
sudo /opt/bitnami/ctlscript.sh restart apache
```

## Gotchas

### Add Environment Variables Variables

```commandline
sudo nano .env
```

### Make Changes

Pull, install, and:
```commandline
npx forever restart -c "npm start" ./
```