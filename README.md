

# News

I'am sorry to announce that Google has not yet approved the **public use** of the API by this APP. They simply gave no reason:

![api usage not approved](/docs/google-unauthorization.png)

However, you can install this application on your own server.

Please don't use the public service until new advice.


# Public Service

This application has been deployed as a public online service on Internet

https://ftpdrive.luisf3908.org/


# Getting started

1. Register to https://ftpdrive.luis3908.org/
2. Authorize access to google-drive
3. Create an FTP user
4. Open your FTP client and connect to host "ftpdrive.luis3908.com.org" and port "50000"
5. Enjoy :)

You can replace *ftpdrive.luis3908.org* with your own domain.


# FTP / FTPs

The google-drive-ftp-adapter-online service offers FTP and FTPs as well.
* FTPs is the recommended connection protocol (encrypted data transfers)
* FTP is an insecure protocol. Use it at your own risk


# Stack

Application is a SBA (built on top of Spring Boot)

* Spring Boot
* Apache Mina
* Google Drive API v3
* Google Drive FTP Adapter (https://github.com/luis3908/google-drive-ftp-adapter)
* spring-session
* spring-security
* spring-data
* Thymeleaf / layouts
* Material getmdl
* Java 8


# Features

* HTTPs endpoint
* FTP / FTPs (FTP over TLS)
* Users Login / Registration
* Google Sign-In
* Thymeleaf layouts
* Error dialog


# Contributions

If you want to contribute to improve the app, send me an email in order to coordinate efforts


# License

This project is licensed under Apache License v2.  It means you can do whatever you want with the source code, while keeping this file and the authoring comments in the code. 


# Build

In order to build it, just git clone the project and run:

    git clone https://github.com/luis3908/google-drive-ftp-adapter-online
    cd google-drive-ftp-adapter-online
    mvn spring-boot:run 


Application is packaged with maven. If you want to make a jar with all dependencies just run:

    mvn clean package
       

# Release

Steps to configure application to put in a production environment.

## Configure an HTTPs certificate

### http certificate (signed certificate)

The easiest way is to request it to https://letsencrypt.org/

    openssl pkcs12 -export -in ftpdrive_luis3908_org.crt -inkey ftpdrive_luis3908_org.key  -name "ftpdrive" -out keystore.p12

### Configure https certificate (self signed certificate)

If you prefer you can instead create a self signed certificate

    keytool -genkey -alias ftpdrive -storetype PKCS12 -keyalg RSA -keysize 2048 -keystore keystore.p12 -validity 3650

## Apache Proxy configuration (optional)

Install necessary modules

    sudo a2enmod proxy
    sudo a2enmod proxy_http

If you want to proxy your service and you are using Apache, you can add this to your /etc/apache2/sites-enabled:

    <IfModule mod_ssl.c>
    	<VirtualHost _default_:443>
    
    		# Preserve "Host" header
    		ProxyPreserveHost On
    
    		# Route all traffic to backend
           	ProxyPass        "/" "https://localhost:1821/"
            ProxyPassReverse "/" "https://localhost:1821/"
    
    		# The name of this virtual host
            ServerName ftpdrive.luis3908.org
    
    		# Custom log files
    		ErrorLog ${APACHE_LOG_DIR}/ftpdrive.error.log
            CustomLog ${APACHE_LOG_DIR}/ftpdrive.access.log combined
    
    		# Enable https on this gateway
    		SSLEngine on
    
    		# Allow backends in https
    		SSLProxyEngine on
    
    		# Uncomment to allow self-signed certificates from backend
    		#SSLProxyVerify none
    		#SSLProxyCheckPeerCN off
    		#SSLProxyCheckPeerName off
    
    		# https certificates
    		SSLCertificateFile      /etc/ssl/certs/ftpdrive_luis3908_org.crt
            SSLCertificateKeyFile /etc/ssl/private/ftpdrive_luis3908_org.key
            
    	</VirtualHost>
    
    </IfModule>


Check syntax and restart apache server

     $ apache2ctl configtest
     $ systemctl restart apache2


## Running process in background (linux)

Once you have packaged the application into a jar file, to run the process you can do:

    $ java -Dspring.profiles.active=production -jar google-drive-ftp-adapter-1.0.0.jar --password --ftps
    $ <ctrl>-z
    $ disown -h %1
    $ bg 1
    $ logout

# Disclaimer

The google-drive-ftp-adapter-online service is secured from top to bottom:
* The service is served by and HTTPs connection. All data entered in the website is secure
* The database used by the application is protected by user and password
* The user passwords are encrypted (bcrypt)
* There is no sensitive information in clear text in the machine

However, use the online service at your own risk.


