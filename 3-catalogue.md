# 🛍️ Catalogue Service Setup Guide

## 📋 Introduction
The Catalogue service is responsible for displaying product listings in our RobotShop application. It's built with NodeJS.

## 🔄 Step 1: Install NodeJS 18
Developer confirmed we need NodeJS version 18+ for this application to work properly.

Now install NodeJS:
```
yum install nodejs -y
```

## 👤 Step 2: Create Application User
Create a dedicated user to run our application:
```
useradd robotshop
```
> 💡 This 'robotshop' user is a service account that runs the application - we don't use it to log into the server.

## 📁 Step 3: Set Up Application Directory
Create a directory for our application:
```
mkdir /app
```

## 📦 Step 4: Download and Extract Application Code
Download the application package:
```
curl -o /tmp/catalogue.zip https://robot-shop-project.s3.us-east-1.amazonaws.com/robot-shop-catalogue.zip
```

Go to the app directory and unzip the package:
```
cd /app 
unzip /tmp/catalogue.zip
```

## 🔧 Step 5: Install Application Dependencies
Install all the required libraries for our application:
```
cd /app
npm install 
```

## ⚙️ Step 6: Create System Service
Create a service file so the system can manage our application:
```
vim /etc/systemd/system/catalogue.service
```

Add these contents to the file:
```
[Unit]
Description = Catalogue Service
[Service]
User=robotshop
Environment=MONGO=true
Environment=MONGO_URL="mongodb://<MONGODB-SERVER-IPADDRESS>:27017/catalogue"
ExecStart=/bin/node /app/server.js
SyslogIdentifier=catalogue
[Install]
WantedBy=multi-user.target
```
> ⚠️ Remember to replace `<MONGODB-SERVER-IPADDRESS>` with your actual MongoDB server IP address!

## 🚀 Step 7: Start the Service
Load the new service configuration:
```
systemctl daemon-reload
```

Enable and start the service:
```
systemctl enable catalogue
systemctl start catalogue
```

## 🗃️ Step 8: Load Database Schema
To make the application fully functional, we need to load data into MongoDB.

First, create the MongoDB repo file:
```
vim /etc/yum.repos.d/mongo.repo
```

Add these contents to the file:
```
[mongodb-org-7.0]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/amazon/2023/mongodb-org/7.0/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://pgp.mongodb.com/server-7.0.asc

```

Install the MongoDB client:
```
sudo yum clean all

sudo yum makecache

sudo yum install -y mongodb-mongosh
```

Load the schema into MongoDB:
```
mongosh --host MONGODB-SERVER-IPADDRESS </app/schema/catalogue.js
```
> ⚠️ Replace `MONGODB-SERVER-IPADDRESS` with your actual MongoDB server IP address!

## 📝 Final Note
Don't forget to update the catalogue server IP address in the frontend configuration file: `/etc/nginx/default.d/robotshop.conf`

## ✅ All Done!
Your Catalogue service is now set up and ready to serve product listings!