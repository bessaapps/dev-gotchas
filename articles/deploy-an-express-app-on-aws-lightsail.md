# Deploy an Express App on AWS Lightsail

Choose the Node.js blueprint when creating a Lightsail instance and connect.

```
sudo mkdir /opt/bitnami/projects
sudo chown $USER /opt/bitnami/projects
```
Clone Project

```
npm install
```

Install forever, if it's not already installed.

Inside the project:
```
npx forever start -c "npm start" ./
```

```
sudo cp /opt/bitnami/apache/conf/vhosts/sample-vhost.conf.disabled /opt/bitnami/apache/conf/vhosts/sample-vhost.conf
sudo cp /opt/bitnami/apache/conf/vhosts/sample-https-vhost.conf.disabled /opt/bitnami/apache/conf/vhosts/sample-https-vhost.conf
```

Nano:
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

```text
<VirtualHost _default_:443>
    ServerAlias *
    SSLEngine on
    SSLCertificateFile "/opt/bitnami/apache/conf/bitnami/certs/server.crt"
    SSLCertificateKeyFile "/opt/bitnami/apache/conf/bitnami/certs/server.key"
    DocumentRoot "/opt/bitnami/projects/myapp"
    <Directory "/opt/bitnami/projects/myapp">
      Require all granted
    </Directory>
    ProxyPass / http://localhost:3000/
    ProxyPassReverse / http://localhost:3000/
  </VirtualHost>
```

```
sudo /opt/bitnami/ctlscript.sh restart apache
```

## Gotchas

### Add Environment Variables Variables

```
sudo nano .env
```

### Make Changes

Pull, install, and:
```
npx forever restart -c "npm start" ./
```

