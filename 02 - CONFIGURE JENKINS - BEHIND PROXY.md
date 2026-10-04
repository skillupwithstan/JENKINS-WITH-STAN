# 2. How to Configure and Run Jenkins Behind Apache Reverse Proxy?

Installing Apache
Install Apache from Repo

```bash
sudo apt-get update
sudo apt-get install apache2 -y
```
Enable proxy, proxy_http, headers module

```bash
sudo a2enmod proxy
sudo a2enmod proxy_http
sudo a2enmod headers
```
Edit Apache Configuration file
```bash
cd /etc/apache2/sites-available/
sudo vim jenkins.conf
```
Then, In the file enter the following code snippet to make the Apache works for Jenkins. Then, In this ServerName should be your domain name, ProxyPass should point your localhost point to Jenkins (Port 8080) and ProxyPassReverse should be added for both localhost address and Domain address. In the <proxy> block, we need to give access to the apache to handle the Jenkins.

```bash
<Virtualhost *:80>
    ServerName        your-domain-name.com
    ProxyRequests     Off
    ProxyPreserveHost On
    AllowEncodedSlashes NoDecode
 
    <Proxy http://localhost:8080/*>
      Order deny,allow
      Allow from all
    </Proxy>
 
    ProxyPass         /  http://localhost:8080/ nocanon
    ProxyPassReverse  /  http://localhost:8080/
    ProxyPassReverse  /  http://your-domain-name.com/
</Virtualhost>
```
Enable and Restart Jenkins
```bash
sudo a2ensite jenkins
sudo systemctl restart apache2
sudo systemctl restart jenkins
```
Configuring Firewall
```bash
sudo ufw allow ssh
sudo ufw allow http
sudo ufw allow https
```
Now, enable firewall by passing following command.
```bash
ufw enable
```
That’s all. Now on, your Jenkins server will run behind the Apache’s Reverse Proxy.

