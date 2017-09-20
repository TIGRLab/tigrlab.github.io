---
layout: article
title: "Setting up web-facing xnat with nginx and https"
categories: guides
excerpt: "Be safe out there."
ads: false
share: false
toc: true
image:
  feature: decor.subway.jpg
  teaser: cover.hpc2015.jpg
---

XNAT is likely the best current database option for neuroimaging data, but the official documentation on it is lacking and potentially insecure. This guide will configure a [XNAT](https://www.xnat.org/) server, which uses [tomcat](http://tomcat.apache.org/) internally, to run via a web-facing [nginx](https://www.nginx.com/resources/wiki/#) proxy server. This will be configured to run using https with [certbot](https://certbot.eff.org/), which auto-manages [let's encrypt](https://letsencrypt.org/) certificates. This guide was written with Ubuntu (14.04) in mind.

XNAT Config
-----------

We built our XNAT instance on top of the [XNAT Vagrant](https://bitbucket.org/nrg/xnat_vagrant_1_6dev) project. This project installs and configures nginx, tomcat, postgresql, and java 7. We aren't actually using Vagrant for anything, we just took advantage of the provision script provided by this project.

{% highlight bash %}

# install mercurial, and postfix (mail) for later...
sudo apt-get install mercurial postfix -y
hg clone https://bitbucket.org/nrg/xnat_vagrant_1_6dev xnat
cd xnat/

{% endhighlight %}

The `vars.sh` script needs to be filled in with the network domain and host name. Then, the install process was:

{% highlight bash %}

sed -i '2 i set -e\nset -x' provision.sh
sed -i "s#/vagrant#$PWD#g" provision.sh # correct paths
sed -i "s/apt-get install/apt-get install -y/g" provision.sh # make all apt-get calls install unattended
sed -i "s/mailserver.host=.*/mailserver.host=localhost/g" build.properties # set the mailserver to localhost
sudo bash provision.sh # install

{% endhighlight %}

This will unpack xnat in `/data/xnat/src/${version_number}`. All of xnat's processes will be run as the user `xnat`. You can edit many XNAT install settings in `/data/xnat/src/${version_number}/build.properties`. [Info here](https://wiki.xnat.org/display/XNAT/Build+Properties).

Afterward, deploy XNAT:

{% highlight bash %}

sudo su xnat -c "source ~/.bashrc && bin/update.sh -Ddeploy=true"
sudo service tomcat7 start

{% endhighlight %}

XNAT should be configured, and accessible through tomcat on your local machine. NB: XNAT uses port 8080 by default.

certbot config
--------------

Certbot is a script that generates a let's encrypt certificate for your server, and also makes it trivial to auto-renew. Let's say we want to install this in the home folder of a `localadmin` user:

{% highlight bash %}

cd /home/localadmin
wget https://dl.eff.org/certbot-auto
chmod a+x certbot-auto
/home/localadmin/certbot-auto certonly

{% endhighlight %}

Select the 'standalone' option. This will generate a public and private certificate on your machine in `/etc/letsencrypt`:

```
/etc/letsencrypt/live/xnat.yoursite.com/fullchain.pem
/etc/letsencrypt/live/xnat.yoursite.com/privkey.pem
```

The last thing we need to do is write a script that renews these certificates. We haven't configured nginx yet, but renewing with certbot requires the server be taken offline briefly. Therefore the script will look something like:

{% highlight bash %}
#!/bin/bash

# shut down nginx, renew certs, start nginx
nginx -s stop
/home/localadmin/certbot/certbot-auto renew
nginx

# keeps track of whether updates are running correctly
timestamp=$(date)
echo "last run on ${timestamp}" > /home/localadmin/certbot/last-run.log

{% endhighlight %}

nginx config
------------

The next step is to configure xginx to proxy requests on port 443 (https) to tomcat at port 8080.

This is done via the configuration file found here: `/etc/nginx/sites-available/srv-web`.

Briefly, a nginx proxy configuration requires a server block, which configures connections, and a location block, which points to the specific tomcat instance we want the web to connect to.

The following configuration sets up a ssl listener on port 443 (https) with the certificates we just generated. This is then associated with the tomcat server we have running on port 8080.

We also set up a second server (listener) on port 80 (http) which simply redirects the user to port 443. This ensures all connection attempts are using https.

```
server {
    listen 443 ssl;
    server_name xnat.yoursite.com;

    ssl on;
    ssl_session_cache  builtin:1000  shared:SSL:10m;
    ssl_protocols  TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers HIGH:!aNULL:!eNULL:!EXPORT:!CAMELLIA:!DES:!MD5:!PSK:!RC4;
    ssl_prefer_server_ciphers on;
    ssl_certificate /etc/letsencrypt/live/xnat.yoursite.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/xnat.yoursite.com/privkey.pem;

    location / {
        root /var/lib/tomcat7/webapps/xnat;
        proxy_pass                          http://xnat.yoursite.com:8080;
        proxy_redirect                      off;
        proxy_set_header Host               $host;
        proxy_set_header X-Real-IP          $remote_addr;
        proxy_set_header X-Forwarded-Host   $host;
        proxy_set_header X-Forwarded-Server $host;
        proxy_set_header X-Forwarded-For    $proxy_add_x_forwarded_for;
        proxy_connect_timeout               150;
        proxy_send_timeout                  100;
        proxy_read_timeout                  100;
        proxy_buffers                       4 32k;
        client_max_body_size                0;
        client_body_buffer_size             128k;
    }

    access_log /var/log/nginx/srv-web.access.log;
    error_log /var/log/nginx/srv-web.error.log;
}

server {
        listen 80 default_server;
        listen [::]:80 default_server;
        server_name xnat.yoursite.com;
        return 301 https://$server_name$request_uri;
}

```

Run `nginx -s reload` to load these settings.


The majority of the location blocks of this configuration file was taken directly from [XNAT's guide](https://wiki.xnat.org/display/XW2/Step+3+of+8+Configure+Tomcat+for+XNAT) on setting up a regular HTTP server.

tomcat config
-------------

The final step is to configure a 'connector' in tomcat that the urls it is formatting should be https. Internally, tomcat will not communicate using encryption, but needs to know that the nginx proxy is. Basically, it needs to be told explicitly that it's being proxied through a SSL port (443). Below is the relevant snippit of `/var/lib/tomcat7/conf/server.xml` (otherwise known as `${CATALINA_HOME}/conf/server.xml`). [See here for more](http://webapp.org.ua/sysadmin/setting-up-nginx-ssl-reverse-proxy-for-tomcat/).

```
<Server port="8005" shutdown="SHUTDOWN">

  # removed for clairty

  <Service name="Catalina">

    <Connector port="8080"
               protocol="HTTP/1.1"
               maxThreads="150"
               connectionTimeout="20000"
               proxyName="xnat.yoursite.com"
               proxyPort="443"
               scheme="https"
               clientAuth="false"
               URIEncoding="UTF-8"/>

  # removed for clairty

  </Service>
</Server>

```

The key lines are the `proxyName` and `proxyPort` entries.

Finally, restart tomcat: `sudo service tomcat7 restart`.

overview
--------

This is what we just did:

+ install `XNAT` using [XNAT Vagrant](https://bitbucket.org/nrg/xnat_vagrant_1_6dev), which gets XNAT and tomcat going locally on port 8080.
+ set up `certbot` to generate and maintain our SSL certificates for HTTPS.
+ set up `nginx` to listen for secure HTTPS requests on port 443, using our certificates. These requests are routed to the insecure tomcat server at port 8080.
+ set up `tomcat` to listen for secure SSL data on port 8080 that originates from port 443.

Now XNAT is somewhat more secure, sitting behind nginx and SSL.

