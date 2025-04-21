# 🗄️ MongoDB 7.0 Installation Guide

## 📋 Background Information
* We're using **MongoDB version 7.0** as decided
* This guide helps you install and set up MongoDB on your server

## 🔧 Step 1: Create the MongoDB Repository File
Create a new file for MongoDB package information:
```
vim /etc/yum.repos.d/mongo.repo
```

Add these lines to the file:
```
[mongodb-org-7.0]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/amazon/2023/mongodb-org/7.0/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://pgp.mongodb.com/server-7.0.asc
```

## 📦 Step 2: Install MongoDB
Run this command to install MongoDB:
```
yum install mongodb-org -y 
```

## 🚀 Step 3: Start & Enable MongoDB Service
Set MongoDB to start automatically when the server boots:
```
systemctl enable mongod
```

Start the MongoDB service right now:
```
systemctl start mongod
```

## 🔒 Step 4: Configure Remote Access
By default, MongoDB only listens to the local computer (127.0.0.1). We need to change this so other servers can connect to it.

Edit the configuration file:
```
vim /etc/mongod.conf
```

Find where it says `bindIp: 127.0.0.1` and change it to `bindIp: 0.0.0.0` (this allows connections from any IP address)

## 🔄 Step 5: Restart MongoDB
Apply the changes by restarting MongoDB:
```
systemctl restart mongod
```

## ✅ All Done!
Your MongoDB server is now installed and configured for remote access.