# Configuring a EC2 server with node.js + mongo db

## Install git
```
sudo yum install git
```
## Install node.js
Amazon recommend to use nvm instead of installing node.js directly:
http://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/setting-up-node-on-ec2-instance.html
```
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.32.0/install.sh | bash
```
Close the terminal and open it again and try to run nvm. Then install node:
```
nvm install 6.9.4
```

## Install nodemon
https://nodemon.io/
```
npm install -g nodemon
```
## Install MongoDB
https://docs.mongodb.com/ecosystem/platforms/amazon-ec2/#manually-deploy-mongodb-on-ec2
Update yum with mongo repositories:
```
echo "[mongodb-org-3.2]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/amazon/2013.03/mongodb-org/3.2/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.2.asc" |
  sudo tee -a /etc/yum.repos.d/mongodb-org-3.2.repo
```
Execute mongo installation:
```
sudo yum -y update && sudo yum install -y mongodb-org-server \
    mongodb-org-shell mongodb-org-tools
```
Start mongodb:
```
sudo service mongod start
```
Configure it to start automatically once the instance is initiated:
```
sudo chkconfig mongod on
```
## Port 80
http://stackoverflow.com/questions/16573668/best-practices-when-running-node-js-with-port-80-ubuntu-linode
By default express js runs in the port 3000 and normal users cannot open the 80 port. Se it is necessary to configure the iptables to redirect port 80 to 3000.
```
sudo iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 80 -j REDIRECT --to-port 3000
```
## Mongo remote connection
https://www.mkyong.com/mongodb/mongodb-allow-remote-access/
Review bind_ip configuration.
