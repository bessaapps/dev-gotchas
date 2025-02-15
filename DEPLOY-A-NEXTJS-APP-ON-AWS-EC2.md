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
sudo cp /etc/apache2/sites-avialble/000-default.conf /etc/apache2/sites-avialble/<domain>.conf
```

Example:
```text
<VirtualHost _default_:80>
    ServerAlias *
    DocumentRoot "/opt/bitnami/projects/myapp"
    <Directory "/opt/bitnami/projects/myapp">
        Require all granted
    </Directory>
    ProxyPass / http://localhost:3000/
    ProxyPassReverse / http://localhost:3000/
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